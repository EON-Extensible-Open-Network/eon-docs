# Changelog

## [Unreleased]

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
