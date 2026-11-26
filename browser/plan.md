<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — plan and roadmap

**26 November 2026.** Step three, after [research.md](research.md). The decision is plan
madde 50; progress is kept in [status.md](status.md).

## The owner's three conditions

1. **Not Chromium, not Windows' engine.** "Our own engine, or something fully open source
   and loved by everyone." WebView2, CEF and every Chromium fork are out.
2. **Every desktop platform.** Windows, Linux and macOS, each a first-class build from the
   first release, x86_64 and ARM64, like EON Stream since `v0.33`.
3. **Light, private, the best.** No telemetry, privacy on by default, small and fast.

## The engine: Servo

Writing a web engine from nothing is not a one-person job: Ladybird, with a funded
full-time team, has been at it since 2022 and has not shipped an alpha. Gecko cannot be
embedded on the desktop: every Firefox-based browser is Firefox's own front end with
patches, built on a farm of 16-core machines in about 2.5 hours a release.

**Servo** is the engine that fits all three conditions. It is written in Rust (the
language of every EON core), licensed MPL-2.0, independent of Google and Apple, governed by
Linux Foundation Europe, released every month and published on crates.io as an embeddable
library since April 2026. It is the engine Firefox's own Stylo and WebRender came from. EON
Browser is the browser *around* it: the window, the privacy layer, the storage, and the
fixes we send upstream. That is what "our own engine" can honestly mean for a team this
size: an engine we build and ship ourselves, whose code we read, patch and contribute to,
rather than one handed to us by an operating system or an advertising company.

Measured on the owner's machine on 26 November 2026 (see research.md): `servo 0.8.0`
builds on Windows without clang in under six minutes, renders Wikipedia, Google search,
GitHub and Reddit correctly offscreen, and its request hook blocks trackers.

**What it costs, said here and again in madde 50:**

- **Sites will break.** Servo is younger than the three big engines. Some pages render
  wrongly or not at all; CSS mask icons, some heavy web apps and media are known gaps.
  The browser says so (a "this page may not work here" notice is honest, a fake fallback
  to Chromium is not).
- **No sandbox on Windows, no site isolation yet.** A content process compromised by a
  hostile page is not contained the way it is in Chrome or Firefox. Until Servo has it,
  every release says so in its notes, and the JavaScript JIT can be turned off per site
  (Servo builds without it) to remove the largest class of engine exploits.
- **No DRM.** Netflix, Disney+ and other Widevine sites will not play. Widevine is
  licensed by Google; no independent engine has it.
