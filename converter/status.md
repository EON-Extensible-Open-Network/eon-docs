<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Converter — status

**As of 22 November 2026.** Plan madde 48; [research](research.md), [plan](plan.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-converter-core` | `v0.0.1` (tag, library) | green on Windows, Linux, macOS | 97 formats, ten families; built-in conversions plus FFmpeg, Pandoc, Typst, PDFium, ImageMagick fetched and SHA-256 pinned; fetch progress in bytes |
| `eon-converter-app` | `v0.0.0`, prerelease | green on Windows, Linux, macOS | release downloaded after publishing: checksums match, the Windows binary passes `--selftest`, converts TXT → PDF and MP4 → GIF through the real engines, and opens its window |

**Tested:** 12 unit tests (detection by content, routing, the core's own conversions, the
read-back of results), and `tests/matrix.rs`: real files of every family converted through
the real, pinned engines on the Windows runner, each result checked by its content and, for
media, by the streams it holds — **90 of 90 pass**. The owner's examples are among them:
TXT → PDF, PDF → MD, MP4 → GIF, GIF → MP4. `tests/fetch.rs` serves an engine-sized file
locally and checks the byte progress and the SHA-256 pin (mutation-checked).

**The window** follows [design.md](design.md): `--selftest` (strings, interface, the owner's
routes, real conversions by the core) and `tools/ui-check.mjs`, 29 checks on the real
window — adding files through a second start, choosing by search and keyboard, a lossy
route offering quality, the route naming its engines and what is fetched, Turkish and the
light theme, every control named. The screenshots were reviewed by eye too: the
horizontal route overflowed the details panel and now runs top to bottom there.

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
  it does. That is next.
- Format names (such as "Plain text") are English in both languages; they come from the core.
