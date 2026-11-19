<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Office — roadmap

**Status:** draft 1 · 19 November 2026 · governed by [madde 43](../plan/eon-plan.md) of the plan.
The Microsoft audit that shaped it is [microsoft-parity.md](microsoft-parity.md); where work
stands today is [status.md](status.md).

This document decides **what EON Office is made of, in what order, and when each piece
counts as done**. It does not promise dates. A one-person project that publishes dates for
forty products publishes fiction; it publishes *gates* instead, and a product starts when
the gate before it is passed.

---

## 1. Principles that decide the order

1. **Foundation before applications.** Nothing below the line "Wave 1" starts until the
   foundation it depends on exists and is tested. (Done for the core, spec and SDK.)
2. **Daily use before breadth.** The first applications are the ones a single person uses
   every day without a server: notes, tasks, calendar, contacts. They exercise the core
   hardest and need nothing that madde 30 gates.
3. **Interop is a product, not a feature.** Writer, Sheets and Slides are worthless to a
   switcher if they mangle a `.docx`. They wait for the formats work and its test corpus.
4. **Organisation features need the server; the server needs madde 30 only if *we* run
   it.** Self-hosted comes first; an EON-operated cloud does not exist before Faz L1.
5. **Use established technology where reinventing is dangerous** — identity providers,
   malware detection, media servers, CRDT libraries, format converters (madde 43f).
6. **No empty repositories** (madde 43d). A repository opens with a working first version.
7. **EON Stream `v1` wins clashes** (madde 43, cost 1).

## 2. The product set

Each line is one independent application with its own repository, release cycle and
manifest, unless it says *merged* — those decisions are explained in §3.

### Foundation (shared infrastructure, not applications)

| Repository | What | Status | Wave |
|---|---|---|---|
| `eon-office-spec` | Contracts: protocol, manifest, capabilities, sync, versioning | **v0 published** | 0 |
| `eon-office-core` | Local core service | **0.1.0 published, tested** | 0 |
| `eon-office-sdk` | Client library + reference integration | **0.1.0 published, tested** | 0 |
| `eon-office-launcher` | **EON Launcher**: the owner's shell — install, grant/revoke, launch, unified notifications, unified search, preferences, recent items | Next | 0 |
| `eon-office-marketplace` | Signed Git index of applications; discovery, install, update, compatibility | Next | 0 |
| `eon-office-formats` | Import/export and conversion (DOCX/XLSX/PPTX/ODF/PDF/ICS/VCF/EML…) + the compatibility test corpus | Research → build | 1–2 |
| `eon-office-engine` | Collaborative document engine: CRDT document model, presence, shared by Writer, Sheets, Slides, Whiteboard, Loop | Research [A] | 2 |
| `eon-office-server` | Self-hostable server: sync relay, organisation directory, OIDC/SCIM, presence relay | Design | 3 |

