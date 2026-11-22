<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Zip — window design

**22 November 2026.** Written before the code, as [the plan](plan.md) asks, with the design
skills. The owner asked for a "new-generation, clean" look; the complaint about both 7-Zip
and WinRAR is that they look twenty years old.

## The family, and the one idea

EON Zip uses the EON Download Manager family the owner accepted: Ink/Deck surfaces, Mist
and Haze text, the Current/Pulse/Rose/Amber/Mint accents, Geist, the frameless title bar,
the list-and-panel layout, dark and light. Each product has one idea that carries it; for
EON Zip it is **the landing**:

> Before anything is written, the window shows exactly where it lands — "Downloads ›
> Proje Şirketi", 214 files, 38 MB — and anything worth knowing: two programs inside, one
> name that tries to climb out (refused). Then one button.

Both 7-Zip and WinRAR make a person choose a folder and hope. The landing is the answer to
"will this make a mess" and "is this safe", in one look.

The second, quieter idea is **the fold**: each row's packed size drawn inside its full size
as a thin two-tone bar, so how well something compressed is seen, not calculated.

## Screens

```
┌ ◆ EON Zip   [Open]  [New archive]                              ⚙  ─ □ × ┐
├──────────────────────────────────────────────┬─────────────────────────────┤
│ proje.7z › Belgeler › Eski       [⌕ search ] │  rapor.txt                  │
│ ─────────────────────────────────────────────│  Text · 126 KB → 18 KB      │
│ ▢ 📁 Eski                    —        2 items│  ┌───────────────────────┐  │
│ ▢ 📄 rapor.txt   126 KB ▓▓░░░░░░  86% smaller│  │ satır 0: ğüşıöç ...   │  │
│ ▢ 🖼 foto.jpg    196 KB ▓▓▓▓▓▓▓▓  stored     │  │ (preview)             │  │
│                                              │  └───────────────────────┘  │
│ 3 selected            [Extract selected]     │  [Extract]  [Test] [⋯]      │
└──────────────────────────────────────────────┴─────────────────────────────┘
```

- **Home (nothing open):** two large choices — *Open an archive*, *Make an archive* — and
  the whole window is a drop target: an archive dropped is opened, files dropped start a
  new archive. Buttons always work too (dragging is never the only way, WCAG 2.2).
- **Archive view:** a breadcrumb through the archive's folders, search across every name
  in it, a table (name, size, the fold, modified) with multi-select (click, Shift, Ctrl,
  Ctrl+A) and the keyboard (arrows, Enter opens a folder, Backspace goes up).
- **The panel** shows the archive when nothing is selected — format, files, size, ratio,
  encryption, the code page names were read in ("Names read as CP857 Turkish"), the
  comment — and the selected file's **preview** (text and pictures) when one is.
- **The landing sheet** (Extract): destination (the archive's folder by default, *Change*),
  the landing line, warnings in words, refused names with why, what to do with files that
  are already there, and — for a bomb — the size and a second, explicit button.
- **The new-archive sheet:** the files, the name and folder, the format (7z · ZIP · TAR.XZ ·
  more), six levels from *Store* to *Ultra*, password with *Hide file names* (7z), split
  size, comment (ZIP). A ZIP password with letters outside English gets a plain warning:
  7-Zip cannot open it; choose 7z or English letters.
- **Progress** stays in the sheet that started it, with *Stop*; the result says what
  happened in one line and offers *Show in folder*.
- **Hash:** algorithms to tick, results with *Copy*.
- **Settings:** theme, language, the right-click menu on Windows (add / remove, per user,
  no administrator rights).

## Words

Plain and short. "Extract", "Make an archive", "Test", not "Add to archive…" or
"Unpack". Errors say what happened and what to do: "The password does not open it."
"This archive holds more than it says; it was stopped." Turkish and English for every
string.

## Rules carried over

WCAG AA in both themes; a visible focus ring; motion under 220 ms and none when reduced;
static HTML/CSS/JS, no bundler, no network, no markup from strings; the interface never
holds a path, only names and numbers.
