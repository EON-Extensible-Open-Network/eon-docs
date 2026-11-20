<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Office — project status

**As of 19 November 2026.** Updated in the same pull request as the work it describes.
Plan: [madde 43](../plan/eon-plan.md). Order of work: [roadmap.md](roadmap.md).

## Completed

| Step (directive, Phase 9) | Result |
|---|---|
| 1. Node.js skill errors | Fixed and verified on this machine: stale terminal `PATH`, a missing `%APPDATA%\npm`, missing global packages for the `pptx`/`docx` skills (installed, `NODE_PATH` set), two defects in `claude-mem`'s `version-bump` script. Side effect worth recording: `npm run validate` now runs locally, which EON's own notes said it could not. Open: `playwright` for one design-audit script was deliberately not installed globally. |
| 2. Existing repositories inspected | Conventions adopted unchanged: Rust with a pinned 1.98.1 toolchain, `cargo deny`, REUSE, DCO, Conventional Commits, SPDX headers, the GPL + EON Module ABI Exception pattern, Apache-2.0 spec / MIT SDK, `.gitattributes` LF, every dependency justified in `Cargo.toml`. EON Office depends on no EON Stream code. |
| 3–4. Core designed, implemented, tested | `eon-office-core` 0.1.0 — see below. |
| 5. Roadmap | [roadmap.md](roadmap.md) and the Microsoft audit [microsoft-parity.md](microsoft-parity.md), integrated. |
| Plan | madde 43 written with its costs; pull request eon-docs#2. |

## Repository status

| Repository | Version | Published | CI | Notes |
|---|---|---|---|---|
| `eon-office-spec` | contracts v0: the shared-folder contract | ✓ public | ✓ green | replaced the core protocol (19 Nov) |
| `eon-office-core` | 0.2.0 — a library, not a service | ✓ public | ✓ green (Windows, macOS, Linux; deny; REUSE; invariants) | [design](design/2026-11-19-library-core.md) |
| `eon-office-sdk` | 0.1.0, release `v0.1.0` | ✓ public | ✓ green (reference integration on three OSes) | release ships EON Hello and EON Caller for Windows, Linux, macOS |
| `eon-office-launcher` | 0.2.0, releases `v0.1.0`, `v0.2.0` | ✓ public | ✓ green (three OSes; 30 window checks on Windows) | release downloaded, checksums verified, `--selftest` passed on the published binaries |
| `eon-office-marketplace` | 0.1.0 (index v0) | ✓ public | ✓ green | lists Hello, Caller, Notes (0.1.0, 0.2.0), Tasks 0.2.0, Contacts 0.2.0, Calendar 0.1.0 (preview, unsigned); artefacts verified by download in CI and weekly |
| `eon-office-notes` | 0.2.0, releases `v0.1.0`, `v0.2.0` | ✓ public | ✓ green (three OSes; window checks on Windows) | stands alone; in the marketplace |
| `eon-office-tasks` | 0.2.0, release `v0.2.0` | ✓ public | ✓ green (three OSes; 22 window checks on Windows) | stands alone. `v0.1.0` was tagged but never published: GitHub never gave its macOS job a runner |
| `eon-office-contacts` | 0.2.0, release `v0.2.0` | ✓ public | ✓ green (three OSes; 20 window checks on Windows) | stands alone; CardDAV moved to 0.2 |
| `eon-office-calendar` | 0.1.0, release `v0.1.0` | ✓ public (created 19 Nov) | ✓ green (three OSes; 31 window checks on Windows) | stands alone; CalDAV moved to 0.2 |
| `eon-docs` | plan v0.8 | branch `docs/madde-43-office`, PR #2 | — | awaiting the owner's merge |
| everything else in the roadmap | — | not created | — | by rule: no empty repositories (madde 43d) |

## Test status

| Suite | Count | What it proves |
|---|---|---|
| core unit tests | 46 | clock ordering, Turkish folding, migrations, manifest and semver rules, audit tamper detection, RBAC table and inheritance, revision conflicts, two-device sync convergence with the loser kept, search visibility, rate limit, job coalescing, contrast ratios |
| core protocol tests | 16 | the real binary over a bare socket: authentication, capabilities checked per call, storage isolation, declared events, calls between apps, a provider crashing mid-call, notifications, search, jobs, preferences, supervisor restarts and give-up, forced stop, sync between two cores, directory, oversized messages, uninstall, backup/restore and one-core-per-home |
| core ↔ spec agreement | 2 | the spec's 25 examples mean the same to the Rust readers as to the JSON Schemas |
| spec validation | 25 examples | 8 valid accepted, 17 invalid rejected |
| core protocol tests (since) | 17 | + a relative `entry.command` found with the platform suffix |
| launcher | 5 unit + 1 host-against-core + `--selftest` + 30 window checks | review, install, permissions, notifications, keyboard, search, open, preferences, activity, start, remove |
| marketplace | 12 unit + 2 index | rules, deterministic build, install planning, signature verification; artefacts downloaded in CI |
| SDK | 3 unit + 1 reference + 1 doctest | an app launched by the core, a second app reaching it only through the core, and graceful behaviour with it absent |
| Mutation check (manual) | — | disabling capability enforcement in the core fails 3 protocol tests |

