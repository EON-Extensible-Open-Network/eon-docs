<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Zip — status

**As of 28 November 2026.** Plan madde 49; [research](research.md), [parity](parity.md),
[plan](plan.md), [design](design.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-zip-core` | `v0.0.0` (tag, library); `0.0.1` on `main` | v0.0.0 green on Windows, Linux, macOS | 7z, ZIP, the tar family and single compressed files, made and read; volumes; hashes; conversion; the path rules in `src/safe.rs`; **`.eonz`, EON Zip's own format** (0.0.1) |
| `eon-zip-app` | `v0.0.1`, prerelease (`.eonz`) | green on Windows, Linux, macOS | release downloaded after publishing: checksums match; the Windows binary passes `--selftest`, makes a password-locked 7z of a folder with Turkish names that the real 7-Zip tests as intact, extracts it byte for byte, and opens its window |

**Tested:** in the core, 12 unit and 11 integration tests: every format made and read back
byte for byte, passwords (7z with hidden names, ZIP AES-256), volumes, conversion, archives
built to attack (the climbing names of the WinRAR and 7-Zip CVEs, mutation-checked),
damage found by a test, the "from the internet" mark following every file, and **the real
7-Zip reading what EON Zip makes and EON Zip reading what 7-Zip makes** — on the Windows
CI runner too. In the app, `--selftest` (both languages, the interface, the attack shapes,
real 7z/ZIP/TAR.XZ archives) and `tools/ui-check.mjs`, 36 checks on the real window: a
hand-built hostile ZIP whose climbing names are refused and whose disguised program is
named before anything is written, a locked 7z, conversion, a new archive the real 7-Zip
opens, the right-click menu written and removed under a test key, Turkish and the light
theme. The screenshots were reviewed by eye too.

## `.eonz` (madde 49i, 28 November)

**v0.0.1 published and verified (Windows, headless):** SHA-256 matches; `--selftest`
passes with a password-locked `.eonz` round trip; an archive made by the downloaded
program, damaged on purpose, is repaired byte for byte. The window check
(`tools/ui-check.mjs`) waits until the machine is free (it opens a window).

The owner asked for a format of EON Zip's own. The core makes, reads and repairs `.eonz`
([specification](eonz.md)); the window and command line come next. Built in the
background (idle priority, two compile jobs) while the owner's brother played. Tests: 19
unit, 4 for `.eonz` (deduplication; names and contents hidden by the password, wrong and
missing passwords; 20 KB of damage in the middle, and the header itself, repaired byte for
byte; damage beyond the recovery data reported as such), and `.eonz` added to the
every-format round trip. Found while building: the first chunk masks had 13 bits set
where 18 were meant (chunks averaged 24 KiB, not 64); the masks are now generated from
their bit count and a test pins them. And a `.eonz` whose header is damaged was reported
as "not an archive"; it is now recognised by name and reported as damaged and repairable.

## Found while building

- **7-Zip refuses a ZIP password with any letter outside ASCII** (`ğ`, `é`: "Parameter is
  incorrect"). The ZIP standard does not say how a password becomes bytes. EON Zip warns
  when such a password is typed for a ZIP and suggests 7z, whose passwords are UTF-16.
- **An operation's result is the command's own reply.** It first travelled as an event,
  and in the window checks events were sometimes late; the outcome no longer depends on
  one. (The checks' own failures were a stale toast and key presses that a background
  window does not always receive; both were fixed in the check.)
- Layout faults seen only by eye — a squeezed file list, plurals like "1 files" — were
  fixed before release.

## Not yet

- RAR, ISO, CAB, DMG and the rest of what only 7-Zip reads: through 7-Zip itself, fetched
  and pinned, in v0.0.1. Downloading and running 7-Zip on this machine needs the owner's
  permission first (the automatic permission check stopped it).
- Creating RAR: not possible for anyone but RARLAB (research.md).
- Every ⏳ row in parity.md, each with its version.
