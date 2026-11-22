<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Zip — research and analysis

**22 November 2026.** Steps one and two: research, then analysis. It is followed by
[parity.md](parity.md) (every capability of 7-Zip and WinRAR, and where EON Zip stands on
each) and [plan.md](plan.md). The decision is plan madde 49.

## What was asked

The owner asked for an archiver: find out why the community loves WinRAR and 7-Zip, what
they do well and how they do it, and build one that leaves **nothing those two can do that
it cannot**. On top of that it should do the useful things neither does, with a clean,
current interface.

## Why people love 7-Zip

1. **Free, with no catch.** Open source since 1999: no trial, no nag, no ads, no bundled
   software. This is the reason given most often, usually in contrast with WinRAR's
   "please buy" dialog.
2. **The best compression ratio among common tools.** Its own 7z format with LZMA2 regularly
   beats ZIP and often beats RAR. Reviewers say this more than anything else about it.
3. **It opens almost anything.** About 40 formats read: RAR and RAR5, ISO, CAB, MSI, WIM,
   DMG, VHD(X), VMDK, NSIS installers, CPIO, RPM, DEB, LZH, ARJ, file system images (NTFS,
   FAT, EXT, HFS, SquashFS).
4. **Strong encryption.** AES-256 in 7z, with the option of encrypting the file names too
   (header encryption), so an encrypted archive does not show what it holds.
5. **Small and fast to start; a console tool for scripts** (`7z`, `7za`, `7zr`, `7zz`).
6. **Right-click integration:** "Extract here", "Extract to *name*\", "Add to *name*.7z",
   and a hashing menu (CRC-32, SHA-256 and more).

**What people dislike:** an interface that "looks like Windows 95" and has not changed in
years; drag and drop that is limited; **no recovery records and no repair**; no preview;
no Windows 11 top-level context menu without a fork (NanaZip exists for exactly that); and
a security record that matters for a program that opens files from the internet
(CVE-2025-0411: Mark of the Web dropped by nested archives; CVE-2025-11001/11002:
symbolic links in ZIPs writing outside the target folder).

## Why people love WinRAR

1. **Habit.** Many people, and many companies, have used it for twenty years. Support
   staff and users know where everything is.
2. **Reliability features nobody else has together:** a **recovery record** inside the
   archive (3–5 % of its size is the documented sweet spot), **recovery volumes** (`.rev`
   files that rebuild missing parts of a multi-part archive: 3 recovery volumes replace any
   3 missing ones), and **repair**.
3. **Solid archives** with good speed, and fast extraction that users consistently notice.
4. **A polished feature set for daily work:** archive comments, locking an archive against
   change, self-extracting archives with their own icon, text and options, compression
   profiles, a password organiser, converting archives between formats, finding files
   inside archives, a benchmark, a virus-scan hook, BLAKE2 checksums stored in the archive.
5. **The "infinite trial"** — the trial never actually ends, which became a meme and a
   reason for affection rather than resentment.

**What people dislike:** it is paid, and it asks; the interface is dated; the RAR format is
proprietary; and its security record is serious — CVE-2023-38831 and CVE-2025-8088 (a path
traversal used by several state-sponsored groups to drop malware into the Startup folder)
were exploited in the wild.

## How they do it

| Capability | How 7-Zip / WinRAR do it | How EON Zip does it |
|---|---|---|
| High ratio | LZMA2 (dictionary up to 1.5 GB, binary-tree match finder), PPMd for text, BCJ/BCJ2 filters for executables, solid blocks | The same algorithms through `sevenz-rust2` (LZMA, LZMA2, PPMd, BZip2, Zstandard, Brotli, LZ4; BCJ x86/ARM/ARM64/RISC-V, BCJ2, Delta; AES-256) — pure Rust |
| ZIP everywhere | Deflate, Deflate64, BZip2, LZMA, PPMd, AES (WinZip AE-2) | The `zip` crate: Deflate, BZip2, Zstandard, XZ, PPMd and AES-256 written; Deflate64 and LZMA read |
| Tar family | Built in | `tar`, with gzip, bzip2, xz and zstd streams |
| Reading 40 formats | 7-Zip's own decoders for each | 7-Zip itself (`7zz`), fetched the first time such a file is opened and pinned by SHA-256, as EON Converter fetches its engines. A separate program, so its licence (LGPL + unRAR restriction) does not combine with ours |
| Recovery records | RAR-only Reed–Solomon data inside the archive | **PAR2 recovery volumes for any archive** — an open, documented standard that QuickPar, MultiPar and par2cmdline read too |
| Repair | WinRAR rebuilds from the recovery record | Repair from PAR2 volumes; for ZIP, rebuilding the central directory from the local headers |
| Multi-threading | Both split work across cores | Zstandard and LZMA2 multi-threaded; several files compressed in parallel for ZIP |
| Shell integration | COM shell extension DLL | Classic context-menu entries under the user's own registry key (no administrator rights). Windows 11's top-level menu needs a signed sparse package, which waits for EON's signing keys (madde 30) |

