<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Download Manager — plan

**20 November 2026.** Step three of four, after [research.md](research.md). Decision: plan
madde 44. Goal of this plan: the first alpha, **`v0.0.0`**, published and verified.

## Shape

| Repository | What | Licence |
|---|---|---|
| `eon-download-manager-spec` | Contracts: the browser hand-off messages, the resume-state file, JSON Schemas with valid and invalid examples checked in CI. Permissive so any browser extension or tool can implement them. | `Apache-2.0` |
| `eon-download-manager-app` | A Cargo workspace: `engine/` (the download engine, a library with no GUI dependency, as `eon-stream-core` is), `src-tauri/` (the window), `src/` (the interface: static HTML, CSS, JavaScript, no bundler), `extension/` (Chromium and Firefox). | `GPL-3.0-or-later` + EON Module ABI Exception |
| `eon-download-manager-marketplace` | Later. Organisations' licences and the add-on index live there; **the application contains no subscription or licence code** (madde 44). Not created until it has a first working version. | — |

Stack: Rust, Tauri 2 over the system webview (as every EON window), `tokio` + `reqwest`
(rustls) in the engine. One binary per platform, `eon-download-manager`.

Data: the per-user data folder (`…/EON/Download Manager/`), like EON Office and unlike
EON Stream's beside-the-executable rule: a download manager is installed once per person
and its browser hand-off is registered per user.

## The engine

**Probe.** `GET` with `Range: bytes=0-0` (servers that refuse `HEAD` are common): final
URL after redirects, length from `Content-Range`, range support, name from
`Content-Disposition` (RFC 6266, including `filename*`) or the path, type, `ETag`,
`Last-Modified`.

