<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Converter — research and analysis

**22 November 2026.** This covers steps one and two of four: research, then analysis. It is
followed by [plan.md](plan.md), and the decision is plan madde 48.

## What was asked

The owner asked for a converter that changes any file into any format it can become.
Their examples were text to PDF, PDF to Markdown with no mistakes, video to GIF and GIF to
video, and "all of them". Its core is a library. EON Download Manager will later use the
same core: every format a download can become is offered before it starts. If the source
cannot be had in that format directly, the person is told once. The download then arrives
in the format that converts best, and the core turns it into the one they asked for.

## What "any format" can honestly mean

No single program converts everything. Every serious converter, desktop or web, is a
router: it detects what a file is, finds a chain of conversions to the target, and runs
proven engines for each step. EON Converter is built the same way. It writes only the
routing, the small conversions that need no engine, and the glue. "Any format" therefore
means the union of what the engines below can read and write. That union is a few hundred
formats, listed by family below, with three honest limits:

1. **Some conversions lose things by nature.** PDF is a page description, not a document
   structure. PDF to Markdown recovers text, headings, lists and paragraphs. Tables, columns
   and footnotes come back approximately, and a scanned PDF has no text at all without OCR.
   A video turned into a GIF keeps 256 colours. The application says which steps lose
   something before it runs them, instead of promising "without errors".
2. **Protected content is refused.** DRM-protected media and ebooks are refused, as in
   EON Download Manager (madde 44b).
3. **Two engines are too large to fetch on demand.** LibreOffice (about 350 MB, needs an
   administrator install) and Calibre are used only when already installed. Without them,
   spreadsheets and presentations still convert to and from their data, and documents
   still reach PDF through Typst. A page layout made in Word may then not survive exactly.

## The engines

Each engine is fetched the first time a conversion needs it, never bundled. Each is pinned
by version and SHA-256 in the code (madde 37), from its own project's release page, as EON
Download Manager already does for its helpers.

| Engine | Version | Licence | Used as | What it covers |
|---|---|---|---|---|
| **FFmpeg** (BtbN static builds) | 8.1 | GPL-3.0 build | separate program | Video, audio, subtitles, animated images, many still images |
| **Pandoc** | 3.14 | GPL-2.0-or-later | separate program | Documents and markup: Markdown flavours, HTML, DOCX, ODT, RTF, EPUB, LaTeX, reST, Org, AsciiDoc, Textile, MediaWiki, DokuWiki, Jira, FB2, DocBook, Jupyter, Typst, man, PPTX (write) |
| **Typst** | 0.15.1 | Apache-2.0 | separate program | PDF (and PNG/SVG pages) from any document Pandoc reads, with no LaTeX install |
| **PDFium** (bblanchon builds) | chromium/8165 | BSD-3-Clause | loaded library | Reading PDF: text with font sizes and positions, page images |
| **ImageMagick** (portable) | 7.1.2-36 | ImageMagick (Apache-2.0-style) | separate program | Image formats FFmpeg and Rust do not cover: PSD, HEIC, JPEG XL, JPEG 2000, SVG rendering, ICO, PCX, XCF, camera RAW and more |
| **Rust, in the core itself** | — | MIT / Apache-2.0 crates | linked | Data (JSON, YAML, TOML, CSV, TSV, XML), spreadsheets read (XLSX, XLS, XLSB, ODS) and XLSX written, common images, archives (ZIP, TAR, TAR.GZ) |
| **LibreOffice** | installed | MPL-2.0 | separate program, if present | DOC/DOCX/ODT/RTF ↔ each other and PDF with layout kept; XLS/XLSX/ODS ↔ each other and PDF; PPT/PPTX/ODP ↔ each other and PDF |
| **Calibre** | installed | GPL-3.0 | separate program, if present | MOBI, AZW3 and other ebook formats |

All of these are called as separate programs or loaded at run time, so their licences
stay with them. The core and the application are `GPL-3.0-or-later` with the EON Module
ABI Exception, like EON Download Manager, so FFmpeg's GPL build and Pandoc sit beside
them without a conflict.

Sizes fetched on first need (Windows x64): FFmpeg about 190 MB unpacked (two
executables kept), Pandoc 44 MB zip, Typst 22 MB, PDFium 4 MB, ImageMagick 13 MB. A person
who only converts images and data fetches nothing.

## Format families

The routing graph has a node per format and an edge per conversion an engine can do in
one step. Families group what the application shows:

- **Video:** MP4, MKV, WebM, MOV, AVI, WMV, FLV, MPEG, MPEG-TS, M4V, 3GP, OGV, and GIF,
  animated WebP and APNG as targets (and sources).
- **Audio:** MP3, AAC, M4A, WAV, FLAC, OGG (Vorbis), Opus, WMA, AIFF, AC3, ALAC (M4A),
  AMR, MKA. Any video's sound can be saved as any of these.
- **Image:** PNG, JPEG, WebP, GIF, BMP, TIFF, ICO, AVIF, HEIC (read), JPEG XL, JPEG 2000,
  TGA, QOI, PNM, HDR, EXR, PSD (read), SVG (render), DDS (read), PCX, XCF (read).
- **Document:** TXT, MD (CommonMark, GFM), HTML, DOCX, ODT, RTF, EPUB, PDF, LaTeX, reST,
  Org, AsciiDoc, Textile, MediaWiki, DokuWiki, FB2, DocBook, IPYNB, Typst, man.
- **Spreadsheet:** XLSX, XLS, XLSB, ODS, CSV, TSV, plus JSON, HTML and Markdown tables as
  targets. With LibreOffice: ODS and XLS written, PDF.
- **Presentation:** PPTX written from a document (Pandoc). With LibreOffice: PPT, PPTX,
  ODP between each other and to PDF.
- **Data:** JSON, YAML, TOML, CSV, TSV, XML.
- **Subtitles:** SRT, WebVTT, ASS/SSA.
- **Archive:** ZIP, TAR, TAR.GZ repacked into each other. 7z and RAR belong to EON Zip,
  which is not built yet.

Fonts, 3D models, CAD drawings and disk images are out of scope for the first version.
They need engines of their own, and none was asked for.

## Routing

A conversion is a shortest path through the graph. Each edge carries a cost, and an edge
that loses something (a re-encode, a format with fewer features) costs more than one
that keeps everything (a container change, a lossless copy). So the planner prefers:

- a remux to a re-encode (MKV → MP4 with `-c copy` when the codecs allow it);
- one step to two;
- a route that keeps text as text (DOCX → Markdown through Pandoc, not through PDF).

When a route has more than one step, the intermediate files live in a work folder and are
deleted at the end. The application shows the route ("PDF → text → Markdown") and marks
the steps that lose something.

## EON Download Manager, later

The core answers two questions that EON Download Manager needs:

- **Which formats can this become?** It answers from a format alone, before anything is
  downloaded: from the file's extension or MIME type, or from the formats a page offers.
- **Which source should be downloaded for a target?** It answers with the cheapest route.
  A page offering MP4 (H.264/AAC) and WebM, when MOV is wanted, downloads the MP4 and
  remuxes it, with no re-encode.

The dialog the owner described then needs only the answer: "This content's original
format is WebM. You want MOV. It will be downloaded as MP4 and converted automatically by
EON Converter. Continue? [Continue] [Cancel] [ ] Do not ask again."
