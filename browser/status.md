<!--
SPDX-License-Identifier: CC-BY-SA-4.0
SPDX-FileCopyrightText: 2026 EON contributors
-->

# EON Browser — status

**As of 26 November 2026.** Plan madde 50; [research](research.md), [plan](plan.md).

| Repository | Version | CI | Notes |
|---|---|---|---|
| `eon-browser-core` | in progress toward `v0.0.0` | — | blocking, link cleaning, HTTPS upgrade, address bar, search engines, profile storage, user-agent policy |
| `eon-browser-app` | not opened yet (43d: a repository opens with a working first version) | — | Servo 0.8.0 embedding proven by an offscreen probe on Windows |

## Measured so far

- `servo 0.8.0` builds on Windows (MSVC, no clang) and renders Wikipedia, Google search,
  GitHub and Reddit correctly offscreen; its request hook blocks trackers. Details in
  [research.md](research.md).
- YouTube failed during a network reset on the test connection; to be retested.

## Open

- How the interface is drawn: HTML in a privileged Servo view, or egui (madde 50, [A]).
- Compatibility of sign-in, forms, video and long-lived web apps (madde 50j).
