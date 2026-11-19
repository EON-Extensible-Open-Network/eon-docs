<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Office — Microsoft ecosystem audit and parity matrix

**Status:** draft 1 · 19 November 2026 · maintained alongside [roadmap.md](roadmap.md).

EON Office is not a Microsoft clone. This document exists so that a person or an
organisation leaving Microsoft can see, product by product, what EON offers, what it does
not yet, and what it will never do — without having to trust a slogan.

## 1. How to read this

**Evidence labels.** Every statement about a Microsoft product carries one:

- **[V]** — verified against an official Microsoft page during this audit (link given).
- **[K]** — from general knowledge as of mid-2026, **not yet re-verified**. Treat as an
  assumption until someone checks it against the official documentation and changes the
  label. Product names, bundling and retirements change often; this label is honest about
  that.

**EON statuses** (the directive's scale): NOT PLANNED · PLANNED · DESIGNED · IN DEVELOPMENT ·
IMPLEMENTED · TESTED · PRODUCTION READY. Nothing in EON Office is PRODUCTION READY today, and
nothing becomes so on the strength of a document or a mock-up.

Today only the foundation exists. Where a Microsoft capability is a *platform* capability
(search across apps, notifications, audit), the matrix says what the EON **core** provides —
which is not the same as a finished product a person can use.

### Verified this audit

| Fact | Source |
|---|---|
| Publisher is removed from Microsoft 365 on or shortly after 1 Oct 2026; perpetual Publisher support ends 13 Oct 2026; Microsoft advises converting files to PDF or Word | [V] [support.microsoft.com](https://support.microsoft.com/en-us/publisher/microsoft-publisher-will-no-longer-be-supported-after-october-2026) |
| Project for the web retired in August 2025; its plans live on as Planner "premium plans"; Roadmaps replaced by Portfolios | [V] [Planner blog](https://techcommunity.microsoft.com/blog/plannerblog/transitioning-to-microsoft-planner-and-retiring-microsoft-project-for-the-web/4410149) |
| Project Online retires 30 Sep 2026 | [V] [Planner blog](https://techcommunity.microsoft.com/blog/plannerblog/microsoft-project-online-is-retiring-what-you-need-to-know/4450558) |
| Microsoft Places: desk booking (individual desks and pools), booking from Teams/Outlook/Places, peripheral-based check-in; individual desk booking needs a space licence | [V] [Learn: Desks](https://learn.microsoft.com/en-us/microsoft-365/places/room-desk-booking/desks), [Places overview](https://learn.microsoft.com/en-us/microsoft-365/places/places-overview) |
| Sway's Windows desktop app was retired on 1 Jun 2026; Sway continues on the web | [V] [Sway service description](https://learn.microsoft.com/en-us/office365/servicedescriptions/microsoft-sway-service-description) |

Three of these are **migration windows open right now**: Publisher users, Project Online
customers and Project for the web roadmap users are being moved whether they like it or not.
The roadmap gives `.pub` import (EON Publish) and `.mpp`/Project Online import (EON Projects)
an explicit place for that reason.

## 2. Capability inventory

| Microsoft product | Problem it solves | Who needs it | Core capabilities | Evidence |
|---|---|---|---|---|
| Word | Documents | Everyone | Rich editing, styles, tables, references, track changes, comments, co-authoring, DOCX | [K] |
| Excel | Calculation and data | Everyone, finance | ~500 functions, dynamic arrays, charts, pivots, Power Query, XLSX | [K] |
| PowerPoint | Presentations | Everyone | Layouts, themes, animations, presenter view, co-authoring | [K] |
| Outlook / Exchange | Mail, calendar, contacts | Everyone, organisations | Multi-account mail, rules, shared mailboxes, calendars, delegation, EWS/Graph | [K] |
| OneNote | Notes | Students, professionals | Freeform notebooks, ink, audio, web clipper | [K] |
| Teams | Chat, meetings, calls | Organisations | Chat, channels, meetings, recordings, transcripts, apps | [K] |
| OneDrive | Personal files | Everyone | Sync, versions, sharing, Files On-Demand | [K] |
| SharePoint | Intranet, document management | Organisations | Sites, libraries, metadata, permissions, workflows, search | [K] |
| Loop | Live shared components | Teams | Components embedded across Teams/Outlook, workspaces | [K] |
| Planner (incl. former Project for the web) | Task and plan management | Teams | Boards, premium plans: Goals, Sprints, dependencies with lead/lag, baselines, Portfolios | [V] |
| Project (desktop / Online) | Scheduling, resources, portfolios | Project managers | Gantt, critical path, resource levelling, `.mpp`; Online retiring | [V] (Online) / [K] |
| To Do | Personal tasks | Everyone | My Day, lists, reminders, flagged mail | [K] |
| Visio | Diagrams | Engineers, analysts | Shape libraries, data-linked diagrams, `.vsdx` | [K] |
| Access | Desktop databases | Small businesses | Tables, queries, forms, reports, VBA, `.accdb` | [K] |
| Lists | Structured lists | Teams | Typed columns, views, rules, forms | [K] |
| Forms | Surveys and quizzes | Everyone | Builder, branching, quizzes, response analytics | [K] |
| Bookings | Customer appointments | Small businesses | Booking pages, staff, services, reminders | [K] |
| Power Apps | Low-code apps | Business units | Canvas/model-driven apps, Dataverse, connectors | [K] |
| Power Automate | Automation | Business units | Triggers, actions, approvals, RPA, connectors | [K] |
| Power BI | Business intelligence | Analysts | Data models, DAX, Power Query, dashboards, embedding | [K] |
| Copilot / Copilot Studio | AI assistance and agents | Everyone | In-app assistance, chat over organisation data, agents | [K] |
| Viva (Engage, Insights, Learning, Connections, Goals…) | Employee experience | HR, communications | Communities, work-pattern insights, learning paths | [K] |
| Places | Hybrid workplace | Facilities | Desk and room booking, presence planning | [V] |
| Stream (on SharePoint) / Clipchamp | Video | Everyone | Video library, transcripts / editor | [K] |
| Whiteboard | Visual collaboration | Teams | Canvas, templates, sticky notes | [K] |
| Publisher | Print layout | Small businesses, schools | Desktop publishing, `.pub` — **retired Oct 2026** | [V] |
| Sway | Web publications | Educators, comms | Interactive stories — **desktop app retired, web continues** | [V] |
| Intune | Endpoint management | IT | Enrolment, compliance, app deployment, remote actions | [K] |
| Entra ID | Identity | IT | SSO, MFA, conditional access, B2B, provisioning | [K] |
| Defender (XDR family) | Security operations | Security teams | Endpoint detection, incidents, threat intelligence | [K] |
| Purview | Compliance | Compliance officers | Classification, DLP, retention, eDiscovery, audit | [K] |
| Priva | Privacy management | Privacy officers | Personal-data discovery, subject requests | [K] |
| Microsoft 365 admin center | Administration | IT | Users, licences, service health, reports | [K] |
| Windows 365 / Azure Virtual Desktop | Virtual desktops | IT | Cloud PCs, pooled desktops | [K] |
| .NET, Visual Studio, VS Code, NuGet | Development | Developers | SDKs, IDEs, packages | [K] |

## 3. Coverage matrix

EON status is where the **EON product** stands. "Core" in *Completeness* means the shared
mechanism exists in `eon-office-core` and is tested, not that a product exists.

| Microsoft | EON product | EON status | Completeness | Main gaps | Priority | Repository | Tests |
|---|---|---|---|---|---|---|---|
| Word | EON Writer | PLANNED | 0 % | everything; DOCX fidelity is the risk | P2 | `eon-office-writer` | — |
| Excel | EON Sheets | PLANNED | 0 % | formula engine, pivots, Power Query equivalent | P2 | `eon-office-sheets` | — |
| PowerPoint | EON Slides | PLANNED | 0 % | everything | P2 | `eon-office-slides` | — |
| Outlook (mail) | EON Mail | PLANNED | 0 % | everything; Exchange-only features via Graph | P2 | `eon-office-mail` | — |
| Outlook (calendar) | EON Calendar | DESIGNED | 0 % (criteria in roadmap §6) | delegation, shared calendars need server | P1 | `eon-office-calendar` | — |
| Outlook (people) | EON Contacts | DESIGNED | 0 % | organisation directory needs server | P1 | `eon-office-contacts` | — |
| OneNote | EON Notes | DESIGNED | 0 % | ink, audio notes later | P1 | `eon-office-notes` | — |
| To Do | EON Tasks | DESIGNED | 0 % | flagged-mail integration needs Mail | P1 | `eon-office-tasks` | — |
| Planner (basic) | EON Tasks | DESIGNED | 0 % | team boards need server | P1 | `eon-office-tasks` | — |
| Planner premium / Project | EON Projects | PLANNED | 0 % | Gantt, CPM, levelling, `.mpp` import | P4 | `eon-office-projects` | — |
| Teams (chat) | EON Chat | PLANNED | 0 % | needs server | P3 | `eon-office-chat` | — |
| Teams (meetings) | EON Meetings | PLANNED | 0 % | media server (LiveKit or Jitsi) integration | P3 | `eon-office-meetings` | — |
| OneDrive | EON Drive | PLANNED | Core sync mechanism | file sync client, sharing, previews | P2 | `eon-office-drive` | core: sync tests |
| SharePoint | EON Share | PLANNED | 0 % | sites, libraries, workflows | P4 | `eon-office-share` | — |
| Loop | EON Loop | PLANNED | 0 % | needs `eon-office-engine` | P4 | `eon-office-loop` | — |
| Visio | EON Diagrams | PLANNED | 0 % | `.vsdx` import | P4 | `eon-office-diagrams` | — |
| Whiteboard | EON Whiteboard | PLANNED | 0 % | needs engine | P4 | `eon-office-whiteboard` | — |
| Access | EON Database | PLANNED | 0 % | `.accdb`/`.mdb` import | P4 | `eon-office-database` | — |
| Lists | EON Lists | PLANNED | 0 % | everything | P3 | `eon-office-lists` | — |
| Forms | EON Forms | PLANNED | 0 % | public forms need server | P3 | `eon-office-forms` | — |
| Bookings | EON Bookings | PLANNED | 0 % | public booking pages need server | P3 | `eon-office-bookings` | — |
| Power Automate | EON Flow | PLANNED | Core events, jobs, calls | builder, connectors, approvals, run history | P2–P3 | `eon-office-flow` | core: event/job/call tests |
| Power Apps | EON Studio | PLANNED | 0 % | everything | P4 | `eon-office-studio` | — |
| Power BI | EON Analytics | PLANNED | 0 % | data model, query language, visuals | P3 | `eon-office-analytics` | — |
| Copilot | EON AI | DESIGNED | 0 % (criteria in roadmap §6) | provider adapters, agent host | P2 | `eon-office-ai` | — |
| Viva Engage | EON Community | PLANNED | 0 % | needs server | P5 | `eon-office-community` | — |
| Viva Insights | EON Insights | PLANNED (restricted scope) | 0 % | personal insights only at first | P5 | `eon-office-insights` | — |
| Viva Learning | EON Learning | PLANNED | 0 % | coordinate with EON Edu | P5 | `eon-office-learning` | — |
| Places | EON Places | PLANNED | 0 % | on Bookings resources | P5 | `eon-office-places` | — |
| Stream / Clipchamp | EON Media | PLANNED | 0 % | everything | P4 | `eon-office-media` | — |
| Publisher, Sway | EON Publish | PLANNED | 0 % | `.pub` import is the urgent piece | P4 | `eon-office-publish` | — |
| Snipping Tool–class capture | EON Capture | PLANNED | 0 % | everything | P4 | `eon-office-capture` | — |
| M365 admin center | EON Admin | PLANNED | Core registry, audit, backup | organisation console needs server | P5 | `eon-office-admin` | core: registry/audit/backup tests |
| Intune | EON Endpoint | PLANNED | 0 % | agent, policies | P5 | `eon-office-endpoint` | — |
| Entra ID | *(integration)* | PLANNED as integration, product NOT PLANNED | Core directory + scoped roles (local) | OIDC/SCIM in server | P2 (server) | `eon-office-server` | core: directory tests |
| Defender | EON Security | PLANNED (integrations only) | 0 % | no own engine, by decision | P5 | `eon-office-security` | — |
| Purview | EON Compliance | PLANNED | Core audit chain (local) | retention, holds, eDiscovery | P5 | `eon-office-compliance` | core: audit tests |
| Priva | EON Compliance | PLANNED | 0 % | subject-request tooling | P5 | `eon-office-compliance` | — |
| Windows 365 / AVD | — | **NOT PLANNED** | — | integration guide with existing VDI | — | — | — |
| .NET / VS / VS Code / NuGet | EON Developer Tools | PLANNED | SDK exists | templates, environment helpers using official installers only | P5 | `eon-office-developer-tools` | — |
| Cross-app search (Microsoft Search) | Core search | **TESTED** (core) | Core | UI in Launcher | P0 | `eon-office-core` | `search_shows_only_what_was_shared` |
| Notification feed | Core notifications | **TESTED** (core) | Core | UI in Launcher | P0 | `eon-office-core` | `notifications_are_rate_limited_and_reach_the_owner` |
| Unified audit log | Core audit | **TESTED** (core, local) | Core | organisation-wide in server | — | `eon-office-core` | `capabilities_are_granted…`, audit unit tests |

## 4. Missing features in the initial application list

What the original EON Office directive did not list, and the audit added:

- **Notes:** handwriting/ink and audio notes (OneNote), web clipper as a browser extension,
  OCR of images in notes (with Capture).
- **Writer:** citations and bibliography, mail merge (with Contacts), document templates
  with content controls, accessibility checker, `.pdf` editing beyond export.
- **Sheets:** dynamic arrays, data validation, conditional formatting, a Power Query-like
  import pipeline (shared with Analytics), protected ranges, macros **only** after madde 4's
  sandbox exists.
- **Slides:** speaker coach-style rehearsal (no cloud requirement), live captions (with
  Meetings' transcription engine), export to video (with Media).
- **Mail:** shared mailboxes and delegation (server), focused inbox *as local rules*, not a
  cloud model; calendar invitations (iMIP).
- **Calendar:** scheduling assistant (free/busy across an organisation — server), room
  resources (Places/Bookings), meeting polls.
- **Tasks:** flagged mail as tasks, My Day, assignment and comments in shared lists.
- **Drive:** on-demand files (placeholder files) — OS-specific and hard; staged after basic
  sync.
- **Chat/Meetings:** recordings with consent indicators, transcripts, breakout rooms,
  webinars later.
- **Everything:** accessibility checker per document type; per-app data export (madde 31).

## 5. File format and interoperability strategy

The rule: **no compatibility claim without a corpus result**. `eon-office-formats` owns a
public test corpus of real documents (licensed for redistribution) and publishes, per
release, the share of documents that round-trip within tolerance.

| Format | Direction | Approach | Target for a `0.1` claim | Status |
|---|---|---|---|---|
| DOCX | import/export | Native Rust reader/writer for the common subset; **[A]** LibreOffice headless as an optional out-of-process converter for the long tail (MPL-2.0, separate process — no licence conflict) | ≥ 95 % of corpus open without error; ≥ 90 % visually equivalent on page count, headings, tables, images | PLANNED |
| XLSX | import/export | Native; formulas preserved as text even when not evaluated | ≥ 98 % open; formula results equal to Excel's stored values on ≥ 99 % of cells | PLANNED |
| PPTX | import/export | Native for layouts/text/images; animations best-effort | ≥ 95 % open; slide count and text exact | PLANNED |
| ODT / ODS / ODP | import/export | Native; ODF is an open standard and the long-term archival format | as above | PLANNED |
| PDF | export (import read-only) | Export via the engine's layout; import as view + annotation | PDF/A-2b export validates with veraPDF | PLANNED |
| CSV / TXT / Markdown / HTML / JSON / XML / SVG | import/export | Native, standard libraries | lossless where the format allows | PLANNED |
| ICS (RFC 5545) | import/export | Native | corpus from Outlook, Google, Apple; recurrence exceptions exact | DESIGNED (Calendar criteria) |
| VCF (RFC 6350) | import/export | Native | vCard 3.0 and 4.0 round-trip | DESIGNED (Contacts criteria) |
| EML / MIME (RFC 5322, 2045) | import/export | Established Rust crate **[A]** | corpus of 1,000 real messages incl. S/MIME | PLANNED |
| PST | import | `libpff` (LGPL) out-of-process | mail, calendar, contacts recovered | PLANNED (migration) |
| `.pub` | import | **[A]** `libmspub` (via LibreOffice) out-of-process | text and images placed; layout approximate | PLANNED (urgent window) |
| `.vsdx` | import | **[A]** `libvisio` | shapes and connectors | PLANNED |
| `.accdb` / `.mdb` | import | **[A]** `mdbtools` | tables and data exact; queries as SQL text; forms/reports not converted | PLANNED |
| `.mpp` | import | **[A]** MPXJ (LGPL, JVM) out-of-process, or Project's XML export | tasks, dependencies, resources, baselines | PLANNED |
| SQL dumps, SQLite | import/export | Native | lossless | PLANNED |
| Images/media | import/export | Established decoders; ffmpeg out-of-process for media | — | PLANNED |

Out-of-process converters keep the GPL/LGPL/MPL boundaries clean and keep a crashing
converter from taking an application with it — the same reasoning as the core's process
model.

## 6. Migration strategy

Principles: **read-only against the source**, **verify before switching**, **report
everything**, **coexist** — EON never asks a person to delete their Microsoft data.

| Source | Path | Validation |
|---|---|---|
| Office documents (DOCX/XLSX/PPTX/.pub) | Bulk import through `eon-office-formats`; originals kept | Per-file report: opened, warnings, fidelity score |
| OneDrive / SharePoint files | Microsoft Graph API (delegated OAuth, read-only scopes) → EON Drive | Count and SHA-256 per file; version history optional |
| Outlook mail | IMAP (simplest) or Graph; PST via `libpff` | Message counts per folder; spot-check hashes of bodies |
| Outlook calendar / contacts | Graph or ICS/VCF export | Event counts incl. recurrence exceptions; contact field coverage |
| Planner / To Do | Graph → EON Tasks | Task, checklist and assignment counts |
| Project Online (retiring 30 Sep 2026 [V]) / `.mpp` | Project XML or MPXJ → EON Projects | Schedule recomputed and compared: finish dates, critical path |
| Access | `mdbtools` → EON Database | Row counts and checksums per table |
| Teams chat | Only where Microsoft's export APIs and the organisation's own policies allow; otherwise the organisation's compliance export | Honest report of what could not be exported |
| Power BI | Re-point data sources; visuals recreated (no faithful `.pbix` conversion is claimed) | Report listing each visual and its EON equivalent or gap |
| Identity (Entra ID) | No migration of credentials ever; SCIM provisioning or OIDC federation to the organisation's IdP | User and group counts |

`eon-office-migrate` becomes its own repository when the second source is implemented, not
before (madde 43d).

## 7. Where EON aims to be better

- **Data stays where the person chooses**: local by default; self-hosted next; any EON-run
  service only behind madde 30's gate.
- **Install only what you need**: each product is a separate download; nothing is bundled.
- **Visible permissions**: every capability an application holds is in its manifest and in
  the launcher, revocable at once.
- **No telemetry, no training on users' data, no mandatory AI, no mandatory account.**
- **Open formats as the archive** (ODF, Markdown, ICS, VCF) with Microsoft formats as
  first-class import/export.
- **Turkish done right** (madde 40) — search, sorting and casing — and every interface in
  Turkish and English from its first release.
- **Lightweight**: Rust and the system webview; a target of < 1 s start-up for daily-use
  applications, measured in each product's acceptance criteria.

## 8. Keeping this document honest

- A row's status changes only in the same pull request as the evidence (a release, a test
  report, a corpus result).
- Every **[K]** row is a standing task: replace it with **[V]** and a link, or correct it.
- Re-audit Microsoft's product list every six months; retirements and renames are dated
  here when found.
