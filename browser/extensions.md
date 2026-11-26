<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — other browsers' extensions, on our own core

**27 November 2026.** The owner asked (26 Nov) for a way to use the extensions of the other
browsers while staying on EON's own core. Plan madde 50h, as amended.

## Is it possible?

Yes, in stages. Chrome, Edge, Opera and Firefox extensions share one format,
**WebExtensions**: a `manifest.json`, JavaScript and CSS files, and calls to a `chrome.*`
/ `browser.*` API. Nothing in that format belongs to Chromium or Gecko: an extension is a
zip of web files plus a list of the APIs it wants. A browser that offers those APIs runs
it. Kagi's Orion did exactly this on WebKit; it took years and is still partial, which is
the honest measure of the work.

What EON Browser already has that the API maps onto:

| WebExtensions part | What it becomes in EON Browser |
|---|---|
| `declarativeNetRequest` rules (MV3 blockers: uBlock Origin Lite, AdGuard, Ghostery…) | **Translated into the core's blocking engine** — the same `adblock` engine that runs EasyList. The rule format is close to Adblock Plus syntax (`urlFilter` is ABP syntax already). |
| `content_scripts` (JS and CSS into matching pages) | Servo's user scripts and user stylesheets, behind a match-pattern guard |
| `background` page / service worker | A hidden Servo view on `https://ext.eon.invalid/<id>/`, served from the unpacked extension |
| `action` popup, options page | A Servo view shown over the bar / in a tab |
| `storage.local` / `sync` | The profile (SQLite); `sync` through EON Account's vault later |
| `runtime.sendMessage`, ports | The same bridge pattern as the bar: requests to `ext.eon.invalid`, answered by the browser |
| `webRequest` (blocking, MV2) | The browser's own request hook (`load_web_resource`) — the hook Chrome took away in MV3 |
| `tabs`, `windows`, `contextMenus`, `notifications` | The browser's tab list, menus and notifications |

## The stages

| Stage | Version | What runs |
|---|---|---|
| 1 | `v0.0.2` | Installing from a file or a store link (Chrome's CRX3 and Firefox's XPI are zips; the header is stripped and the signature recorded); the manifest read and **its permissions shown in words before anything installs**; `declarativeNetRequest` static rules into the shield; `content_scripts` CSS and JS with match patterns, `document_start`/`end`/`idle`. Extensions such as uBlock Origin Lite, Dark Reader's static themes, and any user-style extension. |
| 2 | `v0.0.3` | `storage`, `runtime` messaging, background pages and MV3 service workers (as a hidden view), the toolbar button and its popup, options page, `i18n`. Most "content + settings" extensions: Dark Reader, SponsorBlock, Return YouTube Dislike, Bitwarden's autofill (partly). |
| 3 | `v0.0.4` | `tabs`, `windows`, `contextMenus`, `notifications`, `alarms`, `webRequest` (blocking, so full uBlock Origin — MV2 — runs as it does in Firefox), `declarativeNetRequest` dynamic rules. |
| later | — | `devtools`, `nativeMessaging` (EON Download Manager's own extension already speaks it, madde 44), `identity`, `debugger` — each a separate decision. |

Each stage publishes a list of extensions tried and how they behave, the same way
`compat.md` does for sites. "Every extension works" is never claimed.

## Where the code goes

- **`eon-browser-core`** (no engine, no network): the manifest (MV2 and MV3, both
  `chrome` and `browser` namespaces), match patterns, the permission summary in words,
  CRX3/XPI unpacking, **`declarativeNetRequest` → Adblock Plus translation**, extension
  storage. All of it testable without Servo.
- **`eon-browser-app`**: the views, the bridge, the API objects injected into pages and
  background views, the install and permission prompts.

## Security, said plainly

- **An extension is JavaScript inside the engine**, with only the APIs its manifest asks
  for and the person approved — not native code (madde 4's concern). It cannot reach the
  file system or other programs.
- **Servo has no "isolated world"** yet: a content script shares the page's JavaScript
  globals. EON Browser captures what the script needs (`fetch`, the bridge secret) before
  any page script runs, so a page cannot call the extension's bridge in its name. That is
  defence in depth, not isolation; when Servo gets isolated worlds, content scripts move
  into them.
- **A broad host permission ("read and change all your data on all websites") is shown as
  exactly that**, before installing. After the person approves it, the browser cannot stop
  that extension from reading the pages it was allowed.
- Extensions come only from a file the person picked or a store page they opened. EON
  runs no extension store of its own until madde 30's gate.
