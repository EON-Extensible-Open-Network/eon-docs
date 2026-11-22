<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Converter — plan

**22 November 2026.** This is step three of four; [research.md](research.md) comes first.
The decision is plan madde 48. The goal is **`v0.0.0`**: the application converting files
across every family listed in the research, published and verified. Bringing the same
core into EON Download Manager is the next slice.

## Shape

Two repositories, following madde 45 (one product, its own core and application):

| Repository | What it is | Licence |
|---|---|---|
| `eon-converter-core` | The library. It holds the format registry and detection, the routing graph, the engines (Rust conversions and fetched programs), and jobs with progress and cancel. It has no window and no GUI dependency. | `GPL-3.0-or-later` + EON Module ABI Exception |
| `eon-converter-app` | The window (Tauri, static HTML/CSS/JS, no bundler), plus a command line (`--convert`, `--formats`, `--selftest`). | `GPL-3.0-or-later` + exception |

EON Download Manager will depend on `eon-converter-core` by git `rev`, as the EON Stream
app depends on its core. Its own FFmpeg and yt-dlp fetching then moves to the core's.

## The core's API

```rust
let core = Converter::new(data_dir)?;            // tools live in data_dir/tools
let fmt  = core.detect(path)?;                   // by content first, then extension
let list = core.targets(&fmt);                   // every reachable format, cheapest route each
let plan = core.plan(&fmt, &target)?;            // steps, engines needed, which steps lose
let out  = core.convert(input, &target, options, progress, cancel).await?;
let src  = core.best_source(&offered, &target);  // for EON Download Manager
```

- **Formats** are a static table: id, name, family, extensions, MIME types and magic
  bytes. Detection reads the first bytes, because a file named `.mp4` may be a WebM.
- **Engines** each declare their edges (`from → to`, cost, lossy) and a `run`. An engine
  whose program is missing still declares its edges, so the targets are known before
  anything is fetched. The plan names the tools it needs, and the application fetches
  them with a visible progress bar.
- **Options** are few and per family: quality (high, balanced, small), size for images
  and video, frame rate and width for GIF, page size for PDF output.

## Engines, in order of work

1. **Rust in the core:** data formats, images (`image` crate), spreadsheets (`calamine`
   reads, `rust_xlsxwriter` writes), archives (`zip`, `tar`, `flate2`).
2. **FFmpeg:** video, audio, subtitles, animated images. A container change is tried with
   `-c copy` first, and re-encoding happens only if that fails. GIF uses a palette pass.
3. **Pandoc + Typst:** every document format, and PDF output through Typst.
4. **PDFium:** PDF to text, Markdown (headings from font size, lists, paragraphs) and page
   images.
5. **ImageMagick:** the image formats the first three do not cover.
6. **LibreOffice and Calibre, if installed:** layout-preserving office conversions and
   ebooks.

## The window

Its design is made with the design skills, before the code. It shows:

- a drop area that takes files and folders;
- each file with its detected format, and a target picker with search, grouped by family
  and listing every reachable format;
- the route, with a mark on the steps that lose something;
- a queue with progress, cancel, and "show in folder";
- a notice when an engine must be fetched, with its size.

## Verification for `v0.0.0`

- Unit tests: detection by magic bytes, the routing graph's shortest paths and its "no
  route" answers, each Rust conversion.
- An integration test over a matrix of real files, run on the Windows runner with the
  pinned engines fetched. At least one conversion per edge family is run and the output is
  checked: its detected format, and for media the streams it holds. The owner's examples
  are each a named test: TXT → PDF, PDF → MD, MP4 → GIF, GIF → MP4.
- `--selftest` in the release workflow, as in the other products.
- After the release: download it, check the checksum, and run real conversions with the
  published binary.
