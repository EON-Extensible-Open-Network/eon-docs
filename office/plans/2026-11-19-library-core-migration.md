<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Office library-core migration — implementation plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** turn `eon-office-core` from a local service into a library every EON Office
application links, so each application runs on its own.

**Architecture:** applications open `<home>/apps/<id>/data.sqlite` (their documents) and
`<home>/shared.sqlite` (registry, preferences, search index, activity, published data,
audit) through `eon-office-core`; actions between applications are `eon-office://` links,
delivered by starting the target from the registry (single-instance forwards to a running
window); notifications go to the operating system.

**Tech Stack:** Rust 1.98.1 (pinned), rusqlite 0.43 (bundled, WAL), serde_json, sha2,
getrandom; Tauri 2 with `tauri-plugin-single-instance` and `tauri-plugin-notification` in
applications only; jiff in Calendar.

**Spec:** `eon-docs/office/design/2026-11-19-library-core.md`

## Global Constraints

- `eon-office-core` has no binary, no socket, no GUI dependency; library crate only.
- Owner identity, `git commit -s`, no AI attribution in any commit (workspace CLAUDE.md).
- Every application works with nothing else installed; integrations degrade silently.
- Interfaces never receive another application's private data; untrusted text via
  `textContent` only; no `fetch` in any `src/`.
- Turkish folding/sorting rules unchanged; all strings EN + TR.
- New versions: core `0.2.0`, applications `0.2.0`, launcher `0.3.0`; published releases
  stay as history.

## Review Focus

1. **Two applications writing `shared.sqlite` at once** (Notes indexing while Tasks records
   activity) → both succeed; no `database is locked` reaches a person. Test in Task 2.
2. **An application started twice** (double-click, or a link while open) → one window; the
   second start's link is handled by the first. Test in Task 3 (ui-check).
3. **A link to an application that is registered but whose file was moved/deleted** →
   a clear "not available" answer, no crash. Test in Task 2 (`links::open`).
4. **`shared.sqlite` from a newer core** → refused with `incompatible`, the application still
   opens its own store read/write. Test in Task 2.
5. **Preferences changed in the launcher while an application is open** → applied within
   2 s without restart. Test in each application's ui-check (Tasks 3–6).

---

### Task 1: Spec — the shared-folder contract

**Files:**
- Create: `eon-office-spec/docs/shared-store.md`
- Modify: `eon-office-spec/docs/architecture.md`, `docs/protocol.md` (head note: superseded
  by `shared-store.md`, kept for history), `docs/capabilities.md` (declared, shown at
  install, revocable in the registry), `README.md`, `CHANGELOG.md`

**Interfaces:** Produces the table layouts Task 2 implements, verbatim:
- `registry(id TEXT PK, manifest TEXT, path TEXT, registered_at INT, revoked TEXT DEFAULT '[]')`
- `prefs(key TEXT PK, value TEXT)`, `prefs_meta(version INT)` (bumped on every change),
  `themes(id TEXT PK, body TEXT)`
- `search_docs(app, id, title, snippet, visibility, workspace, updated_at, PK(app,id))`,
  `search_terms(term, app, id, weight, PK(term,app,id))`
- `activity(seq INT PK, at INT, app TEXT, item TEXT, title TEXT, kind TEXT)` (≤ 500 rows kept)
- `public(app TEXT, key TEXT, value TEXT, updated_at INT, PK(app,key))`
- `audit(seq, at, actor, action, target, detail, prev, hash)` (unchanged chain)
- `PRAGMA user_version` = 1 for shared.sqlite; data.sqlite keeps `docs`, `conflicts`,
  `settings`, `meta(node)`.
- Links: `eon-office://<app-id>/<item>[?query]`; delivery = start registry `path` +
  `entry.command` with the link as the only argument.

- [ ] **Step 1:** Write `shared-store.md` with the layouts above, the WAL/busy-timeout rule
  (5 s), the visibility rule for search, the activity rule (title, never contents), the
  public-data rule (summaries only; owner app writes, any app reads), and link delivery.
- [ ] **Step 2:** `npm run validate` → "All examples behave as specified."
- [ ] **Step 3:** Commit `docs(spec): the shared-folder contract replaces the core protocol`.