**Dynamic segmentation (IDM's rule).** Start with one connection; open more one at a time
(every ~150 ms) up to the download's limit (default 8, at most 32). Each time a slot is
free, take the segment with the most bytes left and split what is left of it in half; the
new connection starts at the midpoint, the old one stops there. Never split below
2 × 256 KiB left. A finished connection frees a slot, so fast connections help slow ones.
Requests are `Range: bytes=pos-end`; a connection that reaches its end returns to the
keep-alive pool and is reused by the next request to that host. The planner is a pure
function, tested on its own.

**Adapts to the server.** `429`/`503` or refused connections while more than one is open
lower that host's connection limit for the session — IDM's per-server exception table,
found instead of configured.

**Writes in place.** One `.part` file pre-sized to the length, written at offsets; a
`.eondl` state file beside it (format in the spec) with every segment's position, written
atomically **at least once a second** and on every pause. On completion: truncate, rename
(never overwrite — `name (1).ext`), SHA-256 computed, state deleted.

**Resumes anything.** On resume each request carries `If-Range` with the stored validator;
a `200` instead of `206` means the file changed and the download restarts (or, with
"ignore changes", is trusted). Without range support or length: one connection, marked
*can't resume* in the interface before you start.

**Refresh address.** A download's URL can be replaced — pasted, or sent again from the
browser — keeping every byte, if the new address reports the same length.

**Speed limit.** A token bucket, global and per download.

**Retries.** Per segment with backoff (1 s, 2 s, 4 s … 30 s), then the download stops in
*error* and can be resumed or refreshed.

**HLS video.** Master playlists list their variants (resolution, bitrate) for the person
to choose; media playlists download in parallel into a segment folder, AES-128 decrypted,
joined at the end into `.ts` (or `.mp4` for fMP4 with `EXT-X-MAP`); remuxed to `.mp4`
with `ffmpeg` only if the person has it — EON ships no `ffmpeg`. Live and DRM streams are
refused with a reason.

**Also in `v0.0.0`:** categories by extension with their own folders; numbered batch
patterns (`photo[001-120].jpg`); a queue with a concurrency limit; a scheduler (start and
stop times, days) with an after-queue action (nothing, sleep, shut down after a 60-second
countdown you can cancel); clipboard watching; SHA-256 compare.

## Browser hand-off

The extension (Manifest V3, Chromium and Firefox from one source):

- **Takes over** a browser download whose type or size matches the settings: cancels it in
  the browser and sends URL, final URL, referrer, file name, MIME type, size and that
  site's cookies to the application. **Alt** while clicking leaves it to the browser;
  an address exception list with wildcards.
- **Context menu:** *Download with EON*, *Download all links* (selection or page).
- **Video:** watches what the page itself loads — HLS playlists, direct media files above a
  size — and shows a *Download video* pill over the playing `<video>`, in a shadow root so
  page styles cannot reach it. No site-specific extraction.

Transport: **native messaging**. The browser starts `eon-download-manager` with the
extension's origin as its argument; in that mode it reads the one message, writes it to a
private file in the data folder, starts (or forwards to, through the single-instance
plugin) the running window with that file, answers `{ "ok": true }` and exits. Only the
listed extension IDs may talk to it. The application registers itself as a native host
**when the person presses *Connect browsers*** — never silently.

## The interface

### Direction

The subject is *transfer*: data arriving in pieces over several connections. IDM draws
those pieces as thin pink and blue lines; that drawing is the most honest picture of what
the program does, and it becomes the centre of the window. Everything else is quiet.

**Colour** (dark is the default; a light set exists):

| Name | Dark | Light | Job |
|---|---|---|---|
| Ink | `#15132B` | `#F7F6FC` | background |
| Deck | `#1F1C3D` | `#FFFFFF` | the one raised surface (the detail deck, sheets) |
| Mist | `#ECE9FB` | `#1B1838` | text |
| Haze | `#A7A2CC` | `#5B5680` | secondary text |
| Current | `#4FD8E8` | `#0A8FA3` | bytes that have arrived |
| Pulse | `#A08BFF` | `#5B45E0` | actions, selection, focus |

Connection lanes cycle Current, Pulse, rose `#FF8FC7`, amber `#FFC861` — colour that
*means* "this is connection 3", never decoration. Done `#74E3AE`, error `#FF7A7A`.

**Type:** Geist (SIL OFL), bundled, one family. Speeds and sizes in tabular figures.
The live speed of the selected download is set large and light (Geist 200, tight
tracking) — the one typographic moment.

**Layout:**

```
┌─ rail ─┬─ list ─────────────────────────────┬─ deck (selected) ───────────────┐
│ All    │ [ Paste a link, press Enter      ] │ ubuntu-24.04-desktop.iso        │
│ Active │ ───────────────────────────────── │ 18.4 MB/s          1:12 left     │
│ Queued │ ubuntu-24.04-desktop.iso   64 %   │ ▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀ │
│ Done   │ ▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▂▁▁▁▁▁▁▁▁▁▁▁▁▁ │  lane map: one row per          │
│ Failed │ 4.1 of 6.2 GB   18.4 MB/s   1:12   │  connection, splits visible     │
│        │                                    │ ╱╲╱╲ speed, last 60 s           │
│ Video  │ ...                                │ 8 connections · resumable       │
│ Music  │                                    │ Saved to  Downloads\Programs    │
│ …      │                                    │ [Pause] [Show in folder] […]    │
│ ────── │                                    │                                 │
│ ↓ total│                                    │                                 │
└────────┴────────────────────────────────────┴─────────────────────────────────┘
```

Left-aligned throughout. The list is a list, not cards: dense rows, a thin lane bar under
each active one. Adding a link — pasted, caught from the browser or the clipboard — opens
one sheet from the top: the file as it will be saved (name, size, type, folder by
category, *resumable* or not), *Start* and *Later*.

**Principles.**

1. The lane map is the identity; spend boldness there and nowhere else.
2. Every number a person watches (speed, size, time left) is tabular and never jumps.
3. Motion only answers something: a lane peeling off when a segment splits, a sheet
   opening, a row settling when done. Reduced motion turns all of it off.
4. Words are the person's: *Pause*, *Resume*, *Show in folder*, *Give it a new link*.
   No middle-dot meta strings, no capital-letter labels, no arrows in buttons.
5. Keyboard first: every action has a key, focus is always visible.

### Checked against the defaults

A first pass came from the design-system generator: slate `#0F172A` with a green "run"
accent and JetBrains Mono / IBM Plex Sans — the generic developer-tool look, and a
monospace face for data labels is one of the tells. Replaced with the violet-ink set
above, whose colours carry the per-connection meaning, and one proportional family with
tabular figures. The detail panel was first sketched as a grid of stat cards; that is the
SaaS-card kit, so it became one deck with the lane map at the top and plain text below.

## Done means

- Engine tests against a local test server that supports ranges, drops connections,
  throttles and changes its file: multi-connection result hashes equal the source; a
  download killed mid-way resumes from its state file to the same hash; a server with no
  ranges still completes; a changed file restarts; a refreshed address keeps the bytes;
  HLS with AES-128 decrypts to the source.
- `--selftest` on the published binary; a window check over the DevTools protocol that
  adds a download from a local server, watches lanes split, pauses, resumes, finishes and
  verifies the hash.
- Screenshots reviewed against this plan before the tag.
- `v0.0.0` released by CI for Windows, Linux and macOS with `SHA256SUMS.txt`, plus the
  extension as a zip; downloaded and checked after publishing.

## After `v0.0.0`

DASH; site grabber; synchronization queues; FTP; quotas; per-site logins; the extension in
the Chrome, Edge and Firefox stores (needs the owner's developer accounts); signing (madde
30, 34); the marketplace repository.
