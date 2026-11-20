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

## Next, in the order the owner gave

1. **Catching browser downloads when clicked** — the owner's test found it not working outside
   the automated check; diagnose on their machine first.
2. **Settings that apply while a download runs** — connections per download, downloads at the
   same time and the speed limit change the running transfer in place, without losing data.
3. **Format choice** — keep the original or ask; when asking, convert, keep audio only or
   video only, or save a GIF as a picture.
4. **Downloading from video and audio platforms** from a link, within the boundary of madde 9
   and with no DRM circumvention; madde 44b to be updated.

Then: EON Account (a new item in the plan).
