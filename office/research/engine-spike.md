<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# eon-office-engine: yrs or automerge — spike

**19 November 2026.** Answers the **[A]** in [roadmap.md](../roadmap.md) §2: which CRDT
library `eon-office-engine` is built on. Recommendation at the end; the code and the raw
output are below so anyone can run it again and argue with the numbers.

## What was measured

Four things the engine has to do for Writer, Sheets, Slides, Whiteboard and Loop, on the
same deterministic edits for both libraries (seeded xorshift; `yrs 0.24.0`,
`automerge 0.6.1`; Rust 1.98.1 release build; Intel Core i9-14900HX, Windows 11):

1. **Concurrent text editing.** A 10,000-character document; two replicas each make 2,000
   offline edits (70 % inserts of 1–5 characters, 30 % deletes of 1–3), one commit per edit
   — what live co-editing produces; then exchange.
2. **Interleaving.** Two people type a phrase at the same position, one character at a time.
   A bad merge interleaves their letters.
3. **Cells.** 1,000 map entries; both sides change the same 100.
4. **History.** One replica, 20,000 edits: saved size and the time to load it.

## Results

| | yrs | automerge |
|---|---|---|
| 1. Converged | yes | yes |
| 1. Edit time, 2 × 2,000 edits | ~25 ms | ~22 ms |
| 1. Merge time, both directions | **~4 ms** | ~30 ms |
| 1. Delta sent, per direction | **~20 KB** | ~230 KB (~220 KB compacted) |
| 1. Whole document encoded | 63 KB | **54 KB** |
| 2. Same-position typing | intact | intact |
| 3. Concurrent cell edits | converge; **one value kept, the other dropped** | converge; **both values kept** (`get_all`) |
| 4. 20,000-edit history: size | 256 KB | **186 KB** |
| 4. 20,000-edit history: load | **~10 ms** | ~107 ms |
| Dependency crates (normal edges) | **38** | 51 |
| Licence | MIT | MIT |

Three runs gave the same sizes and times within a few per cent. automerge's deltas shrink
if commits are batched; that trades away the per-edit history live co-editing needs, so the
per-edit number is the honest one here.

## Reading it