### Task 2: Core — library API

**Files:**
- Create: `src/home.rs`, `src/shared.rs`, `src/context.rs`, `src/links.rs`
- Modify: `src/lib.rs`, `src/db.rs` (two migration sets), `src/storage.rs` (functions take
  `&Connection` as now), `src/search.rs`, `src/audit.rs`, `src/settings.rs`,
  `Cargo.toml` (no `[[bin]]`; version `0.2.0`), `README.md`, `docs/architecture.md`,
  `SECURITY.md`, `CHANGELOG.md`, `.github/workflows/ci.yml` (drop protocol tests and
  loopback invariant; add "no binary target" invariant)
- Delete: `src/server.rs`, `src/apps.rs`, `src/notify.rs`, `src/schedule.rs`,
  `src/main.rs`, `tests/protocol.rs`, `tests/support/`
- Test: `tests/library.rs`

**Interfaces (produces):**
- `Home::open(path: &Path) -> Result<Home>`; `Home::default_location() -> Option<PathBuf>`;
  `Home::path(&self) -> &Path`
- `Home::app(&self, manifest: &Manifest, path: Option<&Path>) -> Result<AppContext>` —
  registers (upsert, refuses a lower version), opens the store
- `AppContext::id(&self) -> &str`; `allows(&self, cap: &Capability) -> bool` (declared and
  not revoked)
- `AppContext::put(&mut self, collection: &str, id: &str, doc: &Value, rev: Option<u64>) -> Result<Value>`,
  `get(&self, collection, id) -> Result<Value>` (`Null` if absent),
  `delete(&mut self, collection, id, rev: Option<u64>) -> Result<Value>`,
  `list(&self, collection, after: Option<&str>, limit: i64) -> Result<Value>`,
  `conflicts(&self, limit) -> Result<Value>`, `resolve(&self, conflict: i64) -> Result<()>`,
  `export(&self, since: Option<&str>, limit) -> Result<Value>`, `import(&mut self, changes: &[Value]) -> Result<Value>`
- `AppContext::index(&self, id, title, text, shared: bool) -> Result<()>` (needs `search.index`),
  `unindex(&self, id) -> Result<()>`, `search(&self, query, limit) -> Result<Value>` (needs `search.query`)
- `AppContext::activity(&self, item, title, kind) -> Result<()>` (needs `events.publish`)
- `AppContext::publish(&self, key, value: &Value) -> Result<()>`, `unpublish(&self, key)`,
  `public_of(&self, app: &str) -> Result<Vec<(String, Value)>>`
- `AppContext::settings_get/settings_set(&self, key, value)`
- `Shared::open(home: &Path) -> Result<Shared>`; `apps() -> Result<Vec<Value>>`,
  `revoke(app, cap)`, `grant(app, cap)`, `unregister(app)`, `prefs() -> Result<Value>`,
  `set_pref(key, &Value)`, `prefs_version() -> Result<i64>`, `themes()`, `put_theme()`,
  `recent(limit) -> Result<Value>`, `query(viewer: Option<&str>, q, limit)`,
  `audit_list(after, limit)`, `audit_verify()`
- `links::parse(&str) -> Option<Link>`; `links::open(home: &Path, link: &str) -> Result<Value>`
  (`{opened:true}` or `{opened:false, reason: "not_installed"|"missing_file"}`)
- Errors keep `Kind` and stable string codes.

- [ ] **Step 1: Write the failing tests** in `tests/library.rs`:
  `an_application_registers_and_reopens_its_store`,
  `two_applications_cannot_read_each_others_documents`,
  `search_shows_only_what_was_shared` (as in the old protocol test),
  `a_capability_not_declared_or_revoked_is_refused` (`Kind::Forbidden`, data.capability),
  `two_threads_with_their_own_connections_write_shared_at_once` (200 writes each, no error),
  `activity_keeps_titles_and_the_last_500`,
  `published_summaries_are_read_by_another_app`,
  `a_newer_shared_schema_is_refused_but_the_store_opens`,
  `links_open_reports_missing_files`, `prefs_version_moves_on_every_change`,
  `two_devices_sync_through_export_and_import` (moved from the protocol suite).