## The one thing that cannot be matched, and why

**Creating RAR archives.** The RAR compression format belongs to RARLAB; the unRAR
licence forbids using its code to build a RAR-compatible archiver, and no free program
creates RAR (7-Zip does not either). EON Zip **opens and extracts** RAR and RAR5, and
**converts** a RAR into 7z or ZIP in one step. Every reason to create a RAR has an open
equivalent here: solid compression (7z), recovery data (PAR2), comments (ZIP carries one;
7z has no field for it, in 7-Zip either), and a lock replaced by something stronger: a
signed-off manifest of hashes that shows any change.

## What neither does, and EON Zip will

1. **Safe by default.** Both have had archives write outside the target folder. EON Zip
   refuses absolute paths, `..`, drive letters and links that point outside, always;
   keeps Windows' "from the internet" mark on everything extracted from a marked archive,
   nested ones included; warns before extracting programs, scripts and double extensions
   (`invoice.pdf.exe`); and stops at archive bombs (a ratio and a size limit, asked once).
2. **Smart extraction that never makes a mess.** One top folder: it is used as is. Loose
   files: they get a folder named after the archive. Never "folder inside folder", never
   a hundred files sprayed into Downloads.
3. **Look inside without extracting.** Pictures, text, code, Markdown, PDF and media
   previewed in the window, with search across the whole archive.
4. **Recovery for any format** (PAR2, above), and a health check that says in words what
   is wrong and what can be saved.
5. **Names that come out right.** Old ZIPs store names in a DOS code page; Turkish,
   Russian or Japanese names turn into nonsense in both tools unless you pass an option.
   EON Zip detects the code page (CP857, CP866, Shift-JIS, …) and shows it.
6. **An honest choice before compressing.** It samples the files and shows the expected
   size and time for each method, and stores already-compressed files (JPEG, MP4, ZIP)
   instead of wasting time on them.
7. **Integrity you can share:** a BLAKE3/SHA-256 manifest, checked in one click.
8. **One look on Windows, Linux and macOS**, English and Turkish, dark and light, every
   action from the keyboard. Free, no account, no telemetry, no nag.

## Sources

- 7-Zip: [download page and platforms](https://www.7-zip.org/download.html),
  [7zz manual (formats)](https://www.gsp.com/cgi-bin/man.cgi?topic=7ZZ),
  [Wikipedia](https://en.wikipedia.org/wiki/7-Zip),
  [TechRadar review](https://www.techradar.com/reviews/pc-mac/software/7-zip-review-1324265/review),
  [How-To Geek on alternatives](https://www.howtogeek.com/i-tried-replacing-7-zip-with-this-open-source-alternative-heres-how-it-went/),
  [7-Zip 2026 review](https://shouldiuse.io/r/7-zip),
  [Tom's Hardware on CVE-2025-0411, -11001, -11002](https://www.tomshardware.com/tech-industry/cyber-security/7-zip-flaws-open-door-to-remote-code-execution).
- WinRAR: [solid archives](https://documentation.help/WinRAR/HELPArcSolid.htm),
  [recovery volumes](https://documentation.help/WinRAR/HELPArcRecVolumes.htm),
  [RAR5 recovery records analysed](https://www.liamfoot.com/analysing-the-effectiveness-of-winrars-rar5-recovery-records),
  [RAR format](https://en.wikipedia.org/wiki/RAR_(file_format)),
  [AnandTech forum: WinRAR vs 7-Zip](https://forums.anandtech.com/threads/7zip-vs-winrar.2029400),
  [CVE-2025-8088 (NVD)](https://nvd.nist.gov/vuln/detail/cve-2025-8088),
  [BleepingComputer on its exploitation](https://bleepingcomputer.com/news/security/winrar-path-traversal-flaw-still-exploited-by-numerous-hackers/).
- Licensing: [Fedora on the unRAR licence](https://fedoraproject.org/wiki/Licensing:Unrar),
  [SPDX: UnRAR](https://spdx.org/licenses/UnRAR.html).
- Windows 11 menu: [Microsoft: integrate a packaged app with File Explorer](https://learn.microsoft.com/en-us/windows/apps/desktop/modernize/integrate-packaged-app-with-file-explorer).
- Libraries: [sevenz-rust2](https://lib.rs/crates/sevenz-rust2), [zip](https://docs.rs/crate/zip/9.6.0),
  [rust-par2](https://docs.rs/crate/rust-par2/latest).
