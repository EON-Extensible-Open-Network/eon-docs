<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON DNS — research and analysis

**21 November 2026.** Steps one and two of four. The plan is [plan.md](plan.md); the
decision is plan madde 47.

## 0. What was asked

> "Build our own WARP / 1.1.1.1, called EON DNS: the same functions, cooler; leave nothing
> it can do that we cannot."

## 1. What Cloudflare's 1.1.1.1 app is

The app has two halves that are often confused.

- **1.1.1.1, the encrypted DNS half.**
  - Name lookups go to Cloudflare's resolver over DNS over HTTPS or DNS over TLS instead of
    in the clear.
  - The *Families* option switches to a resolver that blocks malware (1.1.1.2), or malware
    and adult content (1.1.1.3).
  - A DNS-only mode encrypts lookups and leaves every other packet alone.
- **WARP, the tunnel half.** All traffic goes through Cloudflare's network.
  - It started as WireGuard. Since late 2024 it runs over MASQUE (HTTP/3 proxying), with
    post-quantum key exchange and a TCP fallback; WireGuard remains selectable.
  - On desktops there is also a hidden *local proxy* mode, in which only apps configured
    to use it are tunnelled.
- **Zero Trust.** Corporate sign-in was moved out of the consumer app during 2026.

**What people like:**
- one switch;
- lookups that cannot be read or changed on the way;
- a resolver with a public no-logs audit (KPMG, 2019);
- for some, faster browsing.

**What people dislike:**
- slow or failing connections on some networks;
- battery and mobile-data use;
- no kill switch;
- no control over where lookups go;
- no view of what is blocked or why;
- everything depends on one company, which sees every lookup and, with WARP, every
  connection.

## 2. What cannot be copied, and what can

**WARP's real asset is Cloudflare's global network.** It is hundreds of data centres that
the tunnel ends in. EON has no network and, under madde 30, runs no infrastructure before
the legal gate. Building "our own 1.1.1.1" in the sense of a public resolver is an
operations commitment, not software. Cloudflare's WARP endpoints themselves are private
API territory: registering against them from a third-party client would sit on someone
else's terms of service. **EON DNS does not do that.**

Everything the app does on the device can be built, and done better:

| 1.1.1.1 app | EON DNS |
|---|---|
| Encrypted DNS to one company | Encrypted DNS (DoH, DoT, DoQ) to **any** resolver: Cloudflare, Quad9, Mullvad, AdGuard, DNS0, a person's own. Several at once, the fastest answering. |
| *Families*: two fixed filter levels, decided by the resolver | Filtering **on the device**: malware, trackers and ads, adult content, each from published open lists, plus the person's own allow and block rules, with a reason shown for every block. |
| No view of lookups | A **query log kept only on the device**, off by default, with a one-press clear. Live counters: lookups, blocked, cache hits, median latency. |
| DNSSEC: whatever the resolver does | **DNSSEC checked on the device**, so a lying resolver is caught. |
| Pick nothing, trust one network | **A resolver benchmark**: real lookups timed against each candidate from this connection, with the fastest suggested. |
| One mode for the whole system | **System mode**: on Windows 11, the operating system's own encrypted DNS is configured, and it keeps working with EON DNS closed. **EON mode**: a local resolver at 127.0.0.1 with everything above. |
| A tunnel to Cloudflare | **A tunnel to an endpoint the person brings**: their own server or a provider's WireGuard configuration. It ends on infrastructure they chose, not ours. |
| Captive portals break it | Captive portals are detected, and the person is offered a temporary bypass for the portal's names only. |
| Closed source | Open source, no telemetry, no account needed. |

## 3. How it is done, technically

- **Resolver library.** [Hickory DNS](https://github.com/hickory-dns/hickory-dns) 0.26 (Rust)
  covers what is needed:
  - a resolver with DoH, DoQ and DoH3;
  - DNSSEC validation back to the root, NSEC and NSEC3 included;
  - a server library that can answer DNS on a socket.

  Its own README still says the `hickory-dns` binary is not recommended for production. We
  use the libraries, not the binary, and test what we use.
- **A local resolver.** It listens on 127.0.0.1 and ::1, port 53. On Windows a port below
  1024 needs no administrator rights to bind; changing the network adapters' DNS servers
  does. EON DNS therefore asks once, through the system's elevation prompt, to point the
  adapters at itself. It writes down what they were before, and puts them back when it
  stops. Because a crash would otherwise leave the machine resolving through a dead
  address, the next start notices that and repairs it.
- **System mode.** Windows 11 can register a DoH template per resolver address
  (`netsh dns add encryption`). EON DNS can configure that and step aside, so it costs
  nothing when closed.
- **Lists.** Filter lists are published plain-text domain lists. They are fetched over
  HTTPS, checked for size and shape, and kept beside the program's data. A list that fails
  to parse keeps the last good copy.
- **Tunnel.** The tunnel needs a virtual network adapter: `wireguard-nt` on Windows, a
  signed driver distributed by the WireGuard project. That is a second, larger piece of
  work, and it is planned after the DNS half ships.

## 4. Analysis: what EON DNS must be

1. **One switch, as on 1.1.1.1.** The big round control is the product. Every other
   control can be ignored.
2. **Honest about what it is.** The interface never says "faster internet". It says what
   is encrypted, where lookups go, and what was blocked.
3. **The person's choice of resolver, and of lists.** EON adds no resolver of its own
   until madde 30 allows running one.
4. **Nothing leaves the device but the lookups themselves.** No telemetry (madde 36). The
   log stays local and is off by default.
5. **Always recoverable.** Turning it off, quitting or a crash must never leave the
   computer without DNS.
6. **Then the tunnel.** The tunnel comes with a kill switch, which the 1.1.1.1 app lacks.

## Sources

- Cloudflare, [WARP modes](https://developers.cloudflare.com/warp-client/warp-modes/) and
  [MASQUE now powers 1.1.1.1 and WARP apps](https://blog.cloudflare.com/masque-now-powers-1-1-1-1-and-warp-apps-dex-available-with-remote-captures/).
- The 1.1.1.1 app's [App Store listing](https://apps.apple.com/app/1423538627) and its
  release notes.
- TechRadar, [Cloudflare DNS (1.1.1.1) review](https://www.techradar.com/reviews/cloudflare-dns),
  and user reviews of the app. These are self-selected, so they are treated as signals,
  not measurements.
- Hickory DNS: [hickory-resolver 0.26](https://docs.rs/crate/hickory-resolver/latest),
  [hickory-server 0.26](https://docs.rs/crate/hickory-server/latest).
