<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Zip — parity with 7-Zip and WinRAR

The owner's bar (22 November 2026): **nothing 7-Zip or WinRAR can do that EON Zip cannot.**
This table is how that is kept honest. Every row is a capability of one or both; the last
column says where EON Zip stands. It is updated in the same pull request as the work.

✅ in the named version · ⏳ planned, with the version it is aimed at · ✗ cannot be done,
with the reason

## Formats

| Capability | 7-Zip | WinRAR | EON Zip |
|---|---|---|---|
| A format of its own, with its own extension | `.7z` | `.rar` | ✅ core v0.0.1 — **`.eonz`** (madde 49i, `eonz.md`); the window and command line next |
| Create and extract 7z (LZMA, LZMA2, PPMd, BZip2, BCJ/BCJ2 filters) | ✓ | — / extract | ✅ v0.0.0 |
| 7z AES-256 encryption | ✓ | — | ✅ v0.0.0 |
| 7z encrypted file names (header encryption) | ✓ | — | ✅ v0.0.0 |
| 7z solid blocks | ✓ | — | ✅ v0.0.0 |
| Create and extract ZIP (Deflate, BZip2, LZMA, PPMd, XZ, Zstandard) | ✓ | Deflate | ✅ v0.0.0 (Deflate64 and LZMA read only, as the library) |
| ZIP AES-256 (WinZip AE-2) | ✓ | ✓ | ✅ v0.0.0 |
| ZIP comment | ✓ | ✓ | ✅ v0.0.0 |
| TAR, TAR.GZ, TAR.BZ2, TAR.XZ, TAR.ZST | ✓ | extract | ✅ v0.0.0 |
| Single-file GZIP, BZIP2, XZ, Zstandard | ✓ | extract | ✅ v0.0.0 |
| Create RAR / RAR5 | — | ✓ | **✗ — the RAR format is RARLAB's; its licence forbids building a RAR compressor, and no free program creates RAR. Open equivalents for every reason to use it are below.** |
| Extract RAR / RAR5 | ✓ | ✓ | ⏳ v0.0.1, through 7-Zip (`7zz`) fetched and pinned |
| Extract ISO, UDF, CAB, MSI, WIM, DMG, VHD(X), VMDK, QCOW2, NSIS, CPIO, RPM, DEB, LZH, ARJ, Z, CHM, file-system images | ✓ | partly | ⏳ v0.0.1, through 7-Zip |
| Create WIM | ✓ | — | ⏳ v0.0.1, through 7-Zip |

## Working with archives

| Capability | 7-Zip | WinRAR | EON Zip |
|---|---|---|---|
| Browse inside, folders and all | ✓ | ✓ | ✅ v0.0.0 |
| Extract all / selected / here / to a named folder | ✓ | ✓ | ✅ v0.0.0 (smart extraction by default) |
| Test integrity | ✓ | ✓ | ✅ v0.0.0 |
| Split into volumes of a chosen size | ✓ | ✓ | ✅ v0.0.0 (`.001`, `.002`…) |
| Open and extract from volumes | ✓ | ✓ | ✅ v0.0.0 (`.001` sets); ⏳ v0.0.1 WinZip `.z01` and RAR `.partN` |
| Add, delete, rename inside an existing archive | ✓ | ✓ | ⏳ v0.0.1 |
| Convert between archive formats | — | ✓ | ✅ v0.0.0 |
| Self-extracting archives (SFX) | ✓ | ✓ (with icon, text, options) | ⏳ v0.0.2 |
| Recovery record / recovery volumes | — | ✓ (RAR only) | ✅ core v0.0.1 inside `.eonz` (Reed-Solomon, repairs without the password) · ⏳ PAR2, for any other archive |
| Repair a damaged archive | — | ✓ | ⏳ v0.0.1 — from PAR2; ZIP central directory rebuilt |
| Lock an archive against change | — | ✓ | ⏳ v0.0.2 — a hash manifest that shows any change |
| Compression profiles | — | ✓ | ⏳ v0.0.1 |
| Password organiser | — | ✓ | ⏳ v0.0.2 (in the system keychain) |
| Find files inside many archives | — | ✓ | ⏳ v0.0.2 |
| Delete files after archiving / archive after extracting | ✓ | ✓ | ⏳ v0.0.1 |
| Exclude / include patterns | ✓ | ✓ | ⏳ v0.0.1 |
| Symbolic and hard links, NTFS streams, timestamps, attributes | ✓ | ✓ | ✅ v0.0.0 timestamps and permissions; ⏳ v0.0.2 links and streams |
| File name code page for old ZIPs | option | option | ✅ v0.0.0 — detected, not an option |
| Keep the "from the internet" mark | option | option | ✅ v0.0.0 — always, nested archives too |

## Around the archive

| Capability | 7-Zip | WinRAR | EON Zip |
|---|---|---|---|
| Checksums: CRC-32, SHA-1, SHA-256, SHA-512, MD5, BLAKE (2sp / 3), XXH64 | ✓ | BLAKE2 | ✅ v0.0.0 (BLAKE3) |
| Benchmark | ✓ | ✓ | ⏳ v0.0.1 |
| Multi-threaded compression | ✓ | ✓ | ✅ v0.0.0 LZMA2 and Zstandard; ⏳ v0.0.1 several ZIP entries at once |
| Right-click menu in the file manager | ✓ | ✓ | ✅ v0.0.0 on Windows (classic menu, per user); ⏳ Windows 11 top-level menu after signing keys (madde 30) |
| File associations | ✓ | ✓ | ✅ v0.0.0 (opens what it is given) |
| Command line | ✓ | ✓ | ✅ v0.0.0 |
| Virus-scan hook | — | ✓ | ⏳ v0.0.2 (the system's own scanner) |
| Windows, Linux, macOS | console only off Windows | Windows (console rar elsewhere) | ✅ v0.0.0, the same window everywhere |
| Free, open source, no nag | ✓ | — | ✅ |

## Beyond both

| Capability | EON Zip |
|---|---|
| Refuses paths outside the target, links outside it, absolute paths — always | ✅ v0.0.0 |
| Warns before programs, scripts and double extensions | ✅ v0.0.0 |
| Stops at archive bombs | ✅ v0.0.0 |
| Says when a ZIP password will not open elsewhere (7-Zip refuses letters outside ASCII) | ✅ v0.0.0 |
| Smart extraction: never a folder inside a folder, never files sprayed | ✅ v0.0.0 |
| Preview inside the archive without extracting | ✅ v0.0.0 (text, pictures) · ⏳ PDF and media |
| Search across the archive's names | ✅ v0.0.0 |
| Expected size and time for each method, before compressing | ⏳ v0.0.1 |
| Already-compressed files stored, not recompressed | ✅ v0.0.0 |
| Content repeated across files stored once (content-defined chunks), not only identical whole files | ✅ core v0.0.1 (`.eonz`) |
| Decompression on every core (7z's LZMA2 decompresses on one) | ✅ core v0.0.1 (`.eonz`) |
| Authenticated encryption with a memory-hard key (AES-256-GCM, Argon2id), names always hidden | ✅ core v0.0.1 (`.eonz`) |
| BLAKE3 for every block, chunk and file, checked without the password | ✅ core v0.0.1 (`.eonz`) |
| Shareable hash manifest, checked in one click | ⏳ v0.0.1 |
| Health report in words | ⏳ v0.0.1 |
