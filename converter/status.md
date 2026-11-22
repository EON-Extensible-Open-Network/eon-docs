<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Converter — status

**As of 22 November 2026.** Plan madde 48; [research](research.md), [plan](plan.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-converter-core` | `v0.0.0` (tag, library) | green on Windows, Linux, macOS | 97 formats, ten families; built-in conversions plus FFmpeg, Pandoc, Typst, PDFium, ImageMagick fetched and SHA-256 pinned |
| `eon-converter-app` | — | — | next: the window, designed before it is coded |

**Tested:** 12 unit tests (detection by content, routing, the core's own conversions, the
read-back of results), and `tests/matrix.rs`: real files of every family converted through
the real, pinned engines on the Windows runner, each result checked by its content and, for
media, by the streams it holds — **90 of 90 pass**. The owner's examples are among them:
TXT → PDF, PDF → MD, MP4 → GIF, GIF → MP4.

## Found while testing

- **An engine can fail and report success.** The pinned ImageMagick reads HEIC but has no
  encoder for it; asked to write one, it writes a PNG under the `.heic` name and exits 0.
  HEIC is therefore read only (the research already said so), and every step's file is now
  read back: content that is not the format asked for, or an empty file, fails the
  conversion. This matters most for LibreOffice and Calibre, which are not pinned — the
  version a person has installed is the one that runs.
- **WMA and WMV share a container.** A file named `.wma` is now taken at its name.
- **`cargo deny`:** chrono is unmaintained (RUSTSEC-2026-0543, no safe version). It is
  recorded in `deny.toml` after a reachability check: pdfium-render uses it only in date
  setters the core never calls, and calamine's `as_datetime` is arithmetic, not parsing.

## Not yet

- OCR: a scanned PDF has no text to convert.
- A sandbox for the engines (madde 4); they run with the person's rights.
- EON Download Manager does not use the core yet; its own FFmpeg fetching moves here when
  it does.
