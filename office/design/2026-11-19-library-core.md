<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Office: the core is a library, every application stands alone

**Design, 19 November 2026.** Replaces the first architecture (a local core *service* every
application connected to). The owner's verdict on that design, and the reason for this one:
"the core is not an app, it should be the kernel of the code"; "is it really modular? to run
Notes you need the launcher and the core running". The choice of approach was handed to the
author, with one bar: *anyone who picks up any single EON Office application should be able
to say it is better than Microsoft's.*

## Intent

- **Install only what you use.** Download EON Notes, run it, done. No service, no shell, no
  second download. Other EON Office applications add integration when present; their
  absence costs nothing.
- **Mirror EON Stream.** `eon-office-core` is code that applications link, exactly as
  `eon-stream-core` is. Runnable programs live in application repositories.
- **Better than Microsoft, one application at a time.** Take what people universally like in
  the Microsoft counterpart, keep EON's rules (offline, private, Turkish done right, fast),
  and fix what people complain about.

### Non-goals and anti-goals

- Not one program with tabs (the directive forbids it; it is also what people dislike in
  heavy suites).
- No background service required for anything to work. An *optional* helper may come later
  for reminders while every window is closed; nothing may depend on it.
- No security theatre: a check that a same-user program can step around is not presented as
  a boundary.

## Approaches considered

| | How it works | Install only Notes | Integration | Moving parts | Stream fit |
|---|---|---|---|---|---|
| **A. Core service** (the first design) | A daemon brokers storage, events and calls over loopback JSON-RPC | No — the daemon must run | Strong, central | High: daemon lifecycle, tokens, supervisor (cf. KDE Akonadi, GNOME EDS — both known for weight and fragility) | No |
| **B. Library core, standalone applications** *(chosen)* | Each application links `eon-office-core`; shared needs go through shared files in a known folder and through the operating system | Yes | Enough: deep links, published data, shared index | Low | Yes |
| **C. One shell, many plug-in modules** | One host process loads applications as modules (Obsidian, VS Code) | Partly | Strong | Medium | No; the directive forbids one application with modules |
| **D. Server-centred** | Applications served to a browser (Nextcloud) | No — a server is required | Strong | High | No; contradicts offline-first and Tauri (madde 2) |

**Why B.** A's one real advantage was central permission checks, and the first design's own
`SECURITY.md` already conceded they do not stop a same-user program reading the database
file. B keeps everything that mattered — the modules written for the core, the protocol's
rules, the tests' intent — and removes the daemon, its tokens, its supervisor and the
launcher dependency. C and D fail the directive or the plan.

## Design

### Repositories (Stream's pattern)

| Repository | Role | Stream counterpart |
|---|---|---|
| `eon-office-spec` | Contracts: the shared on-disk formats, the manifest, capabilities, `eon-office://` links, sync format | `eon-stream-spec` |
| `eon-office-core` | **Library**: storage, shared index, preferences, application registry, activity, sync, audit, themes, text folding, file-format helpers | `eon-stream-core` |
| `eon-office-<app>` | One runnable application each (Notes, Tasks, Contacts, Calendar, …) | `eon-stream-app` |
| `eon-office-launcher` | Optional hub: marketplace, permissions overview, shared preferences, recent items. Never required | — |
| `eon-office-sdk` | Tooling for application authors (templates, manifest checks), MIT. The daemon client is retired | `eon-stream-sdk` |
| `eon-office-marketplace` | Signed Git index (unchanged format) | `eon-stream-marketplace` |

### The shared folder

The per-user EON Office home (`EON_OFFICE_HOME`, else the platform data directory — kept
from the first design):

```
<home>/
  shared.sqlite        registry · preferences · search index · activity · public data · audit
  apps/<id>/data.sqlite each application's own documents (only it opens this)
  apps/<id>/            the application's other files
```

SQLite in WAL mode with a busy timeout lets several processes read and write safely; every
write is a short transaction. `eon-office-core` is the only code that touches these files,
so the format has one implementation, specified in `eon-office-spec`.

### What each former core service becomes

| First design (service method) | Now |
|---|---|
| `storage.*` | `core::Store::open(home, app_id)` on `apps/<id>/data.sqlite`; same documents, revisions and conflict records |
| `sync.*` | Same export/import functions on the application's own store |
| `search.*` | `core::Index` on `shared.sqlite`; private entries carry their owner and are filtered for others |
| `notifications.*` | The operating system's own notification centre (Tauri's notification plugin). People already look there |
| `events.*` / `apps.call` | `eon-office://<app>/<action>` links for actions (open, create), handled through single-instance + deep-link so a running window receives them and a closed one starts. Read-only data another application may use is **published**: e.g. Contacts writes name/e-mail/phone summaries to the shared `public` table that Mail and Calendar read |
| `schedule.*` | Each application schedules its own reminders while running and catches up at start. An optional background helper is a later, separate decision |
| `prefs.*`, `themes.*` | `core::Prefs` in `shared.sqlite`; applications watch the file for changes |
| `apps.*` (registry) | `core::Registry` in `shared.sqlite`: an application registers itself (id, name, version, path, `provides`) on first start; the launcher and the marketplace read and write the same table |
| `directory.*`, `access.check` | Library functions; meaningful once `eon-office-server` brings organisations |
| `audit.*` | Library function appending to the shared hash chain |
| Capabilities | Declared in the manifest, **shown at install** by the launcher/marketplace and enforced by the library for the application's own calls. Stated plainly as transparency and a guard against mistakes, not a boundary against same-user code; a real boundary arrives with sandboxed (WASM) third-party code, madde 4 |

### Applications

Each application crate depends on `eon-office-core` (pinned by commit, as `eon-stream-app`
pins `eon-stream-core`), opens its store and the shared folder on start, registers itself,
and works fully with nothing else installed. Tauri plugins allowed, by decision: notification,
single-instance, deep-link — nothing that gives the interface file system, shell or network
access.

### "Better than Microsoft" — the bar each application is held to

Kept across all applications: start in under a second; no account; works offline; one
keyboard path to everything; Turkish sorting, search and casing right; follows the person's
text size, contrast and motion settings; never loses an edit silently.

Per application, the loved features to match and the complaints to fix are written into each
application's acceptance criteria in `roadmap.md` §6 (first: Notes ← OneNote/Obsidian,
Tasks ← To Do/Planner, Contacts ← Outlook People, Calendar ← Outlook Calendar).

## Migration (summarised; the plan has the steps)

1. Spec: the shared-folder contract replaces the protocol document; the protocol stays
   readable in history.
2. Core: the modules become a library API over the two databases; the server, token and
   supervisor code are removed. Tests keep their intent (isolation, conflicts, sync,
   Turkish folding, audit) as library and multi-process tests.
3. Applications (Notes, Tasks, Contacts, then Calendar): swap the SDK client for the core
   library; add self-registration, OS notifications, deep links. Window checks keep running.
4. Launcher: reads the registry and marketplace directly; becomes optional.
5. SDK: retires the client; becomes tooling. Marketplace installs: download, verify, place,
   register — no service involved.
6. Releases: new versions on the new architecture; published ones stay as history.

## Left to the builder

- Exact table layouts of `shared.sqlite` (specified in `eon-office-spec`).
- How an application notices preference changes (file watcher vs a short poll).
- Whether the launcher keeps its name.

## Open for the owner (madde 33)

Third-party applications linking a GPL library: today they would use the spec'd formats
(any language) or an MIT re-implementation; whether the core should carry LGPL/MPL instead
is a licence decision, not made here.
