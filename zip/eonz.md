<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# The `.eonz` archive format, version 0

**28 November 2026. Unstable** (version 0: anything may change until version 1, and an
EON Zip that reads version 1 will say so plainly about a version 0 file rather than guess).
Plan madde 49i. Implemented in `eon-zip-core` (`src/eonz/`).

EON Zip's own format, at the owner's request (28 Nov: *"kendi sıkıştırma sistemimizi ekle,
bir uzantı ismi bul"*). It is not a new compression algorithm. A codec that beats Zstandard
or LZMA would be years of research, as LZMA itself was. It is a new **container**: proven
pieces put together in a way neither 7z nor RAR offers.

| | 7z | RAR5 | `.eonz` v0 |
|---|---|---|---|
| Compression | LZMA2 (decompression is single-threaded) | RAR's own | **Zstandard**, solid blocks compressed and decompressed in parallel |
| Repeated content across files | — | whole identical files only | **content-defined chunks stored once** (deduplication) |
| Recovery data inside the archive | — | recovery record | **Reed-Solomon recovery** (GF(2¹⁶)), any share, repairs without the password |
| Integrity | CRC32 | CRC32 / BLAKE2sp | **BLAKE3** for every block, chunk and file |
| Encryption | AES-256-CBC, no authentication, SHA-256 key stretching | AES-256-CBC, PBKDF2 | **AES-256-GCM** (authenticated), **Argon2id**; names always hidden |
| Names | UTF-16 | UTF-8 | UTF-8, `/`-separated, checked by EON Zip's extraction rules |

## Why `.eonz`

`.eon` is EON Studio's format (EON Reality), and EON is the umbrella's name, not the
archiver's. `.ezip` is free, but a Microsoft Store archiver is named "EZip". `.ezp` and
`.eza` belong to other programs. `.eonz` ("EON Zip") is registered nowhere we could find
(28 Nov 2026), is short, and names its product. Media type `application/x-eonz`.

## Layout

Everything little-endian. A file is:

```
header (128 bytes) | block … block | index block | recovery (optional) | trailer (64 bytes)
```

### Header (128 bytes, offset 0)

| Offset | Size | Field |
|---|---|---|
| 0 | 8 | magic `45 4F 4E 5A 0D 0A 1A 0A` (`EONZ\r\n\x1a\n`: like PNG's, it shows a text-mode transfer at once) |
| 8 | 2 | format version, `0` |
| 10 | 2 | flags: bit 0 encrypted, bit 1 recovery present; others must be 0 |
| 12 | 16 | archive id, random |
| 28 | 16 | Argon2id salt (zero if not encrypted) |
| 44 | 4 | Argon2id memory, KiB |
| 48 | 4 | Argon2id passes |
| 52 | 4 | Argon2id lanes |
| 56 | 8 | index block offset |
| 64 | 8 | index block stored length |
| 72 | 8 | recovery offset (0 if none) |
| 80 | 8 | recovery length |
| 88 | 24 | reserved, zero |
| 112 | 16 | BLAKE3 of bytes 0–111, first 16 bytes |

The trailer repeats the offsets, so either end of the file is enough to find the index.

### Block

```
stored length (u64) | stored bytes | BLAKE3 of the stored bytes (32)
```

The stored bytes are a Zstandard frame of the block's plain bytes. When the archive is
encrypted, they are that frame encrypted with AES-256-GCM: a 12-byte nonce (the block's
number, u64 little-endian, then four zero bytes; the key is unique per archive because the
salt is), the ciphertext and the 16-byte tag. The BLAKE3 covers what is stored, so damage is
found, and repaired, without the password.

The **index** is a block like any other, last before the recovery data: UTF-8 JSON (below),
compressed and, when the archive is encrypted, encrypted, so an encrypted archive shows
nothing about its files without the password.

### Key

Argon2id (RFC 9106) of the password's UTF-8 bytes with the header's salt and parameters,
32 bytes. Defaults: 64 MiB, 3 passes, 1 lane. Wrong passwords fail at the index's GCM tag,
which says "wrong password" and nothing else.

### Chunks and deduplication

Files are cut into chunks where their content says, not at fixed offsets. A 64-bit gear
hash rolls over the bytes (`h = (h << 1) + GEAR[byte]`, the table being 256 values of
SplitMix64 from seed `0x454F4E5A`), and a chunk ends after the byte where `h & mask` is
zero: no cut in the first 16 KiB, an 18-bit mask up to 64 KiB, a 14-bit mask after it, and
a forced cut at 256 KiB (`FastCDC`'s normalised cut, level 2). The masks set bits 63, 61,
59 … downwards. The same content therefore gives the same chunks wherever it sits in a
file, after an insertion too. A chunk is known by the BLAKE3 of its plain bytes and is
**stored once**, however many times it appears. Chunks fill blocks in order (solid
compression) up to the block size (4 MiB at the fastest level to 64 MiB at ultra), and
Zstandard's long-distance window spans the block from the Normal level up. The gear table
and the masks are part of version 0: changing them changes every cut.

### Index (JSON)

```json
{
  "blocks": [ { "offset": 128, "stored": 5123456, "plain": 16777216, "hash": "<blake3 hex>" } ],
  "chunks": [ { "block": 0, "at": 0, "len": 65536, "hash": "<blake3 hex>" } ],
  "entries": [
    { "path": "docs/readme.txt", "kind": "file", "size": 1234,
      "chunks": [0, 1], "hash": "<blake3 hex of the whole file>",
      "mtime": 1764320000000000000, "mode": 420, "attributes": 32 },
    { "path": "docs", "kind": "dir", "mtime": 1764320000000000000 },
    { "path": "link", "kind": "symlink", "target": "docs/readme.txt" }
  ],
  "comment": "optional text"
}
```

`mtime` is nanoseconds since 1970 (UTC). `mode` (Unix permission bits) and `attributes`
(Windows) are each kept where the archive was made and used where they mean something.
Extraction applies EON Zip's rules to every path and link target (madde 49d): nothing is
written outside the destination, whatever the index says.

### Recovery

The region from offset 0 to the end of the index block is cut into equal **shards**:
4 KiB, or more for a large archive so there are at most 262,144 (a multiple of 64 bytes;
the last shard is padded with zeros). Shard `j` belongs to **group** `j mod G`, at position
`j div G`, where `G` is the smallest number of groups that keeps each at 256 shards or
fewer. A burst of damage (a bad sector, a cut download) is spread thinly over every group
instead of sinking one, and memory stays a small fixed share of the archive. Each group
gets `ceil(shards per group × percent / 100)` parity shards (at least one; 5 % by default)
from Reed-Solomon over GF(2¹⁶) (`reed-solomon-simd`; positions past the last data shard
count as zeros).

```
fixed (48) | table | check (16) | parity shards | table | check | fixed (48)

fixed: "EONZREC0" | shard size u64 | data shards u64 | groups u32 | parity per group u32
       | region length u64 | shards per group u32 | reserved u32
table: BLAKE3-128 of every data shard, in order, then of every parity shard (group by group)
check: BLAKE3-128 of fixed + table
parity shards: group by group, each group's in order
```

The second copy ends with `fixed`, so it can be found from the end of the recovery data
when the first is damaged. To repair: hash each data shard, treat any that does not match
as lost, and rebuild a group from its parity when it lost no more shards than it has
undamaged parity. The original is never changed; the repaired copy is written beside it,
and its trailer is written again from its (repaired) header.

### Trailer (64 bytes, at the end)

| Offset | Size | Field |
|---|---|---|
| 0 | 8 | index block offset |
| 8 | 8 | index block stored length |
| 16 | 8 | recovery offset (0 if none) |
| 24 | 8 | recovery length |
| 32 | 16 | archive id (as in the header) |
| 48 | 8 | `EONZEND\0` |
| 56 | 8 | BLAKE3 of bytes 0–55, first 8 bytes |

## What version 0 does not have yet

Multi-volume `.eonz` (EON Zip's generic split works on it as on any file), appending to an
existing archive, LZMA2 as a second codec for the last few per cent, and a format comment
in the header. Version 1 freezes the layout; until then each EON Zip release reads the
`.eonz` files it writes and the self-test proves it.
