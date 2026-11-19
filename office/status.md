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
| `eon-office-spec` | 0.1.0 (contracts v0) | ✓ public | ✓ green | schemas + examples validated in Node; links checked |
| `eon-office-core` | 0.1.0 | ✓ public | ✓ green (Windows, macOS, Linux; deny; REUSE; invariants; spec agreement) | 5 dependencies |
| `eon-office-sdk` | 0.1.0 | ✓ public | ✓ green (reference integration on three OSes) | 1 dependency |
| `eon-docs` | plan v0.8 | branch `docs/madde-43-office`, PR #2 | — | awaiting the owner's merge |
| everything else in the roadmap | — | not created | — | by rule: no empty repositories (madde 43d) |

## Test status

| Suite | Count | What it proves |
|---|---|---|
| core unit tests | 46 | clock ordering, Turkish folding, migrations, manifest and semver rules, audit tamper detection, RBAC table and inheritance, revision conflicts, two-device sync convergence with the loser kept, search visibility, rate limit, job coalescing, contrast ratios |
| core protocol tests | 16 | the real binary over a bare socket: authentication, capabilities checked per call, storage isolation, declared events, calls between apps, a provider crashing mid-call, notifications, search, jobs, preferences, supervisor restarts and give-up, forced stop, sync between two cores, directory, oversized messages, uninstall, backup/restore and one-core-per-home |
| core ↔ spec agreement | 2 | the spec's 25 examples mean the same to the Rust readers as to the JSON Schemas |
| spec validation | 25 examples | 8 valid accepted, 17 invalid rejected |
| SDK | 3 unit + 1 reference + 1 doctest | an app launched by the core, a second app reaching it only through the core, and graceful behaviour with it absent |
| Mutation check (manual) | — | disabling capability enforcement in the core fails 3 protocol tests |

## Technical decisions taken (and where they are argued)

- The core is a **local service**, applications are **separate processes** — `eon-office-core/docs/architecture.md`.
- **Loopback TCP + JSON-RPC 2.0, one object per line**; threads, no async runtime — same.
- **SQLite**, one connection, WAL; migrations refuse newer schemas — same.
- **Hybrid logical clocks, last-writer-wins per document, losers kept** — `eon-office-spec/docs/sync.md`.
- **Home in the per-user data directory**, unlike EON Stream's beside-the-executable rule, because many programs must find one core — `eon-office-core/src/lib.rs`.
- **Coarse, owner-granted capabilities** — `eon-office-spec/docs/capabilities.md`.
- Product merges and splits (BI → Analytics, Admin Center → Admin, Device Manager → Endpoint, Defender → Security, no Identity product, no Cloud PC) — `roadmap.md` §3.

## Current work

Nothing is half-done in a repository. The next items, in order:

1. **EON Launcher** (`eon-office-launcher`) — the owner's shell, so nobody needs a terminal
   to install or grant (Gate 0 → 1).
2. **Marketplace** index format and signature checks (madde 34 mechanism; empty trust set).
3. **Spec additions** for the launcher: the `activity.*` event convention (recent items,
   activity feed) and the `eon-office://` deep-link format.
4. **`eon-office-engine` spike**: `yrs` vs `automerge`, two weeks, measured.

## Blockers and open questions

| Item | Blocks | Owner |
|---|---|---|
| eon-docs#2 (madde 43) merge | nothing technically; it is the record of the decision | project owner |
| madde 30 Faz L1 gate (legal entity) | any EON-operated Office service; application signing keys | project owner |
| madde 32g name availability (`EON Notes`, `EON Writer`, …) | public launch of product names | project owner |
| madde 33 licence confirmation | first outside contribution to Office repositories | project owner |
| `eon-office-engine` library choice [A] | Writer, Sheets, Slides, Whiteboard, Loop | spike |
| LibreOffice as optional converter [A] | the long tail of DOCX/PPTX fidelity | formats research |

## Next milestone

**Gate 0 → 1:** a person installs, grants, runs and removes an EON Office application
without a terminal, through EON Launcher, and the launcher passes the madde 35 keyboard and
screen-reader checklist.