- **Live co-editing** is dominated by merge time and delta size; yrs is about 7× faster to
  merge and sends about 11× less per edit. For a person on a slow school or mobile
  connection (madde 40's assumption) that is the difference that is felt.
- **Loading a long document** is 10× faster with yrs, which matters for opening.
- **automerge stores less on disk** and, more importantly, **keeps both sides of a
  concurrent cell edit**. yrs resolves it silently. EON's rule — no edit disappears without
  a record (`sync.md`) — therefore needs a layer on top of yrs that records the losing
  value for map-like data (Sheets cells, Lists fields).
- **The interface is a webview.** Yjs, which yrs implements, is the most widely used CRDT
  in browser editors, with existing bindings for rich-text editors; the update format is the
  same on both sides. That is not measured here, and is a practical advantage, not a proof.

## Recommendation

**Build `eon-office-engine` on yrs**, with one obligation written into the engine's design:
**map and cell values record concurrent losers** (observe conflicting writes per key and
keep the losing value beside the winner, surfaced through the same conflict list the core
uses), so a spreadsheet never silently drops a colleague's number.

Re-run this spike before the engine's `0.1` and on every major version of either library.

## Raw output

```
base text 10000 chars, 2000 offline ops per replica

1. concurrent text editing, then exchange
   yrs       converged=true edit=24.9ms merge=4.3ms delta A->B=19489B B->A=20422B full=63177B
   automerge converged=true edit=20.3ms merge=28.3ms delta A->B=230086B (as change chunks; 219978B compacted) B->A=255684B full=54396B
2. two people typing at the same spot, character by character
   yrs       "Hello brave new cruel world" intact=true
   automerge "Hello cruel brave new world" intact=true
3. 100 of 1000 cells changed on both sides
   yrs       converged=true (per-key last-writer-wins)
   automerge converged=true (both values kept: get_all(c0) = 2)
4. a long history: size and load
   yrs       20k ops -> 256081B, load=10.0ms, text=29827 chars
   automerge 20k ops -> 185715B, load=112.7ms, text=29828 chars
```

## The spike

`Cargo.toml`:

```toml
[package]
name = "engine-spike"
version = "0.0.0"
edition = "2021"
publish = false

[dependencies]
yrs = "0.24"
automerge = "0.6"

[profile.release]
opt-level = 3
```

`src/main.rs`:

```rust
// EON Office engine spike: yrs (Yjs port) vs automerge, on the four things
// eon-office-engine needs. Deterministic: a seeded xorshift drives every edit.

use std::time::Instant;

struct Rng(u64);
impl Rng {
    fn next(&mut self) -> u64 {
        self.0 ^= self.0 << 13;
        self.0 ^= self.0 >> 7;
        self.0 ^= self.0 << 17;
        self.0
    }
    fn below(&mut self, n: u64) -> u64 {
        if n == 0 { 0 } else { self.next() % n }
    }
    fn word(&mut self) -> String {
        let len = 1 + self.below(5) as usize;
        (0..len).map(|_| (b'a' + self.below(26) as u8) as char).collect()
    }
}

#[derive(Clone, Copy)]
enum Op { Ins(u64, usize), Del(u64, u64) }

const BASE: usize = 10_000;
const OPS: usize = 2_000;

fn base_text() -> String {
    let mut r = Rng(42);
    let mut s = String::new();
    while s.len() < BASE { s.push_str(&r.word()); s.push(' '); }
    s.truncate(BASE);
    s
}

// ---------------------------------------------------------------- yrs
mod y {
    use super::*;
    use yrs::updates::decoder::Decode;
    use yrs::{Doc, GetString, Map, ReadTxn, StateVector, Text, Transact, Update};

    fn fresh(id: u64) -> Doc { Doc::with_client_id(id) }

    fn sync(from: &Doc, to: &Doc) -> usize {
        let sv = to.transact().state_vector();
        let update = from.transact().encode_state_as_update_v1(&sv);
        let n = update.len();
        to.transact_mut().apply_update(Update::decode_v1(&update).unwrap()).unwrap();
        n
    }

    fn edit(doc: &Doc, seed: u64, ops: usize) {
        let text = doc.get_or_insert_text("t");
        let mut r = Rng(seed);
        for _ in 0..ops {
            let mut txn = doc.transact_mut();
            let len = text.len(&txn) as u64;
            if r.below(10) < 7 || len < 10 {
                let w = r.word();
                text.insert(&mut txn, r.below(len + 1) as u32, &w);
            } else {
                let at = r.below(len - 3);
                text.remove_range(&mut txn, at as u32, 1 + r.below(3) as u32);
            }
        }
    }

    pub fn concurrent_text() -> String {
        let a = fresh(1);
        { let t = a.get_or_insert_text("t"); let mut txn = a.transact_mut(); t.insert(&mut txn, 0, &base_text()); }
        let b = fresh(2);
        sync(&a, &b);
        let start = Instant::now();
        edit(&a, 7, OPS);
        edit(&b, 9, OPS);
        let edit_ms = start.elapsed().as_secs_f64() * 1000.0;
        let start = Instant::now();
        let ab = sync(&a, &b);
        let ba = sync(&b, &a);
        let merge_ms = start.elapsed().as_secs_f64() * 1000.0;
        let (ta, tb) = (a.get_or_insert_text("t"), b.get_or_insert_text("t"));
        let same = ta.get_string(&a.transact()) == tb.get_string(&b.transact());
        let full = a.transact().encode_state_as_update_v1(&StateVector::default()).len();
        format!("converged={same} edit={edit_ms:.1}ms merge={merge_ms:.1}ms delta A->B={ab}B B->A={ba}B full={full}B")
    }

    pub fn interleaving() -> String {
        let a = fresh(1);
        { let t = a.get_or_insert_text("t"); let mut txn = a.transact_mut(); t.insert(&mut txn, 0, "Hello world"); }
        let b = fresh(2);
        sync(&a, &b);
        // Typed one character at a time, at the same spot, offline.
        for (doc, word) in [(&a, " brave new"), (&b, " cruel")] {
            let t = doc.get_or_insert_text("t");
            for (i, c) in word.chars().enumerate() {
                let mut txn = doc.transact_mut();
                t.insert(&mut txn, 5 + i as u32, &c.to_string());
            }
        }
        sync(&a, &b); sync(&b, &a);
        let s = a.get_or_insert_text("t").get_string(&a.transact());
        format!("{s:?} intact={}", s.contains(" brave new") && s.contains(" cruel"))
    }

    pub fn cells() -> String {
        let a = fresh(1);
        { let m = a.get_or_insert_map("cells"); let mut txn = a.transact_mut(); for i in 0..1000 { m.insert(&mut txn, format!("c{i}"), i as i64); } }
        let b = fresh(2);
        sync(&a, &b);
        for (doc, v) in [(&a, 1i64), (&b, 2i64)] {
            let m = doc.get_or_insert_map("cells");
            let mut txn = doc.transact_mut();
            for i in 0..100 { m.insert(&mut txn, format!("c{i}"), v * 1_000_000 + i); }
        }
        sync(&a, &b); sync(&b, &a);
        let (ma, mb) = (a.get_or_insert_map("cells"), b.get_or_insert_map("cells"));
        let (ta, tb) = (a.transact(), b.transact());
        let same = (0..1000).all(|i| ma.get(&ta, &format!("c{i}")) == mb.get(&tb, &format!("c{i}")));
        format!("converged={same} (per-key last-writer-wins)")
    }

    pub fn history() -> String {
        let a = fresh(1);
        edit(&a, 3, 20_000);
        let bytes = a.transact().encode_state_as_update_v1(&StateVector::default());
        let start = Instant::now();
        let b = fresh(5);
        b.transact_mut().apply_update(Update::decode_v1(&bytes).unwrap()).unwrap();
        let load_ms = start.elapsed().as_secs_f64() * 1000.0;
        let len = b.get_or_insert_text("t").len(&b.transact());
        format!("20k ops -> {}B, load={load_ms:.1}ms, text={len} chars", bytes.len())
    }
}

// ---------------------------------------------------------------- automerge
mod am {
    use super::*;
    use automerge::{transaction::Transactable, ActorId, AutoCommit, ObjType, ReadDoc, ScalarValue, ROOT};

    fn text_id(doc: &AutoCommit) -> automerge::ObjId {
        doc.get(ROOT, "t").unwrap().unwrap().1
    }

    fn edit(doc: &mut AutoCommit, seed: u64, ops: usize) {
        let t = text_id(doc);
        let mut r = Rng(seed);
        for _ in 0..ops {
            let len = doc.length(&t) as u64;
            if r.below(10) < 7 || len < 10 {
                let w = r.word();
                doc.splice_text(&t, r.below(len + 1) as usize, 0, &w).unwrap();
            } else {
                let at = r.below(len - 3);
                doc.splice_text(&t, at as usize, 1 + r.below(3) as isize, "").unwrap();
            }
            doc.commit();
        }
    }

    fn sync(from: &mut AutoCommit, to: &mut AutoCommit) -> usize {
        let heads = to.get_heads();
        let changes: Vec<u8> = from.get_changes(&heads).iter().flat_map(|c| c.raw_bytes().to_vec()).collect();
        let n = changes.len();
        to.load_incremental(&changes).unwrap();
        n
    }

    fn fresh(actor: u8, with_text: &str) -> AutoCommit {
        let mut d = AutoCommit::new().with_actor(ActorId::from(vec![actor; 16]));
        if !with_text.is_empty() {
            let t = d.put_object(ROOT, "t", ObjType::Text).unwrap();
            d.splice_text(&t, 0, 0, with_text).unwrap();
            d.commit();
        }
        d
    }

    pub fn concurrent_text() -> String {
        let mut a = fresh(1, &base_text());
        let mut b = a.fork().with_actor(ActorId::from(vec![2u8; 16]));
        let _ = a.save();
        let start = Instant::now();
        edit(&mut a, 7, OPS);
        edit(&mut b, 9, OPS);
        let edit_ms = start.elapsed().as_secs_f64() * 1000.0;
        let compact = a.save_incremental().len();
        let start = Instant::now();
        let ab = sync(&mut a, &mut b);
        let ba = sync(&mut b, &mut a);
        let merge_ms = start.elapsed().as_secs_f64() * 1000.0;
        let same = a.text(text_id(&a)).unwrap() == b.text(text_id(&b)).unwrap();
        let full = a.save().len();
        format!("converged={same} edit={edit_ms:.1}ms merge={merge_ms:.1}ms delta A->B={ab}B (as change chunks; {compact}B compacted) B->A={ba}B full={full}B")
    }

    pub fn interleaving() -> String {
        let mut a = fresh(1, "Hello world");
        let mut b = a.fork().with_actor(ActorId::from(vec![2u8; 16]));
        for (doc, word) in [(&mut a, " brave new"), (&mut b, " cruel")] {
            let t = text_id(doc);
            for (i, c) in word.chars().enumerate() {
                doc.splice_text(&t, 5 + i, 0, &c.to_string()).unwrap();
                doc.commit();
            }
        }
        sync(&mut a, &mut b); sync(&mut b, &mut a);
        let s = a.text(text_id(&a)).unwrap();
        format!("{s:?} intact={}", s.contains(" brave new") && s.contains(" cruel"))
    }

    pub fn cells() -> String {
        let mut a = AutoCommit::new().with_actor(ActorId::from(vec![1u8; 16]));
        let m = a.put_object(ROOT, "cells", ObjType::Map).unwrap();
        for i in 0..1000 { a.put(&m, format!("c{i}"), i as i64).unwrap(); }
        a.commit();
        let mut b = a.fork().with_actor(ActorId::from(vec![2u8; 16]));
        for (doc, v) in [(&mut a, 1i64), (&mut b, 2i64)] {
            for i in 0..100 { doc.put(&m, format!("c{i}"), v * 1_000_000 + i).unwrap(); }
            doc.commit();
        }
        sync(&mut a, &mut b); sync(&mut b, &mut a);
        let get = |d: &AutoCommit, k: String| d.get(&m, k).unwrap().map(|(v, _)| v.to_scalar().cloned().unwrap_or(ScalarValue::Null));
        let same = (0..1000).all(|i| get(&a, format!("c{i}")) == get(&b, format!("c{i}")));
        let conflicts = a.get_all(&m, "c0").unwrap().len();
        format!("converged={same} (both values kept: get_all(c0) = {conflicts})")
    }

    pub fn history() -> String {
        let mut a = fresh(1, " ");
        edit(&mut a, 3, 20_000);
        let bytes = a.save();
        let start = Instant::now();
        let b = AutoCommit::load(&bytes).unwrap();
        let load_ms = start.elapsed().as_secs_f64() * 1000.0;
        let len = b.length(text_id(&b));
        format!("20k ops -> {}B, load={load_ms:.1}ms, text={len} chars", bytes.len())
    }
}

fn main() {
    println!("base text {BASE} chars, {OPS} offline ops per replica\n");
    println!("1. concurrent text editing, then exchange");
    println!("   yrs       {}", y::concurrent_text());
    println!("   automerge {}", am::concurrent_text());
    println!("2. two people typing at the same spot, character by character");
    println!("   yrs       {}", y::interleaving());
    println!("   automerge {}", am::interleaving());
    println!("3. 100 of 1000 cells changed on both sides");
    println!("   yrs       {}", y::cells());
    println!("   automerge {}", am::cells());
    println!("4. a long history: size and load");
    println!("   yrs       {}", y::history());
    println!("   automerge {}", am::history());
}
```