- **No WebExtensions.** uBlock Origin cannot be installed. Its job is built in instead
  (Brave's `adblock` engine reads the same lists and the same syntax).
- **Monthly breaking API.** The app pins Servo exactly and moves forward deliberately, or
  stays on a Servo LTS line for security fixes only.
- **Not small on disk.** Servo is about 100 MB unpacked (a system webview would have been
  15 MB). "Light" is therefore measured where it matters to a person — memory, start time,
  background activity, battery — and published with each release.

## Shape

Two repositories, following madde 45 and EON Stream's layout:

| Repository | What it is | Licence |
|---|---|---|
| `eon-browser-core` | The library: the privacy and browsing policy. Blocking, link cleaning, HTTPS upgrade, address-bar parsing, search engines, site settings, history, bookmarks, sessions, fingerprinting policy. **No engine, no window, no network access of its own.** | `GPL-3.0-or-later` + EON Module ABI Exception |
| `eon-browser-app` | The browser: Servo, the window, tabs, the interface, a command line (`--selftest`, `--shot`). | `GPL-3.0-or-later` + exception |

The core knows nothing about Servo. That is deliberate: it is tested in seconds instead of
minutes, it survives an engine change, and the policy that protects a person can be read
on its own. There is no `eon-browser-spec` yet: the browser publishes no contract of its
own. Blocking lists use the existing Adblock Plus / uBlock Origin syntax, and sync, when
it comes, uses EON Account's vault (madde 46), whose contract lives in `eon-account-spec`.

### The core never touches the network

Filter lists must be updated, and that is the only thing the core needs from the internet.
It does not fetch them: `Lists::update` is handed a function that fetches a URL, and the
app supplies it. A test asserts the core has no HTTP client in its dependency tree. The
person sees every list, where it comes from, its licence and when it was last fetched;
there is no EON server in the path.

**The lists ship with the release.** EasyList, EasyPrivacy and uBlock Origin's lists are
GPL-3.0 (EasyList dual-licensed with CC BY-SA 3.0), compatible with the app's licence, so
a snapshot taken at build time goes into the release (`Lists::seed`). Blocking works from
the first second, and the first start makes **no request at all**. Fetching fresh copies
from their publishers is asked once ("keep the blocking lists up to date from easylist.to
and github.io?") and can be turned off; a release always carries a fresh snapshot.

## The core's API

```rust
let profile  = Profile::open(dir)?;                     // SQLite: history, bookmarks, sessions, sites
let lists    = Lists::load(dir)?;                       // known lists, what is on, what was fetched
lists.update(|url| fetch(url), now)?;                   // the app fetches; the core checks and stores
let shield   = Shield::build(&lists)?;                  // adblock engine, cached compiled to disk

match shield.check(&Request { url, source, kind }) {    // every request the engine makes
    Verdict::Allow => {}
    Verdict::Block { rule } => cancel(),
    Verdict::Redirect { resource } => serve(resource), // uBO's neutered stand-ins
}
let page     = shield.cosmetic(&url);                   // selectors to hide, scriptlets to inject

let nav      = navigate(&url, &policy);                 // strip tracking parameters, unwrap
                                                        // redirect wrappers, upgrade to HTTPS
let target   = omnibox::classify("rust servo", &engines); // Url | Search | Internal
let ua       = fingerprint::user_agent(Platform::current());
let sites    = profile.site(&url);                      // shields, JS, JIT, permissions per site
profile.visit(&url, title, now)?;  profile.suggest("wik")?;  // history with full-text search
```

## Privacy defaults

| Protection | Default | Note |
|---|---|---|
| Telemetry, crash reports, usage counters | **absent** | madde 36; not a setting |
| Requests before the person opens a page | **none** | first start, idle, typing in the address bar: tested in CI on a clean profile |
| Ads and trackers (EasyList, EasyPrivacy, uBO lists) | on | per-site off switch; counts kept on the device only; Peter Lowe's list offered, off |
| Cookie banners (EasyList Cookie) | on | |
| Tracking parameters (`utm_*`, `fbclid`, `gclid`, `msclkid`, …) | stripped, in normal windows too | our own list, every entry documented (Firefox does this only in Strict) |
| Redirect wrappers (`google.com/url`, `l.facebook.com`, …) | unwrapped | the destination is in the address; skip the hop |
| HTTPS | **only**, with a page to continue on HTTP for one site | localhost, private IPs and `.onion` are exempt |
| Third-party cookies and storage | blocked / partitioned as far as Servo allows | [A]: Servo's partitioning state is measured in the selftest |
| Referrer to another site | origin only | |
| Global Privacy Control | sent | California's AB 566 requires a signal from 2027 |
| Do Not Track | **not** sent | it is ignored and adds a fingerprint bit |
| User agent | the same string as Firefox ESR on the same system | the largest crowd that freezes its UA; Servo's own token is removed |
| Search suggestions | off | typing is sent to the engine only when the person turns it on |
| Search engine | asked on first start | no default sold to anyone; DuckDuckGo, Brave Search, Startpage, Mojeek, Ecosia, Qwant, Google, Bing listed |
| AI features | none | "one switch to turn AI off" is the most repeated request in 2025–2026; there is nothing to switch off |
| Private window | memory only | nothing written to disk, closed with the window |

## The interface (in the app, after the core)

**How it is drawn — decided by a prototype ([design.md](design.md)):** HTML/CSS/JS rendered
by Servo itself in its own view above the page, talking to the browser through
`https://ui.eon.invalid/cmd`, a host that never resolves and that pages cannot reach. One
engine, the same no-Node, no-bundler interface as EON Stream. What people love and ask
for, in order of how often it came up in the research:

1. **Vertical tabs** with a compact horizontal option; tab groups; pinned tabs.
2. **Spaces** (Arc, Zen, Vivaldi): separate sets of tabs, each with its own container of
   cookies if the person wants.
3. **Split view** of two pages.
4. **A command bar** (Ctrl/Cmd+K, Arc/Floorp): every action and every open tab by name.
5. **Peek / Glance**: open a link in a floating view and dismiss it.
6. **Sleeping tabs** and a memory figure per tab, so "light" is visible.
7. **Reader view** and picture-in-picture.
8. **A shield per site** showing what was blocked, and why, with one switch.

## Roadmap

Each line is a release; each release ships (CLAUDE.md, "every alpha build ships").

| Version | Repository | What it adds |
|---|---|---|
| `v0.0.0` | core | Blocking (network, cosmetic, scriptlets) over `adblock`, the list catalogue and its update/verify cycle, link cleaning, HTTPS upgrade, address-bar parsing and search engines, site settings, history with full-text search, bookmarks, sessions, user-agent policy. Unit tests and a test that the core has no network client. |
| `v0.0.0` | app | Servo window on Windows, Linux and macOS; tabs; address bar with history suggestions; back, forward, reload; the core wired in (every request through `Shield`, every navigation through `navigate`, element hiding in two steps); the browser's own pages, including the first search-engine choice and the harmful-site and no-HTTPS pages; the session restored; a private window; `--selftest` (26 checks offscreen, no network); the day's lists inside each release; SHA-256 sums. |
| `v0.0.1` | app | **Video and audio** (Servo's GStreamer back end, packaged per platform) and **WebGL** (ANGLE's translator needs libclang at build time). The designed interface: vertical tabs, command bar, the shield panel, settings, light theme. List updates from their publishers after the one-time consent. Bookmarks in the interface (the core has them). Downloads (handed to EON Download Manager when installed, madde 44; otherwise saved by the app). ARM64 builds. |
| `v0.0.2` | core + app | Fingerprinting protections (madde 50e), per-site JIT switch, third-party storage measured and closed as far as Servo allows, permissions prompts. |
| `v0.0.3` | app | Spaces, split view, peek, sleeping tabs, reader view, import from Firefox and Chrome. |
| `v0.0.4` | core + app | Sync through EON Account's sealed vault (madde 46b): the server stores what it cannot read. |
| later | — | Android (Servo runs there); a Servo LTS line for the stable channel; contributions upstream for what breaks most. |

The **site compatibility list** (`browser/compat.md`, from `v0.0.0`) records the sites
people use most and how each renders, with the Servo version; a site that breaks is a bug
report to Servo or a fix from us, and the list shows which.

## Testing

- **Core:** `cargo test` — every rule has a test, blocking against real list syntax with a
  fixture list, cleaning against a table of real tracking links, the omnibox against a
  table of inputs. No network.
- **App:** `--selftest` renders local pages through the real engine, offscreen (Servo's
  `SoftwareRenderingContext`, so CI needs no display), and checks by pixel and by script
  that a blocked image is gone, a stripped parameter is gone, an `http://` link opened as
  `https://`. A live-site matrix (`tools/sites.mjs`) loads the compatibility list and
  stores screenshots; it is not a CI gate, because the web changes under it.
- **Light:** each release publishes start time, memory with one and with ten tabs, and
  idle CPU, measured the same way every time.
