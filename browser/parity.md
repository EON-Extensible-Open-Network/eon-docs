<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — parity with Tor Browser, Opera, Firefox, Chrome and Edge

**28 November 2026.** (v0.0.1 was tagged but not published; its contents shipped as v0.0.2.) The owner's bar (26 Nov): *"make sure we have everything Tor,
Opera, Firefox, Chrome and Edge offer."* This table is how that promise is kept: every
capability of those five is a row, and each row says where EON Browser stands. It is
updated in the same pull request as the work (as `zip/parity.md` is for EON Zip).

**Status:** ✓ `version` — shipped · ⏳ `version` — planned there · ✗ — will not be built,
with the reason · ⚠ — possible only partly, with the reason.

Who has it: **T** Tor Browser · **O** Opera · **F** Firefox · **C** Chrome · **E** Edge.

## Privacy and security

| Capability | Who | EON Browser |
|---|---|---|
| Ad and tracker blocking built in | O, E (trackers), F (trackers) | ✓ `v0.0.0` — EasyList, EasyPrivacy, uBlock Origin's lists through `adblock`; no Manifest V3 ceiling |
| Cookie-banner handling | O, F (some regions) | ✓ `v0.0.0` — EasyList Cookie (hides); ⏳ `v0.0.2` auto-reject where a site offers it |
| Tracking parameters stripped | F (Strict/private only), O | ✓ `v0.0.0` — in every window |
| Redirect / bounce tracking skipped | F, (Brave) | ✓ `v0.0.0` — known wrappers; ⏳ `v0.0.2` bounce-tracker list |
| HTTPS-only mode | F, C, E, T | ✓ `v0.0.0` — on by default, per-site exception by choice |
| Third-party cookies blocked / partitioned | F (Total Cookie Protection), T, O | ⏳ `v0.0.2` — measure what Servo partitions, then close the rest |
| Fingerprinting protection | T (uniformity), F (FPP), E (strict) | ⚠ `v0.0.0` user agent and GPC; ⏳ `v0.0.2` normalisation and per-site randomisation (madde 50f); Tor-level uniformity needs Tor Browser's crowd — ✗ |
| Global Privacy Control | F (private), T | ✓ `v0.0.0` script side; the header needs a Servo API (upstream) |
| Private window | all | ✓ `v0.0.0` — memory only, nothing written |
| **Tor network, new identity, new circuit per site** | **T** | ✓ `v0.0.2` — Arti (the Tor Project's Rust Tor) in its own process beside the browser: each site its own circuit, renewed every 10 minutes, "New identity" (`Ctrl+Shift+U`) drops every circuit and clears every cookie. check.torproject.org in EON Browser: "configured to use Tor". ⚠ hides the IP, does not make one as anonymous as Tor Browser (fingerprint) — said on the route page |
| Onion services (`.onion`) | T, (Brave) | ✓ `v0.0.2` — Arti's onion-service client, on the Tor route |
| Tor bridges (obfs4, Snowflake, meek) for censored networks | T | ⏳ `v0.0.3` — Arti's pluggable-transport client with the Tor Project's transport binaries |
| Built-in "VPN" (a proxy) | O, E (Secure Network, Cloudflare) | ✓ `v0.0.2` — the route setting: Tor, or a proxy server the person brings (HTTP `CONNECT`, with credentials); ✓ `v0.0.3` SOCKS5 too, with the name resolved by the proxy (no DNS leak); an EON relay plugs in the same way when one exists (madde 30) |
| Tor's own proxy (Tor Browser's 9150, the tor service's 9050) | T (others through settings) | ✓ `v0.0.3` — one click in the Route panel, beside Tor built in |
| Security level (JavaScript and JIT off) | T (Safer/Safest), E (enhanced security) | ⏳ `v0.0.2` — per-site JavaScript and JIT switches (the core has them) |
| Safe Browsing (phishing and malware warnings) | C, F, E, O | ✓ `v0.0.0` — uBlock Origin's badware list, warning page, local only; ⏳ local phishing lists. Never a remote lookup |
| Site isolation, sandbox | C, F, E, O, T | ✗ for now — Servo has no Windows sandbox and no site isolation (madde 50 cost); follows Servo |
| Password manager | C, F, E, O | ⏳ `v0.0.3` — local, encrypted; sync through EON Account's vault (`v0.0.4`) |
| Password breach check | C, E, F | ⏳ `v0.0.4` — k-anonymity range queries only |
| Passkeys / WebAuthn | C, F, E | ⚠ follows Servo's WebAuthn |
| Clear data on exit, per-site data management | all | ⏳ `v0.0.2` (the core forgets a site; Servo's `SiteDataManager`) |
| Permission prompts and per-site permissions | all | ⏳ `v0.0.2` — refused until then |
| DNS over HTTPS | F, C, E, O | ⏳ `v0.0.2` — or EON DNS system-wide (madde 47) |

## Tabs, windows and navigation

| Capability | Who | EON Browser |
|---|---|---|
| Tabs, reopen closed tab, session restore | all | ✓ `v0.0.0` tabs and session; ⏳ `v0.0.1` reopen closed tab |
| Vertical tabs | E, F, O, C (2026) | ✓ `v0.0.3` — the default; narrow sidebar or tabs across the top as options |
| Tab groups / stacks | C, E, F, O | ⏳ `v0.0.3` |
| Workspaces / Spaces | O, E | ⏳ `v0.0.3` |
| Split view | E, C (2026), O, F | ⏳ `v0.0.3` |
| Sleeping tabs | E, O, F (unloading) | ✓ `v0.0.3` — after 15 minutes by default (5 min – 1 h, or never); the tab keeps its address and title and wakes where it was |
| RAM limiter, network limiter (GX Control) | O (Opera GX) | ✓ `v0.0.3` — a memory limit that sleeps the least recently used tabs; download and upload limits for all connections, in the route process. ⚠ no CPU limiter: the operating system schedules Servo's threads, and slowing them would make pages stall rather than the browser light |
| Resource monitor (GX Control, Task Manager) | O (GX), C, E, F (`about:processes`) | ✓ `v0.0.3` — memory, processor, download and upload speed, live, pausable; per tab: awake or asleep. ⏳ memory per tab follows Servo's per-process reports |
| Pinned tabs | all | ⏳ `v0.0.1` |
| Address-bar search, keywords, history suggestions | all | ✓ `v0.0.0` — `@w`, `@ddg`…; suggestions from local history only |
| Search engine choice | all | ✓ `v0.0.0` — asked, never sold |
| Bookmarks, bookmarks bar, import/export | all | ✓ `v0.0.2` — star in the bar, `Ctrl+D`, `eon:bookmarks`; ⏳ bookmarks bar and folders in the window, import `v0.0.4` |
| History page | all | ✓ `v0.0.2` — `eon:history` (`Ctrl+H`): by day, searchable, forget the last hour / today / everything |
| Import from other browsers | all | ⏳ `v0.0.3` |
| Command palette | O (2025), E | ✓ `v0.0.3` — `Ctrl+K`: every action, panel and open tab, by typing; anything else is searched |
| Side panels | E, O, F | ✓ `v0.0.3` — Shield, Route, Limiters, Monitor docked on the right; the page narrows rather than being covered |
| Speed dial / new-tab shortcuts | O, E, C, F | ⏳ `v0.0.2` — local only, no sponsored tiles |
| Zoom, find in page, print | all | ✓ `v0.0.0` zoom; ⏳ `v0.0.1` find in page; print follows Servo |
| Keyboard shortcuts | all | ✓ `v0.0.0` (`Ctrl+T/W/L/Tab`, `Alt+←/→`, `F5`, `Ctrl+Shift+N`) |
| Mouse gestures | O | ⏳ `v0.0.3` |

## Reading, media and content

| Capability | Who | EON Browser |
|---|---|---|
| Video and audio playback | all | ⏳ `v0.0.1` — Servo's GStreamer back end |
| DRM (Netflix, Disney+…) | C, F, E, O | ✗ — Widevine is licensed by Google; no independent engine has it |
| Picture-in-picture | all | ⏳ `v0.0.3` |
| Reader view, read aloud | F, E, O (reader) | ⏳ `v0.0.2` reader; read aloud through the system's voices `v0.0.3` |
| Translation | C, E, F (on-device), O | ⏳ `v0.0.4` — on-device only (Firefox's Bergamot models), never a cloud service |
| PDF viewer | all | ⏳ `v0.0.2` — pdf.js, local |
| Screenshots of pages | F, E, O | ⏳ `v0.0.2` (Servo's `take_screenshot`) |
| Dark mode for pages | E, O, C (flag) | ⏳ `v0.0.3` |
| Downloads | all | ⏳ `v0.0.1` — to EON Download Manager when installed (madde 44) |

## Extensions

| Capability | Who | EON Browser |
|---|---|---|
| Chrome Web Store extensions | C, E, O | ⏳ phased — the WebExtensions runtime (madde 50h as amended). ✓ `v0.0.2` stage 1: install a CRX3, XPI or folder with the permissions shown first; `declarativeNetRequest` rules run in the shield (the real uBlock Origin Lite: 117,636 rules); content scripts and styles; web-accessible files only. `v0.0.3` storage, messaging, background pages, popups; then `tabs`, `contextMenus`, `webRequest`. ⚠ full compatibility is years of work, as Orion's took |
| Firefox add-ons (AMO) | F, T | same runtime; the same phases |
| Edge Add-ons, Opera add-ons | E, O | same runtime |
| Built-in sidebar apps / messengers | O, E | ⏳ `v0.0.3` — a web panel of any site, no partner apps |

## Accounts, sync and devices

| Capability | Who | EON Browser |
|---|---|---|
| Sync (bookmarks, history, passwords, tabs) | all | ⏳ `v0.0.4` — EON Account's sealed vault: the server cannot read it (madde 46b) |
| Send tab / files to a phone | O (Flow), F, C, E | ⏳ after Android |
| Android | all | ⏳ later — Servo runs on Android |
| iOS | all | ✗ for now — Apple requires WebKit on iOS (outside the EU) |

## What EON Browser will not have, by decision

| Capability | Who | Why not |
|---|---|---|
| AI assistant (Copilot, Gemini, Aria, Firefox's AI) | E, C, O, F | madde 50d: the one switch people ask for is "AI off"; there is nothing to switch off |
| Crypto wallet | O | outside a browser's job; a common source of distrust (Brave, Opera) |
| Shopping, coupons, cashback | E, O | affiliate rewriting by another name (madde 50d) |
| Sponsored tiles, news feed with ads | E, O, F, C | no advertising, by EON's founding principle |
| Telemetry, crash reports sent by default | C, E, O, F | madde 36: absent, not off |
