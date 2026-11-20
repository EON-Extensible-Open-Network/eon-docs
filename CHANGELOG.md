# Changelog

## [Unreleased]

### Plan v0.10 — 20 November 2026

One new item, madde 45. This branch also carries v0.8 (madde 43, pull request #2) and v0.9 (madde 44, pull request #3).

- **madde 45: every service is its own product.** Files, audio and streaming are no longer modules of one application: each has its own repositories, its own core and its own application. EON Stream is video (and stays EON Edu's core); files are EON Download Manager; audio becomes a separate product, deferred, its name and scope to be researched. A product may use another product's core only where that is more efficient than duplicating it, and no core grows for another product's needs.
- **What it costs, stated in the item:** some infrastructure is written per product, more repositories to maintain, and the shared-engine benefit of madde 28 stays between Stream and Edu.
- Records added where the old plan said otherwise: the vision, the priority order, madde 3, 15a, 41, phase 2, rejected alternatives, open items.

### Plan v0.9 — 20 November 2026

One new item, madde 44 (43 is EON Office, v0.8, pull request #2).

- **madde 44: EON Download Manager.** A download manager that does what people value in
  Internet Download Manager, free for individuals, open source, on Windows, Linux and
  macOS. No subscription or licence code in the application, ever; organisations may later
  buy lifetime licences through a separate marketplace repository. No site-specific video
  extraction and nothing DRM-protected. Research, analysis and plan in
  `download-manager/`.
- **What it costs, stated in the item:** a third product family before EON Stream v1,
  which madde 42c warns against; Stream's installer and signing keys wait meanwhile.

### Plan v0.8 — 19 November 2026

One new item number: **madde 43**.

- **madde 43, EON Office: a third product family, started before EON Stream `v1`.** The
  owner's decision. The first work is the shared foundation — `eon-office-spec`,
  `eon-office-core`, `eon-office-sdk` — and the roadmap; applications follow in the
  roadmap's order. EON Office has its own core and does not depend on EON Stream's.
- **It is written against madde 42c's reasoning, on purpose.** 42c covers the kernel and
  the distribution, not Office, but its reason — a one-person team starting a second
  product is the most common reason the first is never finished — applies word for word.
  The item does not argue it away; it states the cost.
- **What it costs, stated in the item.** EON Stream `v1` (installer, signing, the keyboard
  and screen-reader half of madde 35) shares the same hours, and a `v0.X` release wins any
  clash. The maintenance surface grows; "no empty repositories" limits it. Every hosted
  Office service is behind madde 30's Faz L1 gate, so local and self-hosted come first and
  there is no EON cloud until then. Microsoft parity is a direction, not a commitment.
- **The vision line changes** from "one roof, one core, two products" to name Office's own
  core; the old line is recorded beside the new one.
- **`office/`** holds the EON Office roadmap (products, merges and splits, waves with
  measurable gates, acceptance criteria), the Microsoft audit with a coverage and parity
  matrix whose every claim is labelled verified or not, and the project status.

### Plan v0.7 — 17 November 2026

No new item numbers. v0.7 changes the player-strategy half of madde 2.

- **madde 2, where the picture goes: inside the EON Stream window from `v0.16`.** The
  previous decision put the picture in mpv's own window for `v1` and left a single window to
  a later libmpv slice. The owner asked for it now. It is not libmpv: mpv is still a
  separate process driven over JSON IPC, handed the window's handle with `--wid`. The
  controls are a transparent second webview added after mpv's surface, and the catalogue's
  webview is hidden while something plays — z-order solved by sequencing, with no `unsafe`.
- **What it costs, stated in the item.** Measured on Windows only; macOS and Linux need
  their own check before the claim extends to them, and fall back to mpv's own window
  until then. Tauri's `unstable` feature is now on (two webviews in one window), which can
  break on a Tauri upgrade. The interface surface madde 4 watches grew by one page, with
  the same commands and no network permission. The in-window behaviour has no automated
  test; it is checked by screen capture.

### Plan v0.6 — 17 November 2026

No new item numbers. v0.6 changes the timing half of one decision and updates the status
table under it.

- **madde 2, when the interface arrives: during the alpha, not at `v1`.** The previous
  decision was that the window came last and the whole alpha ran in a terminal, because a
  half-generated Tauri scaffold rots and hides which decisions are real. It shipped at
  `v0.14` instead, at the owner's decision. What the old reasoning was protecting still
  holds and is recorded as such: the window was written against a system already proven at
  a prompt, and the headless build is still published beside it as `eon-stream-cli`.
- **What it costs, stated in the item.** A second surface to maintain until `v1`: every
  behaviour change is now answered twice, and modules, the update check, subtitle search
  and deep links live only at the prompt for now. madde 35's keyboard and screen-reader
  criteria stop being deferred work and become open work that has to be measured — the
  contrast and text-scaling half is met, the other half is a `v1` exit criterion.
- **The eight-item table is now as of `v0.14`:** item 8 (Turkish and English) is done
  including the interface; what is left is the installer, the signing keys, and the parts
  of the window that still send a person to the prompt.

### Plan v0.5 — 4 October 2026

No new item numbers. v0.5 closes items that were open and marks the Faz 0 outputs; a number
never changes meaning, so a closed decision is an edit to its own item rather than a new
one.

- **madde 1, torrent engine: librqbit.** Decided and implemented. What settled it was not
  BitTorrent in general but the one behaviour needed here — a piece window kept ahead of
  the read position, re-queued around a seek. The item records why writing it ourselves
  would have meant DHT, uTP, web seeds and tracker protocols first, to arrive at the same
  place.
- **madde 1, the loopback server.** The open half of the question — whether it should also
  serve non-torrent HTTP sources — is closed by not doing it: an HTTP source is already
  seekable, so proxying one would add a hop, a buffer and a second thing that can fail, to
  replace what mpv does natively.
- **madde 34, canonical serialisation: JCS (RFC 8785).** Pinned, with the reasoning that
  the contract's own warning was correct and that *before any key exists* was the cheapest
  moment to settle it. The item now also records the client-side implementation and the
  state that matters most: **no key in the hierarchy has been generated**, so every build
  ships an empty trust set and installs nothing, by design.
- **Faz 0 outputs:** seven of eight checked. The threat model (madde 37) is the one
  outstanding. The Tauri shell is marked as deliberately absent rather than pending — the
  whole alpha line runs in a terminal and the interface arrives at `v1`.
- **A status table for the eight v1 items**, so "what is left" is one place rather than an
  inference: the interface, the installer, and the signing keys.

### Plan v0.4
- **madde 1, protocol layer: ours.** Verified end to end against Cinemeta and
  OpenSubtitles v3; `stremio-core` stays a behaviour reference, not a dependency.

### Added
- `plan/eon-plan.md` v0.2: status labels on every item; legal section rewritten around
  three principles (separate the roles, stage the obligations, build the process on the law
  actually applying); items 29-41 added (legal positioning, obligation phases, data
  protection, brand posture, licence and governance, key management and revocation,
  accessibility, telemetry, threat model, sustainability, update security, i18n and offline,
  non-goals); phase plan, v1 definition of done, rejected-alternatives table.
- `policies/README.md` with the policy versioning rules and the planned set.
