<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — status

**As of 28 November 2026.** Plan madde 50; [research](research.md), [plan](plan.md),
[design](design.md), [parity](parity.md), [extensions](extensions.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-browser-core` | `v0.0.0` tagged; extensions stage 1 on `main` | green on Windows, Linux, macOS, with the real default lists | blocking, link cleaning, HTTPS-only, address bar, search engines, profile, user agent; extension packages, manifests, permissions in words, match patterns, `declarativeNetRequest` → the shield. No engine, no GUI, no network client |
| `eon-browser-app` | `v0.0.4` published and verified (`v0.0.3` had a Tor defect, disclosed on its release) | green on Windows, Linux, macOS (Windows without the offscreen self-test, see below) | Servo 0.8.0; the window drawn by Servo (sidebar, bar, panels, overlay); tabs that sleep, history, bookmarks, session, private window; the route (Tor through Arti, Tor's own proxy, SOCKS5 or HTTP servers) with download/upload limits; memory limit and monitor; command bar; extensions stage 1; `--selftest` 49 checks |

## Verified

- **v0.0.0 published file (Windows)**: SHA-256 matches `SHA256SUMS.txt`; `--selftest`
  26/26 on this machine; BBC News opens. **Defect found by that check**: the Windows file
  carried no blocking lists (the release script passed a POSIX path the build could not
  read). Disclosed in the release notes; fixed in v0.0.1, where the build refuses to finish
  without lists and the self-test checks they are there.
- **Tor** (v0.0.1): Arti bootstraps in 2–8 s on the owner's connection;
  check.torproject.org inside EON Browser says "Congratulations. This browser is configured
  to use Tor."; a site keeps its exit, another site gets another, New identity changes it;
  the route process refuses programs without the browser's secret and exits with the
  browser.
- **Extensions** (v0.0.1): the real uBlock Origin Lite (CRX3 from the Chrome Web Store)
  installs; 117,636 of its rules run in the shield, 1,147 are not translated yet
  (redirects, header changes, and rules with conditions the translator does not know —
  skipped rather than widened); on BBC News its rules alone block 21 trackers (release
  build). A debug build compiles that many rules too slowly for BBC's scripts; release
  builds load them from the cache.
- **Blocking** (core, real lists): compile 107 ms, from cache 18 ms, **5.2 µs per
  request**.
- **The release build** (Windows 11, lists inside): 126 MB; window in **1.1 s**; BBC News
  11 trackers blocked; **380 MB working set** for that one page — not light yet; memory is
  a v0.0.x target.

- **The v0.0.3 window** (debug build, Windows 11, real GPU): the three views composite
  in place (interface, page, overlay); Wikipedia draws beside the sidebar. Self-test 49/49:
  the page's place, a SOCKS5 server on 127.0.0.1 given the site's *name* (no local
  lookup), a tab put to sleep and woken at its address, the monitor's memory reading,
  limits set by clicking their buttons and kept, `Ctrl+K` running a command found by
  typing and the panel narrowing the page.

- **v0.0.3 published file (Windows)**: SHA-256 matches; `--version` 0.0.3-alpha;
  self-test 49/49 with 7 lists inside; BBC News in the window, 11 trackers blocked, 350 MB
  working set (v0.0.0: 380 MB). **Defect found by that check: no page opened over Tor**
  (`client error (Connect)`). The route's new meter wrote without flushing; Arti's stream
  holds bytes until flushed, a TCP stream does not, so only Tor broke and no test saw it.
  Fixed in v0.0.4 with a unit test that fails without the fix; check.torproject.org in
  the fixed window says "Congratulations".
- **v0.0.4 published file (Windows)**: SHA-256 matches; `--version` 0.0.4-alpha;
  self-test 49/49, 7 lists inside; from a fresh Tor folder, check.torproject.org says
  "Congratulations" within 30 s; BBC News opens directly (354 MB working set).

## Not yet (each is in the roadmap)

- **No sandbox on Windows, no site isolation** (Servo). Not for banking yet.
- **Windows CI cannot run the offscreen self-test**: Servo's GL layer needs
  `WGL_NV_DX_interop`, which GPU drivers have and a GPU-less runner does not. Windows
  releases are self-tested on a real machine. ANGLE (v0.0.2) lifts it.
- **No video or audio, no WebGL** — v0.0.2.
- **YouTube fails inside Servo's HTTP client** (`client error (SendRequest)`), not in the
  network: curl through the browser's own route proxy gets 200 at the same moment.
  HTTP/2 is the first suspect; to reduce and report upstream (madde 50j).
- **`Sec-GPC` header not sent**: Servo's embedding API cannot add request headers; the
  script-side `navigator.globalPrivacyControl` is set. Raise upstream.
- **Permission prompts**: everything is refused until the prompt exists.
- **Select boxes, context menus, file pickers and dialogs** are not shown yet (the
  interface uses buttons wherever it would have used a drop-down list).
- **No CPU limiter** (Opera GX has one): slowing Servo's threads would stall pages rather
  than lighten the browser; the memory limit and sleeping tabs are the levers that work.
- **Extensions**: storage is in memory, no messaging, background pages or pop-ups
  (stage 2); content scripts share the page's world (Servo has no isolated worlds).
- List updates after consent, bookmarks in the interface, downloads — v0.0.2.
