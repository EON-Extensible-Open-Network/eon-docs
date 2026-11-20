<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON DNS — plan

**21 November 2026.** Step three of four, after [research.md](research.md). The decision
is plan madde 47. The goal of this plan is **`v0.0.0`**: the encrypted-DNS half, published
and verified. The tunnel comes next, as its own slice.

## Shape

| Repository | What | Licence |
|---|---|---|
| `eon-dns-app` | A Cargo workspace with three parts: `engine/` (the local resolver, upstreams, filtering, cache, counters and system configuration, a library with no GUI dependency); `src-tauri/` (the window and the tray); `src/` (static HTML, CSS and JavaScript, no bundler). | `GPL-3.0-or-later` + EON Module ABI Exception |

There is no `-spec` repository yet. EON DNS speaks DNS, DoH, DoT and DoQ, which are other
people's standards, and it has no contract of its own for anyone else to implement. One
opens the day it does (madde 43d: no empty repositories). The same goes for a server
repository: a resolver EON would run waits for madde 30.

## The engine

- **Listener.** UDP and TCP on 127.0.0.1 and ::1, port 53. A test can choose another
  port.
- **Upstreams.** Every lookup goes out encrypted:
  - DoH, DoT or DoQ, through `hickory-resolver` 0.26 with rustls;
  - presets for Cloudflare (plain, malware, family), Quad9, Mullvad, AdGuard and DNS0, or
    any address the person enters;
  - one resolver, or several raced, where the first good answer wins;
  - plain DNS is never used upstream, unless the person explicitly adds a plain address,
    and the window says so.
- **DNSSEC.** Answers are validated on the device when switched on. A failure is answered
  as `SERVFAIL`, and the log says why.
- **Cache.** The cache respects TTLs, and its size can be changed.
- **Filtering.** Three published open lists can be switched on independently: malware and
  phishing, ads and trackers, adult content. On top of them come the person's own
  *always allow* and *always block* rules, with subdomains matching their parent.
  - A blocked name is answered with `0.0.0.0` or `::`, which fails fast in every browser.
  - The log and the counters say which list blocked it.
  - Lists refresh daily. A list that fails keeps its last good copy.
- **Counters.** Lookups, blocked, cache hits, upstream median latency, and per-upstream
  errors. They are kept in memory and reset at restart.
- **Log.** The last 500 lookups (name, type, answer, time, which list or upstream), kept
  **in memory only and off by default**. One press clears it.
- **Benchmark.** Twenty common names are timed against each candidate upstream, cache
  bypassed. The median and the failure count are shown, and the fastest is offered.

## System configuration (Windows first)

- **EON mode** (the switch):
  1. Start the listener.
  2. Point every connected adapter's DNS at 127.0.0.1 and ::1, writing down what they were
     before. Changing adapters needs administrator rights, so the program runs itself
     elevated once, through the system prompt, with `--apply` or `--restore`. That
     elevated run does one thing and exits.
  3. Turning off, quitting from the tray, and the next start after a crash all restore
     what was written down.
- **System mode.** On Windows 11, register the DoH template for the chosen resolver's
  addresses and point the adapters at them. Encryption is then the operating system's,
  and it keeps working with the program closed. There is no filtering or log in this mode,
  and the window says so.
- **Captive portals.** When lookups fail and the network reports a portal, EON offers to
  pause for two minutes.
- **Linux and macOS** run the listener and the window. Configuring the system there is
  documented (systemd-resolved, `networksetup`) and comes in a later alpha.

## The window

The design is made with the design skills, in the same research → plan → build → critique
loop as EON Download Manager.

- **The switch** is the screen. It says where lookups go now, and whether they are
  encrypted and checked.
- **Behind the switch:**
  - live counters, with a small chart of the last hour;
  - **Protection**: the three lists and the person's rules;
  - **Resolvers**: presets, custom entries, and the benchmark;
  - **Log**: off by default;
  - **Settings**: start with Windows, the tray, the cache size and language (English and
    Turkish).
- **The tray** shows the state at a glance and toggles it.

## Checks

- **Engine tests** run a local fake upstream (plain DNS on loopback) and cover the cache,
  filtering, rules, racing, DNSSEC failure handling and the log.
- **An ignored real-network test** sends real DoH, DoT and DoQ lookups to two public
  resolvers.
- **`--selftest`** is CI's check before a release. It starts the listener on a spare port
  and resolves through it without touching the system's DNS.
- **A window check** drives the window over CDP, as `tools/ui-check.mjs` does in EON
  Download Manager.
- **Apply and restore** are tested on the CI runner, where changing an adapter is
  harmless. They are never tested on the owner's machine without asking.

## Then

1. **The tunnel.** WireGuard to an endpoint the person brings, using `wireguard-nt` on
   Windows, with a kill switch.
2. **Per-app rules.**
3. **Linux and macOS system configuration.**