## Technical decisions taken (and where they are argued)

- ~~The core is a local service~~ — replaced 19 Nov: **the core is a library every
  application links, and every application stands alone**; shared needs go through files in
  a known folder and through the operating system — [design](design/2026-11-19-library-core.md).
  The loopback JSON-RPC protocol went with the service.
- **SQLite**, one connection, WAL; migrations refuse newer schemas — same.
- **Hybrid logical clocks, last-writer-wins per document, losers kept** — `eon-office-spec/docs/sync.md`.
- **Home in the per-user data directory**, unlike EON Stream's beside-the-executable rule, because many programs must find one core — `eon-office-core/src/lib.rs`.
- **Coarse, owner-granted capabilities** — `eon-office-spec/docs/capabilities.md`.
- Product merges and splits (BI → Analytics, Admin Center → Admin, Device Manager → Endpoint, Defender → Security, no Identity product, no Cloud PC) — `roadmap.md` §3.

## Current work

Done since the first status: **EON Launcher 0.1** (Gate 0's first half) and the
**marketplace** (index format, tooling, signature check, compatibility, first entries).
Verified end to end with published files only: the released core read the live
`index.json`, downloaded EON Hello, checked its size and SHA-256, installed and launched it;
notifications, Turkish search and a call to `greet` worked.

Also done: install from the marketplace in EON Launcher (`0.2.0`, 36 window checks
including an install from the live index); the `activity` event and `eon-office://` deep
links in the spec; design notes for encryption at rest (core) and the sync relay (spec);
the engine spike ([research/engine-spike.md](research/engine-spike.md)).

**Gate 0 → 1 status:** a person can install, grant, run and remove an application without a
terminal — met. The madde 35 keyboard and screen-reader checklist: keyboard and the
machine-checkable half are met and tested; **a session with a real screen reader (NVDA or
Narrator) is still owed** and is the one item that needs a person.

**Wave 1:** EON Notes, EON Tasks and EON Contacts each have a first version with the
roadmap's acceptance criteria tested on the core library and through the real window
(Notes: a 1,000-note vault imports in about 3 s with every link to an existing note
resolving; Tasks: RFC 5545 repetition including 29 February and months without a 31st,
reminders from the window's own timer, iCalendar round trips; Contacts: vCard 2.1/3.0/4.0
including Outlook's quoted-printable, Turkish alphabetical order). Bugs the window checks
caught before release: a hidden-attribute override, a startup focus race, search-box
sizing, dynamic text not re-translated, subtasks filtered out of date views, a missing
`search.query` capability in Notes' manifest.

**EON Calendar 0.1** (the last Wave 1 application): several calendars, RFC 5545
repetition with single occurrences changed or deleted from the window, IANA time zones
(a weekly meeting keeps its wall time across daylight saving, tested), reminders, day,
week, month and agenda views, people by address or through what EON Contacts publishes,
iCalendar import and export that doubles nothing. 23 tests, `--selftest` (including the
zone database) and 31 window checks. CalDAV moved to 0.2.

**Found on the way, fixed, and now guarded:**

- *Export and import did nothing in EON Tasks and EON Contacts windows.* The pages invoked
  `exportIcs`/`exportVcf`; Tauri matches the Rust name, `export_ics`. Unit tests called the
  library directly and the window checks never pressed those buttons. `--selftest` now fails
  when a page invokes a command the host does not register, and the window checks export
  and re-import through the forms. Fixed in both before their `v0.2.0` tags.
- *A reminder firing made an open Tasks editor lose its save* (the shown-mark bumped the
  task's revision). The mark now lives beside the task.
- *Calendar*: deleting a changed occurrence brought the original back; re-importing a file
  doubled it; a script ordering error stopped the whole page — caught by the window check
  before any commit.
- A forced shutdown on 19 Nov zeroed two uncommitted files and a linker database; nothing
  committed was lost.

Next, in order:

1. Keychain credential storage (for CalDAV/CardDAV and Mail), then CalDAV/CardDAV.
2. Encrypted backups in the core (step 1 of `encryption-at-rest.md`).
3. Small debt: the command-name guard in EON Notes and EON Launcher (both match their
   hosts today; the guard keeps it so).

The three releases above were downloaded after publishing: every file matches
`SHA256SUMS.txt` and each Windows binary passes `--selftest` and reports its version.

## Blockers and open questions

| Item | Blocks | Owner |
|---|---|---|
| eon-docs#2 (madde 43) merge | nothing technically; it is the record of the decision | project owner |
| madde 30 Faz L1 gate (legal entity) | any EON-operated Office service; application signing keys | project owner |
| madde 32g name availability (`EON Notes`, `EON Writer`, …) | public launch of product names | project owner |
| madde 33 licence confirmation | first outside contribution to Office repositories | project owner |
| ~~`eon-office-engine` library choice~~ | answered by the spike: yrs, with a conflict-recording layer for cells ([research/engine-spike.md](research/engine-spike.md)) | owner to confirm |
| LibreOffice as optional converter [A] | the long tail of DOCX/PPTX fidelity | formats research |

## Next milestone

**Gate 0 → 1:** a person installs, grants, runs and removes an EON Office application
without a terminal, through EON Launcher, and the launcher passes the madde 35 keyboard and
screen-reader checklist.
