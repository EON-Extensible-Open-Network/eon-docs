# eon-docs

The **project plan**, the versioned policies, and the guides.

[![Status](https://img.shields.io/badge/status-alpha%20·%20active-f59e0b)](plan/eon-plan.md)
[![License](https://img.shields.io/badge/license-CC--BY--SA--4.0-3b82f6)](LICENSE)

---

## The plan is the source of truth

[`plan/eon-plan.md`](plan/eon-plan.md) holds every architectural, policy and legal
decision the project has made.

How it works:

- **Numbers are permanent identities, not positions.** `madde 22` means the same decision
  forever. Content is rewritten; the number never changes. Code comments, pull requests and
  documentation cite these numbers, so a renumbering would silently invalidate references
  across every repository.
- **Every item carries a status:** `[K]` decided · `[A]` to be researched · `[E]` deferred.
  An `[A]` item is a directed question, not a decision — reading it as settled is the mistake
  the labels exist to prevent.
- **A decision changes through a pull request**, not a chat thread: `madde N: <what changes>`,
  stating the current decision, the proposed one, and **what it costs**. A proposal with no
  stated downside has not been thought through (see `GOVERNANCE.md`).
- **Code that contradicts the plan is a bug** — in one of the two.

The plan also carries a non-goals list (madde 41) and a rejected-alternatives table.
Both exist so the same arguments are not relitigated every six months.

## Contents

| Path | What |
|---|---|
| `plan/eon-plan.md` | The numbered decisions. Start here. |
| `docs/architecture.md` | How EON is put together, in English: one product per service (madde 45), the shape every product follows, the rules they all keep. |
| `office/` | EON Office: roadmap, Microsoft parity audit, design notes, status (madde 43). |
| `download-manager/` | EON Download Manager: research, plan, status (madde 44). |
| `policies/` | Terms, privacy, content policy, moderation and notice process, Edu institution agreement — each semver'd with an effective date (madde 11). Not written yet. |
| `docs/` (guides) | User guides, institution deployment guides, per-role guides (madde 17) — to come. |

## The products

EON is an umbrella; every service under it is its own product with its own repositories,
core and application (madde 45): **EON Stream** (video) and **EON Edu** (schools, on Stream's
core), **EON Download Manager** (files), **EON Office** (productivity), and an audio product
still to be named.

## Policies are versioned, in the open

Every policy carries a version and an effective date, and lives here so that changes appear as
diffs. A policy that can be changed quietly is not a policy (madde 11).

Rules that apply to every policy:

- Which version a user accepted, and when, is recorded.
- Significant changes are announced at least 30 days before taking effect.
- Changes to personal data handling require **fresh explicit consent**; silence is not
  acceptance.
- Each policy states what happens if a user declines. Declining must never brick the local
  viewer (madde 7).
- Policies are not copied from commercial platforms. Openly licensed texts are adapted;
  structure is borrowed, wording is not.

**Status:** the policies are not written yet. They are a precondition for opening the
marketplace, along with a legal review and a legal entity (madde 13, 30). Nothing here is legal
advice.

## Language

The plan is maintained in **Turkish** — it is the working language of the decisions and
precision matters more than reach. Code, contracts and public documentation are in English.
User-facing documentation is Turkish and English (madde 40).

## License

[CC-BY-SA-4.0](LICENSE) — the same licence recommended as the default for material packages
(madde 16e). Adapt and share, under the same terms, with attribution.
