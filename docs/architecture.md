<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON — architecture

**20 November 2026.** How EON is put together, in English. The decisions themselves are
in the Turkish [plan](../plan/eon-plan.md); the numbers in brackets (madde N) point there.

## The rule: one product per service

EON is an umbrella (madde 32, 42). Under it, **every service is its own product** (madde 45):

| Service | Product | Repositories | Core |
|---|---|---|---|
| Video | **EON Stream** | `eon-stream-spec`, `-core`, `-engine`, `-app`, `-sdk`, `-marketplace`, `-community` | `eon-stream-core` (+ `eon-stream-engine`) |
| Schools | **EON Edu** | built from `eon-stream-app`; `eon-edu-server` | shares EON Stream's core — the one deliberate sharing (madde 22, 28) |
| Files | **EON Download Manager** | `eon-download-manager-spec`, `-app` | the engine crate in `eon-download-manager-app` |
| Productivity | **EON Office** | `eon-office-spec`, `-core`, `-sdk`, `-launcher`, `-marketplace`, `-notes`, `-tasks`, `-contacts`, `-calendar` | `eon-office-core` |
| Audio | not yet named | none until it has a first working version (madde 43d) | — |

What follows from the rule:

- **A person installs what they use.** No product needs another to run.
- **Cores are libraries, not services.** An application links its core and runs on its own;
  nothing waits on a background process (madde 43, the 19 November record).
- **A core is shared only where that is more efficient** than duplicating it, and the reason
  is written where the dependency is declared (madde 37). No core grows for another
  product's needs.
- **What products share is contracts and umbrella decisions**, not code: licensing (madde 33),
  signing (34), accessibility (35), no telemetry (36), localisation (40), naming (32a).
- **Products meet through documented contracts** — a deep link, a hand-off message, a
  published file format — never through each other's internals.

## The shape of a product

Every product follows the same shape, which keeps each one small enough to understand on its
own:

```
eon-<product>-spec    contracts: JSON Schemas + prose, valid and invalid examples,   Apache-2.0
                      validated in CI. Written before code; v0 is unstable.
eon-<product>-core    the library: no GUI dependency, panics denied, tested alone    GPL-3.0-or-later
                      (in a small product it may be a crate inside -app)              + module exception
eon-<product>-app     the window: Tauri over the system webview, static HTML/CSS/JS,  GPL-3.0-or-later
                      no bundler; --selftest; a window check over DevTools             + module exception
eon-<product>-sdk     for people building on it                                        MIT
eon-<product>-server  if it ever needs one                                             AGPL-3.0-or-later
```

The licences are not uniform by accident: copyleft protects the cores, permissive contracts let
anyone implement the other side, the MIT SDK means an author never meets a licence question, and
AGPL is where copyleft has teeth (madde 33). The EON Module ABI Exception exists from the first
commit, because the DCO makes adding it later impossible.

## Rules every product keeps

- **The interface has no network of its own and holds no secrets.** It reaches its host only
  through registered commands; CI fails a build where the page calls `fetch` or receives an
  addon address, a token or a credential. `--selftest` checks that every command the page
  invokes is registered.
- **Untrusted text never becomes markup** (`textContent` only; CI checks).
- **Every dependency is justified where it is declared** and checked by `cargo deny`.
- **Commits are signed off** (DCO) and follow Conventional Commits with the component as scope.
- **A release is a tag, built by CI** with the pinned toolchain, self-tested on the binary that
  ships, published with `SHA256SUMS.txt` — and downloaded and checked afterwards.

## Product by product

### EON Stream and EON Edu

A media viewer where everything beyond the core is a module (madde 3): the core is the module
manager, the addon protocol client, signature verification and the updater; `eon-stream-engine`
is BitTorrent with a sequential window, a loopback HTTP server and mpv over JSON IPC. The app
builds two binaries (window and headless). EON Edu is the same code with the open marketplace,
arbitrary addon URLs and open BitTorrent **absent at compile time** — CI checks the binary for
the forbidden symbols (madde 22).

### EON Download Manager

One workspace: `engine/` (dynamic segmentation, resume from an atomic state file, speed limits,
HLS — a library with no GUI dependency), `src-tauri/` (the window, the tray, and the browser's
native messaging host in the same executable), `src/` (the interface) and `extension/` (Manifest
V3 for Chromium and Firefox). The browser hand-off and the resume state are contracts in
`eon-download-manager-spec` (madde 44). Free for individuals; no licensing code in the
application, ever.

### EON Office

Separate applications on `eon-office-core`, a library that opens a shared folder per person:
each application's own store, a shared search index, preferences, a recent list and published
summaries other applications may read with the person's permission (madde 43). Applications
meet through `eon-office://` links and published data, never by calling each other.

## Where to read more

| | |
|---|---|
| The decisions | [plan/eon-plan.md](../plan/eon-plan.md) (Turkish) |
| EON Download Manager | [download-manager/](../download-manager/) — research, plan, status |
| EON Office | [office/](../office/) — roadmap, Microsoft parity audit, design notes, status |
| EON Stream | the READMEs and changelogs of `eon-stream-app`, `-core`, `-engine` |
