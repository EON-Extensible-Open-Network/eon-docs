<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Zip — plan

**22 November 2026.** Step three, after [research.md](research.md); the capability list is
[parity.md](parity.md). The decision is plan madde 49.

## Shape

Two repositories, following madde 45 (one product, its own core and application):

| Repository | What it is | Licence |
|---|---|---|
| `eon-zip-core` | The library: formats, listing, extraction with its safety rules, creation, testing, volumes, hashing, conversion between formats, and later the fetched 7-Zip for the formats it alone reads. No window, no GUI dependency. | `GPL-3.0-or-later` + EON Module ABI Exception |
| `eon-zip-app` | The window (Tauri, static HTML/CSS/JS, no bundler) and a command line (`list`, `extract`, `create`, `test`, `hash`, `--selftest`). | `GPL-3.0-or-later` + exception |

EON Converter's core already reads and writes ZIP and TAR for its own conversions. That
small piece is not shared: EON Zip's core is the archiver, and the converter may later use
it instead of its own, with the reason written at the dependency (madde 45).

## The core's API

```rust
let archive = Archive::open(path, password)?;        // any supported format, volumes joined
let entries = archive.entries();                       // names (code page detected), sizes, times, flags
let plan    = archive.extract_plan(dest, Smart);       // where each file goes, and every warning, first
archive.extract(&plan, &only, progress, cancel)?;      // refuses anything outside dest
archive.test(progress, cancel)?;                       // reads every byte, checks every CRC
create(sources, target, &Settings { format, level, method, password, solid, split }, progress, cancel)?;
hash(paths, &[Crc32, Sha256, Blake3], progress)?;
```

## Safety, in the core, not the window

Every extraction goes through one function that decides where an entry may be written:
absolute paths, drive letters, `..`, UNC paths and links pointing outside the target are
refused, always, with the reason. On Windows the Zone.Identifier stream of a marked archive
is copied to every file written. A total size or ratio past a limit stops and asks. Names
ending in executable extensions, and double extensions, are listed as warnings in the plan
before anything is written.

## The window

Designed with the design skills before the code, in the family of EON Download Manager and
EON Converter. It is an archive browser first:

- open an archive (drop, *Open with*, right-click, or the button) and see its folders, with
  a breadcrumb, search, sizes and ratios;
- a preview pane for text and pictures, without extracting;
- **Extract** (smart by default, or choose the folder) and **Extract selected**;
- **Create** as a sheet: format, level, method, password, encrypted names, split size,
  solid — with the size each choice is expected to give;
- Test, Hash, Convert; a queue with progress and cancel;
- Settings: the right-click menu on Windows, theme, language.

## Verification for `v0.0.0`

- Unit tests: the path rules (every traversal shape from the CVEs above), smart extraction,
  code-page detection, volumes, every format created then read back byte for byte.
- An interoperability test: archives made by EON Zip are opened by the real 7-Zip and the
  other way round, in CI (7-Zip is installed on the runners).
- `--selftest` in the release workflow; a window check over the DevTools protocol.
- After the release: download it, check the checksum, and create, test and extract real
  archives with the published binary.
