<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — research and analysis

**26 November 2026.** Steps one and two: research, then analysis. It is followed by
[plan.md](plan.md) (shape, defaults, roadmap) and [status.md](status.md). The decision is
plan madde 50. Every claim below was dated and sourced in the research notes; the sources
most worth reading are linked inline.

## What was asked

The owner asked for solid research into which browsers people love and why, what each does
well and badly, and what the perfect privacy-respecting browser would be; then a roadmap
for the core of EON's own browser, then the browser itself: light, and the best. During the
work the owner added two conditions: **no Chromium and no Windows engine** ("our own
engine, or something fully open source and loved by everyone — people hate Windows and
Chromium"), and **every platform**, not only Windows.

## The market, and how browsers lose people

StatCounter, desktop, October 2026: Chrome 66.98 %, Edge 13.24 %, Safari 7.83 %, Firefox
5.72 %, Opera 2.65 %, Brave 1.01 %. People choose a browser for compatibility, extensions
and workflow features. They **leave** one, almost every time, because something was
imposed on them:

- **Chrome** switched off Manifest V2 in July 2025, which removed the full uBlock Origin;
  Google then kept third-party cookies on by default and retired Privacy Sandbox (Oct
  2025).
- **Edge** made a Copilot page the default new tab (March 2026), after years of importing
  Chrome data unasked.
- **Firefox** lost trust through text, not code: the Terms of Use of February 2025 were
  rewritten within three days, and the "AI browser" turn of December 2025 led to an AI
  kill switch in Firefox 148. About 1 % of users flipped it; they are exactly a privacy
  browser's audience.
- **Brave** is the most private by default in independent measurement (Leith), but
  autocompleted affiliate links (2020) and installed VPN services without consent (2023).
- **DuckDuckGo**'s Bing contract stopped it blocking Microsoft's trackers (2022).
- **Vivaldi** wins power users with tab stacks, tiling and a no-AI stance; about 5 % of its
  interface is closed source.
- **Arc** gave the industry Spaces, Peek and the sidebar; it shuts down on 22 December 2026
  and cannot be open-sourced. **Orion** dropped Windows and Linux in October 2026 because
  porting WebKit cost too much. **Zen** carries Arc's ideas on Firefox (~500k users) with
  recurring memory and crash issues.
- Engines differ by only about 10 % in speed (Speedometer 3.1). "Light" is won on memory,
  background activity and battery, and it must be measured, because most published
  numbers are marketing.

The interface features people now expect everywhere: **vertical tabs** (Chrome only got
them in April 2026; even Tor Browser 15 has them), tab groups, **split view**, **Spaces**,
**Peek/Glance**, a command bar, sleeping tabs, reader view, picture-in-picture. They live
in the interface layer, independent of the engine.

## The privacy bar in 2026

- **Partition, then block.** Third-party state keyed by the top-level site (Firefox Total
  Cookie Protection in every mode, Brave's ephemeral storage, Safari ITP), with list-based
  blocking on top.
- **Built-in blocking without MV3 limits.** Brave's `adblock` crate (MPL-2.0) reads
  EasyList, EasyPrivacy and uBlock Origin syntax, network and cosmetic, with scriptlets.
  Inside the browser, no 30,000-rule ceiling applies.
- **Clean links in normal windows.** Firefox strips query parameters only in Strict or
  private mode. EON's default should be Firefox's Strict, not its Standard.
- **Fingerprinting: normalise and randomise, do not chase uniformity.** Uniformity (Tor,
  Mullvad) needs a huge crowd that never changes a setting; a new browser has no crowd.
  Mozilla's November 2026 study over ~25 million users shows the practical middle:
  hardware concurrency reported as 4 or 8, the taskbar hidden from screen APIs, the rest
  randomised; **language and time zone deliberately left alone**. Brave randomises canvas,
  WebGL and audio per site and session ("farbling").
- **Network:** HTTPS-only; encrypted DNS and ECH together (ECH is RFC 9849 since early
  2026, and its keys come from DNS); **Global Privacy Control** sent, since California's
  AB 566 requires an opt-out signal from 1 January 2027; Do Not Track not sent.
- **No phoning home.** Zero requests until the person opens a page: no first-start pings
  (Helium), no suggestions while typing, no remote Safe Browsing lookups.
- **Patch speed is privacy too.** In 2026 a V8 fix was public on 7 August, exploited on
  28 August and shipped by Chrome on 3 September.

## The engine

| Option | The owner's condition | One-person maintenance | Privacy control | Compatibility |
|---|---|---|---|---|
| System webview (WebView2 on Windows) | rejected | lightest | Microsoft collects "required" diagnostic data that the app cannot turn off (MS Learn, Oct 2026) — contradicts madde 36 | Chrome level |
| CEF / Chromium fork | rejected | heavy: every Chromium security release, 100–200 MB | full | Chrome level |
| Gecko (Firefox patch set) | open, but "Firefox with patches" | heavy: ~2.5 h builds on 16-core runners per release, 40 GB disk; interface in Firefox's JS/CSS, not Rust; MPL/GPL split per file | full | Firefox level |
| Ladybird | fits | no embedding API, no alpha, no Windows yet | — | — |
| An engine from scratch | fits | impossible: Ladybird's funded team is still before alpha | — | — |
| **Servo** | **fits** | medium: monthly breaking API, softened by LTS lines | **full; no telemetry** | **partial, to be measured** |

Two consequences beyond the table. Tauri and wry use WebView2 on Windows, so **EON Browser
cannot be a Tauri application** like the rest of the family. And Gecko is the honest fallback:
Zen and LibreWolf prove one person can keep a Firefox fork current within a day or two of
upstream.

**Servo** is in Rust, MPL-2.0 (it combines with GPL-3.0 code as a Larger Work; no file is
marked "Incompatible With Secondary Licenses"), governed by Linux Foundation Europe and
built mostly by Igalia. It has been on crates.io as an embeddable library since April
2026, releases monthly, and has LTS lines (six-monthly, nine months of security fixes).
Firefox's Stylo and WebRender came from it.

### Measured on the owner's machine, 26 November 2026

`servo = "0.8.0"` from crates.io, Rust 1.98.1, MSVC, **no clang installed**: a clean debug
build linked in 5 min 48 s (SpiderMonkey comes prebuilt). An offscreen probe
(`SoftwareRenderingContext`, 1280×800) with a `load_web_resource` hook blocking three
tracker hosts:

| Page | Loaded | Requests | Blocked | Rendering |
|---|---|---|---|---|
| example.org | 1.6 s | 4 | 0 | correct |
| Wikipedia, "Rust" article | 2.0 s | 85 | 0 | layout, fonts and infobox correct; CSS-mask SVG icons drawn as black squares |
| DuckDuckGo search | 1.1 s | 143 | 0 | the page kept animating; the screenshot timed out |
| github.com/servo/servo | 2.8 s | 187 | 0 | header, tabs, sidebar correct; the file list still a skeleton after 3 s |
| Google search | 3.9 s | 174 | 2 | correct, with the embedded map and images |
| reddit.com/r/rust | 1.4 s | 192 | 0 | essentially pixel-perfect |
| youtube.com | failed | 1 | 0 | a network reset (curl failed the same way at the time); inconclusive, retest |

That is clearly better than reading Servo's blog suggests. **Not yet measured:** signing
in, forms, video (MSE), long-lived web apps (Gmail, Discord), banking. **No DRM** should be
assumed: Netflix and Spotify on the web will not play.

The embedding API has what a privacy browser needs: `load_web_resource` sees every HTTP
request and can cancel it or answer it; `request_navigation` can veto or rewrite a
navigation; `UserContentManager` injects user scripts and stylesheets (cosmetic filters,
fingerprinting script); `SiteDataManager` manages cookies and storage per site; permission,
popup, authentication, notification and form-control requests all come to the embedder;
`ServoBuilder` preferences set the user agent and proxy; the `js_jit` feature can be left
out for a hardened build. **There is no download API yet** (an NLnet grant for navigation
and downloads was approved in 2026).

## The cost of Servo, honestly

| Risk | Weight | Mitigation |
|---|---|---|
| **No sandbox on Windows, no site isolation**; Servo itself says it comes "AS IS … no guarantees, including security guarantees"; SpiderMonkey fixes arrive about one monthly release late | high | a per-site JIT switch and a no-JIT build; sandboxed multiprocess on Linux and macOS; every release note says it plainly |
| Heavy web apps, video and DRM | high | a compatibility list measured with each Servo version; "open this page in another browser" as an honest escape; fixes sent upstream |
| Monthly breaking API; an unknown preference name panics | medium | pin Servo exactly, follow an LTS line for the stable channel, check preference names in CI |
| Upstream's people and money (~$8k/month recurring donations; Verso, the nearest precedent, was archived in Oct 2025 for "limited manpower and funding") | medium | contribute upstream; keep the core engine-independent so the policy survives an engine change |
| Servo does not accept AI-written contributions | — | upstream fixes from EON are written by hand, under that policy |
| ~100–115 MB on disk | low | "light" is defined and published as memory, start time, idle CPU and zero background traffic |

## What EON Browser must do — the analysis

The engine is not what will set EON Browser apart; the **contract** is. Mainstream browsers
lost trust in 2025–2026 on exactly these points, and none of them waits for the engine to
mature:

1. **Nothing imposed.** No AI, no sponsored tiles, no affiliate rewriting, no search
   default sold to anyone, no component installed unasked. Every protection on by default
   and every one can be turned off.
2. **Zero requests until the person opens a page**, proven in CI on a clean profile: start,
   idle, type in the address bar — no outgoing connection. Filter lists ship with the
   release (EasyList, EasyPrivacy and uBlock Origin's lists are GPL-3.0, compatible with
   ours), so blocking works from the first second; updating them from their publishers is
   asked once.
3. **Blocking built in**, network and cosmetic, with a per-site switch and the reason for
   every block.
4. **Strict by default:** tracking parameters stripped and redirect wrappers skipped in
   normal windows, HTTPS-only, third-party state partitioned or blocked, GPC sent.
5. **Fingerprinting normalised, not uniform**; the user agent of the largest crowd that
   freezes it (Firefox ESR on the same system), with Servo's own token removed.
6. **The interface people already love:** vertical tabs, Spaces, split view, peek, a
   command bar, sleeping tabs, reader view.
7. **Separate products stay separate** (madde 45): downloads go to EON Download Manager,
   the tunnel to EON DNS, sync to EON Account's sealed vault.

**Left open for the plan:** WebExtensions. Servo has none, and madde 4 forbids
code-executing plugins until a sandbox design exists. Built-in blocking does the job people
install extensions for most; v1 ships without extensions, and that is written as a decision.

**Not included, with the reason:** the Turkish ad list `turk-adlist` (hosts format,
CC BY-SA 3.0) has not been updated since December 2023.
