<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — the window's design

**26 November 2026.** Settles the open question in [plan.md](plan.md) ("how the interface
is drawn", madde 50 [A]) with a working prototype, which became `v0.0.0`.

![EON Browser v0.0.0 showing Wikipedia](https://raw.githubusercontent.com/EON-Extensible-Open-Network/eon-browser-app/main/docs/window.png)

## Decision: the bar is HTML, drawn by Servo itself

Servo's embedding API can draw several views into one window's GL context:
`WindowRenderingContext::offscreen_context` gives each view its own surface, and
`OffscreenRenderingContext::render_to_parent_callback` copies a surface into place. EON
Browser uses two: **the bar** on top and **the page** below. Each frame copies the page
first and the bar over it, so the bar can grow over the page while address suggestions are
open without the page reflowing.

Why not `egui` (servoshell's choice): one engine draws everything, so there is one text
renderer, one font stack and one accessibility tree to look after; the bar is written in
the same static HTML/CSS/JS as every other EON interface (no Node, no bundler), and the
self-test can read and screenshot it the same way it reads a page. The cost: the bar shares
Servo's limits (below).

### The bridge

The bar loads `https://ui.eon.invalid/index.html`. `.invalid` never resolves; the bar's
delegate answers every request itself from files compiled into the program and refuses
everything else. Commands are `fetch('https://ui.eon.invalid/cmd?c=<json>')`; the browser
answers with JSON. State goes the other way with `evaluate_javascript("eonState(...)")`,
at most once per turn of the event loop. **Pages cannot use the bridge**: their delegate
refuses any request to `ui.eon.invalid`, and the self-test checks it.

The browser's own pages (new tab, search-engine choice, harmful-site warning, no-HTTPS,
about) are served on `https://page.eon.invalid/`. Their action links (`/choose`,
`/proceed`, `/allow-http`) are intercepted as navigations and acted on only when the page
asking is itself on `page.eon.invalid` — a site linking there changes nothing (checked).

## The look

The EON family's tokens (ink `#15132b`, deck `#1f1c3d`, pulse `#a08bff`, mint `#74e3ae`,
amber `#ffc861`) and Geist. One idea: **the page is the content, so the bar is quiet** —
one surface, no borders between rows, colour only where it says something: the lock (mint
encrypted, amber plain HTTP by the person's choice), the shield's count (mint, grey when
blocking is off for the site), the note after a cleaned link ("1 tracker removed from the
link"). The active tab is the bar's own surface continuing upward; others are text on ink.

## What Servo 0.8 does not draw, and what was done instead

| Gap | Instead |
|---|---|
| Inline `<svg><use href="#symbol">` | Icons are SVG files served to `<img>` (Servo draws SVG images) |
| CSS `mask-image` (seen on Wikipedia: black squares) | Not used in the bar; reported as a compatibility gap |
| Starting a script evaluation inside another's callback panics (`RefCell` borrowed twice) | Follow-up scripts are queued and run on the next turn |

## v0.0.3: the full window (owner's request, 28 November)

Design system from the UI/UX skill: **flat**, **dense** (density 8/10, 8–32 px spacing),
**quiet motion** (3/10, 150–200 ms, nothing that moves without a cause, none under
reduced motion), the EON family palette (the skill's teal proposal was set aside for
family consistency) and Geist. Every control reachable by keyboard with a visible focus
ring; icon buttons carry labels; live numbers are written out, never colour alone.

```
┌──────────┬───────────────────────────────────────────────┬─────────────┐
│ ＋ New   │ ‹ › ⟳  [🔒 address ………………… ★]   ●Tor  🛡12  ▣ │  Panel      │
│──────────│───────────────────────────────────────────────│  (docked)   │
│ ● Tab    │                                               │  Shield     │
│ ● Tab    │                 the page                      │  Route      │
│ ☾ Tab    │                                               │  Limiters   │
│  …       │                                               │  Monitor    │
│──────────│                                               │             │
│ 🛡 ⇄ ⏱ 📈│                                               │             │
└──────────┴───────────────────────────────────────────────┴─────────────┘
```

- **Vertical tabs** in a sidebar (240 px, collapses to 56 px); a sleeping tab is dimmed
  with a moon and wakes on click. A horizontal strip remains as an option.
- **The bar** keeps one surface; the route pill (Direct / Tor / Tor proxy / Proxy) and
  the shield count stay visible.
- **Docked panels** on the right, one at a time, the page narrows rather than being
  covered: **Shield** (count and switch), **Route** (Direct, Tor through Arti, Tor's own
  proxy at 9050/9150, a SOCKS5 or HTTP server; New identity), **Limiters** (sleep after N
  minutes, RAM limit, network down/up limit — Opera GX's), **Monitor** (RAM, CPU, down/up
  speed as a moving line with the current value written, pausable).
- **Over the page**: address suggestions and the **command bar** (`Ctrl+K`) are drawn by a
  third, small view composited above the page, so the page is never resized for them.

Built as drawn (v0.0.3). Two departures, both from Servo's limits: choices are rows of
buttons, not drop-down lists (Servo 0.8 draws no `<select>` popup); the overlay is
rectangular (it is copied into the window as a block, so rounded corners would show its
background).

## v0.0.5: familiar first (owner's request, 28 November)

*"Yeni kullanıcıların çok kolay alışacağı tarzda olsun."* The v0.0.3 window put its tabs
and tools in a sidebar, which is a choice people must learn. Someone arriving from
Chrome, Edge or Firefox should find every control where their hands already go, and
discover what is new from there. The principle (people expect a new product to work like
the ones they use) is ours; the UI/UX skill's database adds that a first-run guide must
be skippable, and that every control must work from the keyboard.

```
┌───────────────────────────────────────────────────────────────────────────┐
│ [◉ Tab] [◉ Tab] [☾ Tab] [+]                                     Private   │
│ ‹ › ⟳  [🔒 address …………………………………………… ★]          🛡 12   ● Tor   ⋮          │
├───────────────────────────────────────────────────────┬───────────────────┤
│                     the page                          │ (panel, if open)  │
└───────────────────────────────────────────────────────┴───────────────────┘
```

- **Tabs across the top, with the site's icon**, `+` right after the last tab, as in
  every major browser. Vertical tabs stay one click away (menu → *Vertical tabs*).
- **The menu, `⋮`, at the right end of the bar** — where Chrome, Edge and Firefox keep
  theirs (`Alt+F` too). Everything is in it with its shortcut: new tab and private
  window; history, bookmarks, extensions; shield, connection (Tor and proxy), limiters,
  monitor; zoom; vertical tabs; the command bar; about. Arrow keys, `Enter` and `Esc`
  work; a click outside closes it. It replaces the sidebar's tool list and the bar's `⌘`
  button, which Windows users do not recognise.
- **Plain names**: *Connection* rather than *Route* for Tor and proxies.
- **A first-run welcome on the new tab page**: three short cards (your tabs and menu
  are where you expect them; what the shield counts; Tor is one choice away) and **Got
  it**, which hides it for good. It never blocks the page and can be ignored.
- What stays: the shield count and connection pill in the bar, docked panels that
  narrow the page, `Ctrl+K`.

Built as drawn. The site icon comes from Servo's favicon, turned into a 32 px PNG and
served to the interface on its own host, so the interface still loads nothing from a site.

## Next

The shield panel listing what was blocked and why; settings; the list-update consent;
downloads; a light theme following the system; the tab strip's drag to reorder.
