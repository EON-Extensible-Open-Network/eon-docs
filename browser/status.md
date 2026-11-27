<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — status

**As of 27 November 2026.** Plan madde 50; [research](research.md), [plan](plan.md),
[design](design.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-browser-core` | `v0.0.0` (library) | green on Windows, Linux, macOS, including the real default lists | blocking, link cleaning, HTTPS-only, address bar, search engines, profile, user agent; no engine, no GUI, no network client |
| `eon-browser-app` | `v0.0.0` in progress | builds, lints and tests on Windows, Linux, macOS; self-test step being fixed | Servo 0.8.0; the bar drawn by Servo; tabs, history, session, private window; `--selftest` 26/26 locally |

## Measured

- **Blocking** (core, real lists, release build): compile 107 ms, from cache 18 ms,
  **5.2 µs per request**; Google Analytics, Tag Manager, AdSense, DoubleClick, Meta Pixel,
  Hotjar and Bing's tracker blocked, CDNs, fonts and Wikimedia not.
- **The release build** (Windows 11, lists inside): 126 MB on disk; the window is up
  **1.1 s** after launch; BBC News renders with **11 trackers blocked**; one process,
  **380 MB working set / 575 MB private** with that one page. That is not light yet — on
  par with the big browsers for one heavy page, not below them. Memory is a target for
  v0.0.x, measured the same way each release.
- **Self-test**: 26 checks through the real engine offscreen against pages the browser
  serves itself. Emptying the rules makes six fail, as they should.

## Not yet (each is in the roadmap)

- **No sandbox on Windows, no site isolation** (Servo). Not for banking yet.
- **No video or audio, no WebGL** — v0.0.1.
- **`Sec-GPC` header not sent**: Servo's embedding API cannot add request headers; the
  script-side `navigator.globalPrivacyControl` is set. Raise upstream.
- **`Accept-Language`** is Servo's own; there is no preference for it.
- **Permission prompts**: everything is refused until the prompt exists (v0.0.2).
- **Select boxes, context menus, file pickers and dialogs** are not shown yet (Servo hands
  them to the embedder; v0.0.1).
- List updates after consent, bookmarks in the interface, downloads — v0.0.1.
- **YouTube fails inside Servo's HTTP client** ("client error (SendRequest)" within
  400 ms). It is not the network: `curl` through the browser's own route proxy gets 200
  from `www.youtube.com` at the same moment, and the connection is established before
  the request fails — so it is at the HTTP level in Servo 0.8 (HTTP/2 is the first
  suspect). An earlier guess here (IPv6 without fallback) was wrong: the host also
  resolves to IPv4. To reduce and report upstream (madde 50j). GitHub's sign-in form and
  DuckDuckGo (in Turkish) load.