- [ ] **Step 2:** `cargo test --test library` → fails to compile (no `Home`).
- [ ] **Step 3:** Implement the interfaces above; delete the server files; keep unit tests of
  kept modules green.
- [ ] **Step 4:** `cargo fmt --all --check && cargo clippy --all-targets -- -D warnings && cargo test`
  → all pass; `EON_OFFICE_SPEC_DIR=../eon-office-spec cargo test --test spec_examples -- --ignored` → pass.
- [ ] **Step 5:** Commit `feat(core)!: a library, not a service (0.2.0)`; push; CI green.

### Task 3: Notes on the library

**Files:** Modify `eon-office-notes/src-tauri/Cargo.toml` (replace `eon-office-sdk` with
`eon-office-core` pinned to Task 2's commit; add `tauri-plugin-single-instance`),
`src-tauri/src/notes.rs` (`Client` → `AppContext`), `src-tauri/src/main.rs` (open `Home`,
`home.app(manifest, exe_dir)`; argv link handling; single-instance forwarding; 2 s prefs
poll emitting `prefs-changed`), `src-tauri/capabilities/default.json` (plugin permissions),
`tests/notes.rs` (no core binary: temp home + library), `tools/ui-check.mjs` (start Notes
directly with `EON_OFFICE_HOME`; prefs via a tiny Node writer is not allowed — use a second
debug command `--set-pref key value` on Notes itself), CI (no core checkout), version `0.2.0`.

- [ ] Steps: failing test `a_link_argument_opens_the_note` (parse + handler returns
  `{open: id}`) → implement → `cargo test -- --include-ignored` and `node tools/ui-check.mjs
  --notes …` (all checks, plus: second start with a link focuses the first window) → commit
  `feat(notes)!: stand alone on the core library (0.2.0)` → CI green → tag `v0.2.0`.

### Task 4: Tasks on the library

As Task 3, plus: reminders become an in-app timer thread that posts an OS notification
(`tauri-plugin-notification`) and, at start, catches up on reminders whose time passed
while closed (one notification each, not more). Test `missed_reminders_fire_once_at_start`.
Tag `v0.2.0`.

### Task 5: Contacts on the library

As Task 3, plus: every save publishes `contact:<id>` → `{name, org, emails, phones}` to the
public table and deletes unpublish it; `lookup` reads it for other applications. Test
`another_app_reads_published_summaries_not_notes`. Tag `v0.2.0` (first release).

### Task 6: Calendar on the library

Finish `eon-office-calendar` (modules `rrule`, `time`, `ics`, `events` already written)
directly on `AppContext`; reminders as in Task 4; attendee lookup from Contacts' public
data when present. Window, tests, docs, CI, release `v0.1.0`, marketplace entry.

### Task 7: Launcher as the optional hub

Replace `host.rs`'s SDK client with `Shared` (apps from the registry; grant/revoke = the
registry's `revoked`; preferences; recent = `Shared::recent`; marketplace install =
download, verify, place under `<home>/installed/<id>/`, then start the application once so
it registers). Remove core spawning. `eon-office://` handling via `links::open`. ui-check
updated. Version `0.3.0`.

### Task 8: SDK, marketplace, docs

- SDK: remove the client and the reference integration; becomes `eon-office-sdk` tooling
  (`new <app-id>` scaffolds an application from the Notes layout; `check` validates a
  manifest). MIT. Version `0.2.0`.
- Marketplace: README and spec text — installs need no service. Add Tasks, Contacts,
  Calendar entries from their releases (`entry_from_release.py` pattern).
- `eon-docs/office/status.md`, `roadmap.md`: the new architecture, versions, tests.

## Self-review

- Spec coverage: library core (T2), standalone apps (T3–T6), OS notifications (T4–T6),
  links (T1, T2, T3), published data (T5), optional launcher (T7), SDK/marketplace (T8),
  Stream naming (T2 removes the binary; repos keep `eon-office-<app>`). Optional background
  helper: deliberately absent (design: later decision).
- Types: `AppContext`, `Shared`, `Home`, `links::open` used consistently in T3–T7.
