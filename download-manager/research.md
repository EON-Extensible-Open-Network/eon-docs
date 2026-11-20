<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Download Manager — research and analysis

**20 November 2026.** Step one and two of four: what people value in Internet Download
Manager (IDM), how IDM does those things, what is wrong with it and with the free
alternatives, and what follows for EON Download Manager. The plan is
[plan.md](plan.md); the decision is plan madde 44.

Sources are listed at the end. Vendor speed figures ("up to 8×") are marketing and are
treated as such; where a source is a listicle, it is said.

## 1. Why people use IDM

What users and reviewers keep returning to, roughly in the order it is mentioned:

1. **It finishes.** Downloads survive dropped connections, sleep, reboots and power cuts
   and continue from where they stopped. Browsers restart many failed downloads from zero.
   This is the single most repeated reason, ahead of speed.
2. **It is fast on bad links.** Several connections per file; on throttled or unstable
   lines the difference is large. On a good line to a fast server it is small — the gain
   depends on the server not capping per IP.
3. **It catches downloads by itself.** Click a link in the browser and IDM takes over
   with no copy-paste. The "Download this video" button over web players is, for many
   people, *the* feature.
4. **It stays out of the way.** Categories put files in sensible folders; a queue and a
   scheduler run things at night and can switch the computer off afterwards.
5. **Batch and grab.** Download every link on a page, a numbered series, or a site's files
   by filter.

## 2. How IDM does it

From IDM's own documentation (features, segmentation, scheduler, queues, options, video
and grabber help pages):

| Thing people love | Mechanism |
|---|---|
| Speed | **Dynamic segmentation.** The file is not cut into fixed parts up front. Whenever a connection is free, IDM takes the *largest remaining segment and splits it in half*; the new connection starts at the midpoint. A connection that finishes early helps the slowest one the same way. Segments too small for the connection type are not split (threshold not published). |
| Speed | **Connection reuse.** A connection that finished its part is reused for the next one without a new connect/login. |
| Speed, tuning | Connection count per download, timeouts, and a per-server exception table (servers that allow 1–2 connections). Per-queue "one file at a time" for strict sites. |
| Finishing | **File positions saved several times a minute**; after a crash the file is reassembled correctly. Option to ignore modification-time changes on resume, because some servers change it on every request. |
| Finishing | Queue retries: a failed file goes to the back of the queue; alone, it is retried every 30 s, optionally capped. |
| Catching downloads | A browser extension ("IDM integration module") plus an "advanced" OS-level integration. Takes over only types on a file-type list; address exception list with wildcards; **Ctrl** forces takeover, **Alt** lets the browser keep it. Context menu: download link, download all links in a selection. |
| Catching downloads | **Clipboard monitoring** for URLs with listed extensions, confirmed with a dialog. |
| The video button | A panel over web players lists the available formats/qualities; capture starts from the beginning even if pressed mid-play. No playlists, no "all videos on a page". Unsupported and protected protocols are simply not offered. |
| Staying out of the way | **Categories** by file type with default folders; temporary folder for parts; a "Download File Info" dialog (name, folder, category, *Download later* / *Start*) that can start downloading while it is still open; silent mode. Antivirus run on completion. |
| Night runs | **Queues** (download and *synchronization* queues that re-fetch changed files), start/stop times, files at a time, **after-queue actions**: hang up, shut down, run a file. A *quota* ("150 MB every 4 h") — not a speed limit. |
| Batch | Batch URLs with wildcards; batch from clipboard; **site grabber** with a four-step wizard and filters, savable and schedulable projects. |
| Odds and ends | Drag-and-drop "drop target", command line (`/d URL /p /f /n /q /a /s`), proxy/SOCKS/PAC, site logins, NTLM/Kerberos, 30 languages, skins. |

## 3. What is wrong

**IDM:** Windows only; paid with a 30-day trial, and the most-searched IDM topic is the
"fake serial number" pop-up that interrupts downloads — including for people who paid.
The interface is a 2000s Win32 grid with skinnable toolbars. No speed limiter (only
quotas). No checksum verification. Closed source, so "no spyware" is a promise, not
something anyone can check.

