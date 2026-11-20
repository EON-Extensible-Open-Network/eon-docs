<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Download Manager — status

**As of 20 November 2026.** Plan madde 44; [research](research.md), [plan](plan.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-download-manager-spec` | contracts v0 | green | hand-off and resume-state schemas; 10 valid and 14 invalid examples |
| `eon-download-manager-app` | `v0.0.0`, prerelease | green on Windows, Linux, macOS | release downloaded after publishing: checksums match, the Windows binary passes `--selftest` |

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
2. **Settings that apply while a download runs: done** (pushed). Connections, downloads
   at the same time and the speed limit change the running transfer in place. A file
   downloaded across 8 → 2 → 6 connections still matched its SHA-256.
3. **Format choice: written, waiting on one test.** Keep the original, or ask. Asking
   offers sound only, a silent picture, MP4/MKV, and a GIF as a still picture or a video,
   all through FFmpeg.
4. **The video or sound a page plays: written, waiting on the same test.** It goes
   through a general extractor. The test is the real-network one: it fetches and checks
   the helpers, downloads a short page as video and as sound, and converts a GIF.

   Both (3) and (4) are on the branch `feat/formats-and-pages`. The test was not run
   here, because this environment's permissions refuse running downloaded programs. Run
   it with `cargo test --test media -- --ignored` in `engine/`, with `EON_DM_MEDIA_URL`
   set to a short public page. Madde 44b is updated in plan v0.12.

Then EON Account (madde 46). It is written and in EON Stream `v0.31`/`v0.32`.