`eon-office-engine` is justified (the prompt allows it "only if architecturally
justified"): five applications need the same real-time co-editing model, and putting it in
the core would violate madde 43c, while putting it in each application would fork it five
ways. **Spike done (19 Nov 2026): yrs recommended** — about 7× faster merges, 11× smaller
per-edit deltas and 10× faster loads than automerge, on the condition that the engine records
the losing value of concurrent cell edits, which yrs drops silently and automerge keeps.
See [research/engine-spike.md](research/engine-spike.md).

### Applications

| # | Product | Repository | Purpose in one line | Wave |
|---|---|---|---|---|
| 1 | EON Notes | `eon-office-notes` | Notes, notebooks, backlinks, daily notes, graph, quick capture | 1 |
| 2 | EON Tasks | `eon-office-tasks` | Tasks, lists, boards, recurring, daily/weekly planning, goals | 1 |
| 3 | EON Calendar | `eon-office-calendar` | Calendars, events, recurrence (RFC 5545), time blocking, CalDAV | 1 |
| 4 | EON Contacts | `eon-office-contacts` | Contacts, groups, history (RFC 6350), CardDAV | 1 |
| 5 | EON Writer | `eon-office-writer` | Documents, DOCX/ODT fidelity, comments, suggestions, versions | 2 |
| 6 | EON Sheets | `eon-office-sheets` | Spreadsheets, formulas, charts, pivots, XLSX/ODS fidelity | 2 |
| 7 | EON Slides | `eon-office-slides` | Presentations, themes, presenter mode, PPTX/ODP fidelity | 2 |
| 8 | EON Drive | `eon-office-drive` | Files, local-first sync, versions, sharing, previews | 2 |
| 9 | EON Mail | `eon-office-mail` | IMAP/SMTP/JMAP mail, unified inbox, rules, signatures | 2 |
| 10 | EON AI | `eon-office-ai` | Optional assistant and agent host; user-chosen providers incl. local | 2 |
| 11 | EON Forms | `eon-office-forms` | Forms and surveys, logic, responses | 3 |
| 12 | EON Lists | `eon-office-lists` | Structured lists with typed columns, views, rules | 3 |
| 13 | EON Flow | `eon-office-flow` | Visual automation over core events and app methods | 3 |
| 14 | EON Chat | `eon-office-chat` | Direct and group messaging, channels, threads | 3 |
| 15 | EON Meetings | `eon-office-meetings` | Video/audio meetings on an established media server | 3 |
| 16 | EON Bookings | `eon-office-bookings` | Appointments and reservations for any business type | 3 |
| 17 | EON Finance | `eon-office-finance` | Income/expense, invoices, budgets, multi-currency, localisable ledgers | 3 |
| 18 | EON Analytics | `eon-office-analytics` | Dashboards and BI (absorbs "EON BI") | 3 |
| 19 | EON Projects | `eon-office-projects` | Gantt, critical path, resources, portfolios | 4 |
| 20 | EON Database | `eon-office-database` | Relational databases, queries, forms, reports | 4 |
| 21 | EON Studio | `eon-office-studio` | Low-code applications on Lists/Database/Flow | 4 |
| 22 | EON Diagrams | `eon-office-diagrams` | Flowcharts, UML, ERD, network and org charts; SVG/PDF | 4 |
| 23 | EON Whiteboard | `eon-office-whiteboard` | Infinite collaborative canvas | 4 |
| 24 | EON Loop | `eon-office-loop` | Live components embedded across applications | 4 |
| 25 | EON Share | `eon-office-share` | Intranet portals, document libraries, approvals | 4 |
| 26 | EON Capture | `eon-office-capture` | Screenshots, recording, OCR, clipboard history, quick notes | 4 |
| 27 | EON Media | `eon-office-media` | Media library and light editing, captions (absorbs Clipchamp/Stream scope) | 4 |
| 28 | EON Publish | `eon-office-publish` | Newsletters, web publications, print layouts (Sway/Publisher scope) | 4 |
| 29 | EON Admin | `eon-office-admin` | Organisation administration (absorbs "Admin Center") | 5 |
| 30 | EON Endpoint | `eon-office-endpoint` | Device inventory, policies, deployment (absorbs "Device Manager") | 5 |
| 31 | EON Security | `eon-office-security` | Posture, permission audit, integrations (absorbs "Defender" scope) | 5 |
| 32 | EON Compliance | `eon-office-compliance` | Retention, legal hold, eDiscovery, classification | 5 |
| 33 | EON Community | `eon-office-community` | Organisation communities and announcements | 5 |
| 34 | EON Learning | `eon-office-learning` | Workplace training paths — coordinated with EON Edu, not duplicating it | 5 |
| 35 | EON Places | `eon-office-places` | Desks and rooms, hybrid-work coordination, on Bookings' resource model | 5 |
| 36 | EON Insights | `eon-office-insights` | Personal work insights first; aggregated, privacy-preserving team views only | 5 |
| 37 | EON Remote | `eon-office-remote` | Consent-based remote support | 5 |
| 38 | EON Customization Tool | `eon-office-customization-tool` | Install profiles, component selection, update channels | 5 |
| 39 | EON Developer Tools | `eon-office-developer-tools` | App templates, manifest tooling, .NET/SDK environment helpers | 5 |

## 3. Decisions taken while drawing this map

| Question (from the directive) | Decision | Why |
|---|---|---|
| EON BI separate, or Analytics grows into it? | **One product, EON Analytics.** | Two BI products split one data-model and connector investment in two. "BI" is a later milestone of Analytics, not another app. |
| EON Tasks vs EON Projects? | **Both, separate.** Tasks for everyday work; Projects for Gantt, critical path, resources, portfolios. Projects reads Tasks through `call:`; it does not own tasks. | Microsoft itself folded Project for the web into Planner, and retires Project Online on 30 Sep 2026 [V]; people who manage portfolios need more than a board, people who need a board must not pay for a portfolio tool's complexity. |
| EON Capture vs EON Media? | **Both, separate.** Capture is instant and tiny; Media is a library and an editor. | Different start-up budgets: a screenshot tool must open in under 200 ms. |
| Admin vs Admin Center | **Merged into EON Admin** (organisation administration). Local system diagnostics go to EON Endpoint. | Two admin consoles is the Microsoft 365 experience nobody likes. |
| Device Manager vs Endpoint | **Merged into EON Endpoint.** | Same agent, same inventory, same policy engine. |
| Security vs Defender | **Merged into EON Security**, staged: posture and permission audit first; XDR-class detection only through established engines. | madde 43f: no detection claims without a real, tested engine. |
| EON Identity | **Not a product for now.** `eon-office-server` is an OIDC relying party and SCIM consumer; organisations use an established identity provider (e.g. Keycloak, Authentik, Kanidm, or their existing one). Revisited after Wave 3. | Writing an identity provider is a security product of its own; doing it badly is worse than integrating a good one. |
| EON Cloud PC | **NOT PLANNED.** Documented integration with existing VDI (e.g. Apache Guacamole) instead. | Infrastructure product with no reuse of the office stack; enormous operating cost. |
| EON Places vs Bookings | **Separate apps, one resource model.** Places depends on Bookings. | Internal workplace coordination and external customer bookings have different users and privacy rules. |
| EON Learning vs EON Edu | **Learning is corporate training;** shared content packaging with EON Edu is investigated before any code. | Avoid two learning products under one roof. |
| EON Insights | **Personal first; team views only aggregated with a minimum group size; never per-employee reports to managers.** | Microsoft Viva Insights draws exactly this criticism; EON's madde 31/36 posture forbids the surveillance version. |
| Publisher's scope | **Folded into EON Publish** (print layouts + web publications). | Publisher left Microsoft 365 on 1 Oct 2026 [V]; Publisher users need a home and a `.pub` import path. |

## 4. Waves and gates

A wave starts when the gate before it is passed. Each gate is measurable.

### Wave 0 — foundation (in progress)

- [x] Spec v0, core 0.1.0, SDK 0.1.0, reference integration, CI green on Windows, macOS, Linux.
- [x] **EON Launcher** 0.1 (released `v0.1.0`): install from a manifest file, grant/revoke with the wording of
  `capabilities.md`, launch/stop, notification centre, unified search, preferences.
- [x] **Marketplace** 0.1: Git index format, signature verification reusing madde 34's
  mechanism (empty trust set until a legal entity exists, as in EON Stream), compatibility
  check on `api` ranges. *Remaining:* `eon-office-launcher` installs from it.
- [ ] Core: encryption-at-rest design document; `sync` transport design for the server.

**Gate 0 → 1:** a person can install, grant, run and remove an application without a
terminal, and the launcher passes the madde 35 keyboard and screen-reader checklist.

### Wave 1 — personal productivity, no server

**Progress (19 Nov 2026):** EON Notes 0.1, EON Tasks 0.1 and EON Contacts 0.1 written and
published with the criteria below tested; EON Calendar is next.

Notes, Tasks, Calendar, Contacts. Each is Tauri + the SDK, offline-first, local-only.
Integrations: Notes ↔ Tasks (create task from a line), Calendar ↔ Tasks (due dates, time
blocking), Contacts ↔ Calendar (attendees). All optional (`app_unavailable` handled).

**Gate 1 → 2:** the four applications have shipped `0.1` with their acceptance criteria
(§6), sync between two devices by file works for all four, and import from Outlook
(ICS/VCF export) and from Markdown vaults (Obsidian-style folders) is tested.

### Wave 2 — documents, files, mail, AI

`eon-office-formats` with the compatibility corpus first; then Writer, Sheets, Slides;
Drive; Mail; AI.

**Gate 2 → 3:** the formats corpus round-trips (see [microsoft-parity.md §5](microsoft-parity.md))
at the thresholds stated there; Writer/Sheets/Slides `0.1`; Drive syncs a folder between
two devices; Mail works against Gmail, Outlook.com and a self-hosted Dovecot over
IMAP/SMTP with OAuth2 where the provider requires it; AI works with at least one remote and
one local provider and is entirely absent when not installed.

### Wave 3 — teams and small organisations (self-hosted server)

`eon-office-server` 0.1 (sync relay, directory, OIDC sign-in), then Forms, Lists, Flow,
Chat, Meetings, Bookings, Finance, Analytics.

**Gate 3 → 4:** a ten-person organisation can run the server with one command, sign in
through its own identity provider, and use Chat, shared Calendar and Drive for a month
without data loss — measured in a pilot with a written report, as madde 24 asks for Edu.

### Wave 4 — platform and professional tools

Projects, Database, Studio, Diagrams, Whiteboard, Loop, Share, Capture, Media, Publish.

### Wave 5 — enterprise administration, security, compliance

Admin, Endpoint, Security, Compliance, Community, Learning, Places, Insights, Remote,
Customization Tool, Developer Tools.

**Before any Wave 5 product is offered to an organisation:** madde 13's legal review covers
its data processing; for Remote, Insights and Compliance specifically, a data-protection
impact assessment is written (madde 31).

## 5. Cross-application capabilities — where each one lives

| Capability | Lives in | State |
|---|---|---|
| Application launcher, favourites, recent items | EON Launcher (recent items via an `activity` event convention) | Wave 0 |
| Unified search | Core `search.*` | **Implemented, tested** |
| Unified notifications | Core `notifications.*` + Launcher | **Implemented** (core); Launcher UI Wave 0 |
| Shared identity, directory, roles | Core `directory.*`, `access.check`; server for organisations | **Implemented** (local); server Wave 3 |
| Unified settings and preferences | Core `prefs.*`, `settings.*` | **Implemented, tested** |
| Cross-application calls and references | Core `apps.call` with `provides`; deep links `eon-office://<app>/<path>` **[A]** | Calls **implemented**; link format to spec |
| Events and automation triggers | Core `events.*`; EON Flow consumes them | Events **implemented**; Flow Wave 3 |
| Background jobs | Core `schedule.*` | **Implemented, tested** |
| Offline-first sync | Core `sync.*`; transport in server | Mechanism **implemented**; transport Wave 3 |
| Real-time co-editing, presence, cursors | `eon-office-engine` + server relay | Research |
| Comments, mentions, version history | Engine (document-level) + each app's UI | Wave 2 |
| Activity feed | A core event convention (`activity.*`) aggregated by Launcher | Spec work, Wave 0 |
| Audit | Core `audit.*`; organisation-wide in server | **Implemented** (local) |
| Backup, restore, migration of the core | Core `backup.create`, `restore`, migrations | **Implemented, tested** |
| Themes, accessibility preferences | Core `themes.*`, `prefs` | **Implemented**; each app must honour them |
| AI context | EON AI as an app; it reaches data only via `call:` capabilities the person granted | Wave 2 |
| Multi-tenancy, HA, data residency | Server | Wave 3+ |

## 6. Acceptance criteria — first release of each Wave 1–2 product

A product's `0.1` ships when all of its lines are true and tested; documentation alone never
counts (Development rule 11).

**EON Notes 0.1** — Markdown notes with live preview; notebooks and tags; `[[backlinks]]`
with a backlinks panel; daily note; full-text search through the core with Turkish folding;
works with no network; sync by core export/import; import a folder of Markdown files with
links intact (≥ 99 % of links resolved on a 1,000-note test vault); keyboard-only operation;
starts in < 1 s on the reference machine.

**EON Tasks 0.1** — tasks, subtasks, due dates, recurring rules (RFC 5545 RRULE subset:
daily, weekly by day, monthly by day/date, yearly); lists and a board view; today/upcoming;
reminders through core jobs and notifications; `provides: task.create, task.open`; ICS
`VTODO` import/export.

**EON Calendar 0.1** — multiple calendars; events with full RFC 5545 recurrence including
exceptions; time zones from the IANA database; reminders; day/week/month/agenda views;
ICS import/export validated against a corpus of Outlook, Google and Apple exports;
CalDAV read/write against Radicale and Nextcloud.

**EON Contacts 0.1** — vCard 3.0/4.0 import/export (round-trip test corpus incl. Outlook
exports); groups; search; `provides: contact.lookup`. *Changed 19 Nov 2026:* CardDAV sync
moves to `0.2`, together with the keychain credential storage EON Mail needs — a CardDAV
password must not be stored in core storage, and that storage is designed once, for both.

**EON Writer 0.1** — paragraphs, headings, lists, tables, images, footnotes, comments,
tracked changes; DOCX and ODT open/save meeting the corpus thresholds; PDF export; version
history through core storage; Markdown import/export.

**EON Sheets 0.1** — formula engine covering the 200 most-used Excel functions (measured
list), with results identical to Excel on the formula test corpus; charts (line, bar, pie,
scatter); XLSX/ODS/CSV with corpus thresholds; freeze panes, filters, sort; pivot tables in
`0.2`.

**EON Slides 0.1** — layouts, themes, text, images, shapes, transitions; presenter view on a
second screen; PPTX/ODP corpus thresholds; PDF export.

**EON Drive 0.1** — a local folder synced between two devices through the server or a
removable disk; version history (last 30 versions); conflict copies, never silent
overwrite; previews for images, PDF and the formats engine's types.

**EON Mail 0.1** — IMAP/SMTP with OAuth2 for Microsoft and Google; unified inbox; threads;
search through core; signatures; rules (move, label, mark); scheduled send through core
jobs; S/MIME verification; `.eml` import/export; **account credentials are stored in the
OS keychain, never in core storage**.

**EON AI 0.1** — providers: one OpenAI-compatible HTTP endpoint (covers many vendors and
local servers such as llama.cpp/Ollama) and one Anthropic endpoint; no provider configured
by default; every request shows what will be sent before it is sent, the first time per
source; `provides: ai.complete, ai.summarize`; agents act only through `call:` capabilities
the person granted, and every agent action is in the audit log.

## 7. Integrated strategies

The directive asks for fourteen strategic deliverables. Each is answered in one place:

| Deliverable | Where |
|---|---|
| 1. Microsoft capability inventory | [microsoft-parity.md §2](microsoft-parity.md) |
| 2. Product coverage matrix | [microsoft-parity.md §3](microsoft-parity.md) |
| 3. Missing applications | §2 above: Launcher, Projects, Database, Diagrams, Whiteboard, Lists, Loop, Flow, Studio, Share, Publish, Media, Community, Insights, Learning, Places, Compliance added to the initial list |
| 4. Missing features in existing applications | [microsoft-parity.md §4](microsoft-parity.md) |
| 5. Recommended new repositories | §2: `eon-office-launcher`, `-formats`, `-engine`, `-server` and the application list |
| 6. Updated architecture | [`eon-office-spec/docs/architecture.md`](https://github.com/EON-Extensible-Open-Network/eon-office-spec/blob/main/docs/architecture.md) |
| 7. Cross-application integration architecture | §5 above + the spec's protocol |
| 8. File compatibility strategy | [microsoft-parity.md §5](microsoft-parity.md) |
| 9. Microsoft migration strategy | [microsoft-parity.md §6](microsoft-parity.md) |
| 10. Enterprise deployment strategy | §8 below |
| 11. Security and compliance roadmap | §9 below |
| 12. AI and automation roadmap | §10 below |
| 13. Development priority matrix | §11 below |
| 14. Long-term roadmap | §4 above |

## 8. Enterprise deployment strategy

- **Packaging:** per application, MSIX and winget on Windows, `.dmg` on macOS, Flatpak and
  `.deb`/`.rpm` on Linux. Every package is signed once madde 34's keys exist; until then
  releases ship with SHA-256 sums from CI, as EON Stream does.
- **EON Customization Tool** produces an install profile (applications, components, update
  channel, preferences, pre-granted capabilities for managed devices) as a signed JSON
  document the installers read — the Office Deployment Tool's job, as data instead of a
  bespoke XML dialect.
- **Update channels:** `stable`, `preview`, and organisation-pinned. An organisation approves
  a version before its devices take it (madde 39's Edu rule, generalised).
- **Server:** one container image and a single-binary option; SQLite for small
  organisations, PostgreSQL for larger ones; high availability is a Wave 5 milestone, not a
  launch claim.
- **Hybrid:** EON Office coexists with Microsoft 365 — Mail and Calendar speak Exchange's
  open standards and Graph for migration; Drive can sit beside OneDrive.

## 9. Security and compliance roadmap

| Stage | Work |
|---|---|
| Now | Core threat model ([`eon-office-core/SECURITY.md`](https://github.com/EON-Extensible-Open-Network/eon-office-core/blob/main/SECURITY.md)); capability model; audit chain; no telemetry |
| Wave 0 | Application signing through the marketplace (madde 34 mechanism); encryption-at-rest design with key custody in the OS keychain |
| Wave 1–2 | Mail credentials in keychains; S/MIME; per-app data export (madde 31 portability) |
| Wave 3 | Server: TLS only, OIDC, SCIM, organisation audit, end-to-end encryption of sync payloads designed before the relay ships; external security review before the first organisation pilot (madde 24's pattern) |
| Wave 5 | Compliance: retention, legal hold, eDiscovery export; Security: posture checks and integrations (OS protection status, osquery, ClamAV) — **no own detection engine** |
| Always | Coordinated disclosure policy; dependency gate (`cargo deny`) in every repository |

## 10. AI and automation roadmap

- **Automation first, AI second.** EON Flow (Wave 3) builds on core events, jobs and
  `apps.call`: triggers, conditions, actions, approvals, run history. It works with no AI.
- **EON AI (Wave 2)** is one application that other applications *call*; there is no AI in
  the core and no application requires it.
- **Agents (Wave 3+):** an agent is a Flow or AI task acting under capabilities the person
  granted to EON AI — it cannot reach an application the person did not connect, every
  step is audited, and a person can see and stop running agents.
- **Providers:** remote (OpenAI-compatible, Anthropic, others via the same adapters) and
  local; none by default; per-application opt-in; organisation policy can forbid remote
  providers entirely.
- **What never happens:** sending a document to a provider without the person having seen
  that it would be sent; training on users' data; AI as the only path to a feature.

## 11. Development priority matrix

Value: how many people need it daily. Effort: rough person-months for a `0.1` that meets §6.
Risk: chance the `0.1` misses its criteria.

| Product | Value | Effort | Risk | Depends on | Priority |
|---|---|---|---|---|---|
| Launcher | High | 2 | Low | core, SDK | **P0** |
| Marketplace | High | 2 | Medium (signing, madde 30) | spec | **P0** |
| Notes | High | 3 | Low | core | **P1** |
| Tasks | High | 3 | Low | core | **P1** |
| Calendar | High | 4 | Medium (recurrence, time zones) | core | **P1** |
| Contacts | Medium | 2 | Low | core | **P1** |
| Formats | High | 6+ | **High** (fidelity) | — | **P1** |
| Writer | High | 6 | High | formats, engine | P2 |
| Sheets | High | 8 | **High** (formula parity) | formats, engine | P2 |
| Slides | Medium | 5 | High | formats, engine | P2 |
| Drive | High | 4 | Medium | core sync, server | P2 |
| Mail | High | 6 | High (providers, OAuth) | contacts | P2 |
| AI | Medium | 2 | Medium | core | P2 |
| Server | High (orgs) | 6 | High | spec | P2 |
| Flow | High (platform) | 4 | Medium | core events | P2–P3 |
| Forms, Lists | Medium | 3 each | Low | core | P3 |
| Chat | High (orgs) | 5 | Medium | server | P3 |
| Meetings | High (orgs) | 6 | High (media server ops) | server, calendar | P3 |
| Bookings, Finance, Analytics | Medium | 4–6 each | Medium | calendar / lists | P3 |
| Projects, Database, Studio, Diagrams, Whiteboard, Loop, Share | Medium | 4–8 each | Medium–High | engine, lists, flow | P4 |
| Capture, Media, Publish | Medium | 3–6 | Medium | drive, formats | P4 |
| Admin, Endpoint, Security, Compliance | Medium (enterprise) | 6+ each | High | server | P5 |
| Community, Learning, Places, Insights, Remote | Low–Medium | 3–6 | High (privacy) | server | P5 |
| Customization Tool, Developer Tools | Low | 2 | Low | marketplace | P5 |

## 12. What this roadmap does not do

- Promise dates.
- Count a product as covered because it appears in a table.
- Put any of these products into one binary.
- Start a Wave before its gate, or start Office work that delays a planned EON Stream
  release.