**Free Download Manager:** the default free recommendation and cross-platform; freemium
and proprietary in parts. **JDownloader:** powerful for file hosters, Java, steep
learning curve. **Motrix / XDM:** aria2-based or Java, maintenance is uncertain (XDM's last
release page is years old). **AB Download Manager:** the newest open-source IDM-alike;
liked for being free, open and having queues and a scheduler; criticised for no automatic
video detection, higher resource use in one hands-on test, and weak *continue a failed
download with a new link*.

Nobody combines IDM's behaviour with an interface people would call beautiful, for free,
open source, on all three systems.

## 4. Analysis: what EON Download Manager must be

**Non-negotiable, because it is why people pay for IDM:**

- Dynamic segmentation with connection reuse, as IDM describes it.
- Resume that survives anything: segment positions persisted at least once a second with
  atomic writes; validators (`ETag`, `Last-Modified`, length) checked on resume, with
  IDM's "ignore modification time" escape hatch.
- **Refresh address:** give a failed or expired download a new link (from the browser or
  pasted) and keep every byte already fetched, as long as the length matches. The one
  thing AB Download Manager users miss from IDM.
- Browser takeover by file type and size, Ctrl/Alt overrides, an exception list,
  "Download with EON", "Download all links", cookies and referrer carried over.
- Video: detect what the page itself loads (HLS playlists, direct media files) and offer
  it from a button. **No site-specific extractors and nothing protected** — the same line
  IDM draws for "legal reasons", and EON's own madde 9 spirit: we ship a tool, not a
  content source.
- Clipboard watching, categories with folders, queue, scheduler, shut-down-when-done.

**Where we beat it:**

- **Free and open** for individuals, forever, with no nag of any kind.
- **A real speed limiter** (global and per download) — IDM only has quotas.
- **Checksums:** SHA-256 shown on completion and compared with one you paste.
- **The interface.** The download *is* the interface: the segment map IDM draws as pink
  and blue lines becomes the centre of the window — every connection a lane you can watch
  split and finish.
- Windows, Linux and macOS from the same code.
- Small and quick: Rust and the system webview, not Java or Electron.

**Deliberately not in the first alpha:** site grabber, synchronization queues, DASH,
FTP, BitTorrent (EON Stream's engine does torrents), quotas, dial-up, skins. Each is a
later alpha, in that rough order.

## Sources

- IDM official: [features](https://www.internetdownloadmanager.com/features.html),
  [dynamic segmentation](https://www.internetdownloadmanager.com/support/segmentation.html),
  [scheduler](https://www.internetdownloadmanager.com/support/idm-scheduler/idm_scheduler.html),
  [queues](https://www.internetdownloadmanager.com/support/idm-scheduler/idm_queues.html),
  [options](https://www.internetdownloadmanager.com/support/using_idm/options.html),
  [starting downloads and command line](https://www.internetdownloadmanager.com/support/using_idm/starting.html),
  [streaming video](https://www.internetdownloadmanager.com/register/new_faq/video5.html),
  [grabber](https://www.internetdownloadmanager.com/support/idm-grabber/idm_grabber.html),
  [fake serial FAQ](https://www.internetdownloadmanager.com/register/new_faq/register7.html).
- User voices: [Tom's Guide forum](https://forums.tomsguide.com/threads/internet-download-manager-starts-downloading-before-clicking-download-button.337667/latest),
  [AlternativeTo: IDM](https://alternativeto.net/software/internet-download-manager/about),
  [Mozilla support: fake serial](https://support.mozilla.org/questions/937657),
  [AB Download Manager add-on reviews](https://addons.mozilla.org/en-US/firefox/addon/ab-download-manager/reviews/),
  [AB Download Manager hands-on test](https://digitalkarthik.com/ab-download-manager-review/).
- Comparisons (listicles, weak evidence): [itechguides](https://www.itechguides.com/top-5-download-managers-to-boost-download-speeds-which-one-is-best-for-you/),
  [unstore](https://unstore.io/discover/best-internet-download-manager-alternatives-desktop/),
  [techlasi](https://techlasi.com/savvy/best-download-managers/).
- Browser handoff pattern: [chrome.downloads](https://developer.chrome.com/docs/extensions/reference/api/downloads),
  [Mozilla bug 1245652](https://bugzilla.mozilla.org/show_bug.cgi?id=1245652).
