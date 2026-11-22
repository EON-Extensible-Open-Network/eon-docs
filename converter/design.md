<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Converter — window design

**22 November 2026.** Written before the code, as [the plan](plan.md) asks. Made with the
design skills; where their generic answer did not fit, the reason is written down.

## The decision the design rests on

EON Converter is a sibling of EON Download Manager, whose look the owner accepted (the
Office apps' look was rejected). So it uses **the same family**: the Ink/Deck surfaces,
Mist and Haze text, the Current/Pulse/Rose/Amber/Mint accents, Geist, the title bar, the
list-and-panel layout, the same light theme. The design skill's own recommendation
(slate and green, a monospace pairing, a landing-page structure) was for a generic
developer tool and is not taken: a second visual language inside one umbrella would cost
more than any gain from it (madde 32).

Each product gets **one idea that carries its design** and everything else stays quiet.
For Download Manager it is the lanes. For the converter it is **the route**.

## The route

A conversion is a chain: `TXT → (Pandoc) → Typst → (Typst) → PDF`. The window draws it
as one strand: a chip per format, joined by a line labelled with the engine that does
that step.

- A step that loses something (re-encoding, a layout rebuilt, 256 colours) is drawn
  **notched in Amber with a mark**, and a plain sentence under the strand says what is
  lost. A colour is never the only sign: the mark and the sentence carry it too.
- An engine that is not here yet shows its size on the strand ("Typst · 22 MB, fetched
  once"). The person sees the cost before choosing.
- While converting, the strand fills step by step, which is the progress bar. A second
  bar would only repeat it.

## Layout

The DM layout, simplified: the converter has no sidebar.

```
┌ title bar: icon · EON Converter · [Add files] [Add folder]         settings ─ □ × ┐
├──────────────────────────────────────────────┬───────────────────────────────────┤
│ Files                [Convert all ▸] [Clear] │  notes.txt                     ×  │
│ ┌──────────────────────────────────────────┐ │  Text · told by its content       │
│ │ ▣ notes.txt   Text → [PDF ▾]   ●──●──●   │ │                                   │
│ │ ▣ clip.mp4    MP4  → [GIF ▾]   ●─◆─●     │ │  TXT ──Pandoc── Typst ──Typst── PDF│
│ │ ▣ report.pdf  PDF  → [MD ▾]    done  ⤴   │ │  (the route, large)               │
│ └──────────────────────────────────────────┘ │                                   │
│                                              │  Quality   High · Balanced · Small│
│   (drop files or folders anywhere)           │  Save to   Beside the original ▾  │
│                                              │  [Convert]  [Show in folder]      │
└──────────────────────────────────────────────┴───────────────────────────────────┘
```

- **Empty window:** the whole list area is the drop target, with *Choose files* and
  *Choose a folder* buttons in it. Dragging is never the only way in (WCAG 2.2, dragging
  movements).
- **A row:** file icon in its family's colour, name, the detected format ("MP4 video"),
  the target button, a small route, and the state on the right (ready, fetching,
  converting with its step, done, failed).
- **The panel:** the chosen file's route drawn large, the options for its family only
  (quality for every lossy target; width and frame rate for GIF; nothing for lossless
  data), where the result goes, and the actions.
- **Convert all:** chooses one target for every file of the same family at once.

## The target picker

A popover opened from the target button, a combobox with a listbox:

- a search field that matches names and extensions ("jpg" finds JPEG);
- the formats grouped by family, the input's own family first;
- per row: name, extension, a lossy mark, and "needs FFmpeg · 190 MB" when it does;
- arrows move, Enter chooses, Escape closes and returns focus to the button.

Every format the core can reach is listed. None is hidden behind "more".

## States and words

| State | What the row shows |
|---|---|
| ready | the target and its route |
| fetching an engine | "Getting FFmpeg, 84 of 190 MB" |
| converting | the strand filling; "Step 2 of 3 · Typst" |
| done | "Done" with *Show in folder* and *Open* |
| failed | the reason in plain words (the core's error code turned into a sentence), *Try again* |
| cancelled | "Stopped", the partial file already removed by the core |

Removing a finished row never touches the file on disk, so it needs no confirmation.
Cancelling a running conversion is one click and is undone by *Try again*.

## Family colours

Ten families, five accents, and an icon each, so no family depends on colour alone:
video Current, audio Rose, image Amber, document and ebook Pulse, spreadsheet and data
Mint, presentation Amber, subtitle Haze, archive Haze.

## Rules carried over from the family

- Every text pair meets WCAG AA in both themes; focus is always visible.
- Motion answers actions only, under 220 ms, and stops with `prefers-reduced-motion`.
- The interface is static HTML/CSS/JS with no bundler, no network of its own, and no
  markup built from strings. It is handed names, formats and indices and asks for the
  rest by number. CI checks all three, as in Download Manager.
- English and Turkish for every string; `--selftest` fails on a missing one.

## The command line

The same executable: `--formats <file>` lists every target with its route,
`--convert <file> <format> [folder]` converts, `--selftest <report>` checks what a
machine can check (strings, the format table, a real conversion done by the core).
