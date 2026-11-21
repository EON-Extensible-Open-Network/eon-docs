<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Download Manager — status

**As of 21 November 2026.** Plan madde 44; [research](research.md), [plan](plan.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-download-manager-spec` | contracts v0 | green | hand-off and resume-state schemas; 10 valid and 14 invalid examples |
| `eon-download-manager-app` | `v0.0.2`, prerelease | green on Windows, Linux, macOS | release downloaded after publishing: checksums match, the Windows binary passes `--selftest` |

**Tested:** 32 engine tests against a local HTTP server — multi-connection splitting byte for
byte, resume after a simulated power cut (mutation-checked), pause and resume, a server without
ranges, a file that changed, an expired link replaced, a server that limits connections, the
speed limit, encrypted HLS, a queue that survives a restart; `--selftest` with a real download;
26 checks driving the real window; and, by hand, a real Microsoft Edge handing a real download
to the application through the extension (`tools/browser-check.mjs`).

**The owner's call (20 November):** no public launch until beta; alpha development continues.

## The owner's queued requests (20 November)

1. **Catching browser downloads when clicked: done** (pushed). The window registers the
   browser host at every start, and a first-start walkthrough opens the browser's
   extensions page and shows "Connected" once the extension answers. Checked in real Edge.
   **Firefox (22 November, `v0.0.2`):** release Firefox installs only Mozilla-signed
   add-ons, so the extension is signed on addons.mozilla.org as *unlisted* (in no
   catalogue). `v0.0.1`'s signed `.xpi` is on its release. The `firefox` workflow signs
   later versions once the owner adds `AMO_JWT_ISSUER` / `AMO_JWT_SECRET`. Installed in the
   owner's Firefox, it caught downloads, but the window never received them: the host's
   second-copy route failed under Firefox. Since `v0.0.2` the window reads the inbox itself,
   and the host answers *ok* only once the window has the hand-off. Verified in a real
   Firefox 160, with the window running and with it closed.
2. **Settings that apply while a download runs: done** (pushed). Connections, downloads
   at the same time and the speed limit change the running transfer in place. A file
   downloaded across 8 → 2 → 6 connections still matched its SHA-256.
3. **Format choice: done** (in `v0.0.1`). Keep the original, or ask. Asking offers sound
   only, a silent picture, MP4/MKV, and a GIF as a still picture or a video, all through
   FFmpeg.
4. **The video or sound a page plays: done** (in `v0.0.1`). It goes through a general
   extractor.

   The real-network test passed on 21 November, run at the owner's request:
   - FFmpeg and the extractor were fetched and checked;
   - a real video page downloaded as MP4 and as MP3, each with its SHA-256;
   - a GIF converted;
   - qualities up to 2160p were on offer, and a 1080p clip downloaded.

   The extractor's newest versions also want a JavaScript runtime for one large site.
   Without one, every quality still came through; if that changes, the runtime is the
   next helper to fetch the same way.

Then EON Account (madde 46). It is written and in EON Stream `v0.31`/`v0.32`.
