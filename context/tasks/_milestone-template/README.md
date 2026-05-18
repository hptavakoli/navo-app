# Milestone: {{FIRST_MILESTONE_DISPLAY_NAME}}

> **Number:** `NN` (e.g., `01` for product/feature milestones; `00` reserved for explicit bootstrap/foundation phases)
> **Slug:** `{{FIRST_MILESTONE_SLUG}}`
> **Folder:** `NN-{{FIRST_MILESTONE_SLUG}}/`
> **Status:** placeholder | open | closed
> **Started:** YYYY-MM-DD (set when milestone moves to `open`)
> **Target close:** which future tagged version is expected to mark milestone close (informational)

## Purpose

1–3 paragraphs: what this milestone groups, why it exists, what it follows or precedes.

> Example skeleton (delete or replace): "Establish the [foundation / governance / first usable surface] required before [next phase] can begin. This milestone follows [prior milestone or bootstrap context] and precedes [next milestone]."

## Relationship to versions

> Project default: versions and milestones are decoupled per [`../../../docs/03-planning-model.md` §15](../../../docs/03-planning-model.md). Milestone close is *marked* by a tagged version, not *caused* by tagging. Note any version-vs-milestone relationships specific to this milestone here.

## Work packages

| # | Slug | Status | Closes at | Task file | Readiness report |
|---|---|---|---|---|---|
| 01 | `<work-package-slug>` | roadmap (not frozen) | future tag | [01-<work-package-slug>.md](01-<work-package-slug>.md) | — |
| 02 | `<work-package-slug>` | roadmap (not frozen) | future tag | [02-<work-package-slug>.md](02-<work-package-slug>.md) | — |

Local numbering reflects **recommended execution order** at roadmap recording time, per [`../../../docs/03-planning-model.md` §7](../../../docs/03-planning-model.md). Order may change at planning time; **this README remains the source of truth for current status, dependencies, and parallelism notes.**

## Roadmap shape (recorded YYYY-MM-DD)

> When work packages are outlined at roadmap level (status `roadmap (not frozen)`), describe each one in 2–4 sentences here. State the Git operating mode (default per-WP-PR or the conditional milestone-branch policy at [`../../../docs/02-git-workflow.md` §14](../../../docs/02-git-workflow.md)).

## Notes

- Add dependency notes, parallelism notes, carry-forwards from prior milestones, or candidate future work packages here.
- This README follows the milestone-README template at [`../../../docs/03-planning-model.md` §12](../../../docs/03-planning-model.md).

---

## How to use this template

`_milestone-template/` is a **reusable scaffold**. Do NOT rename, move, or delete it — it stays in place so every new milestone can start from the same starting point.

1. Create a sibling folder `context/tasks/<NN>-<slug>/` next to `_milestone-template/`, where:
   - `<NN>` is `01` for product/feature work, or `00` for an explicit bootstrap/foundation phase.
   - `<slug>` is the kebab-case milestone slug.
   - Example: `01-mvp/` or `00-bootstrap/`.
2. Copy this README into the new folder as `context/tasks/<NN>-<slug>/README.md`. Leave `_milestone-template/README.md` unchanged.
3. Fill in the placeholders in the new copy (`Number`, `Slug`, `Folder`, etc.).
4. Author the work-package task files following [`../README.md`](../README.md). Work packages restart at `01-` per milestone.
5. Author the matching readiness-report folders under `context/sanity-checks/<NN>-<slug>/<NN-WP>-<work-package-slug>/` when work closes.

The leading underscore in `_milestone-template` keeps the template directory sorted to the top of filesystem listings and signals "not a real milestone."

## Numbering reference

- **Milestone number** (`NN-<slug>/`): default start `01`; `00-` reserved for bootstrap/foundation. See [`../../../docs/03-planning-model.md` §4](../../../docs/03-planning-model.md).
- **Work-package number** (`NN-<slug>.md` inside this folder): default start `01`; `00-` reserved for bootstrap WPs if needed. See [`../../../docs/03-planning-model.md` §5](../../../docs/03-planning-model.md).
- **Task ID** (`Tx` inside a work-package task file): `T0` reserved for setup; substantive tasks start at `T1`. See [`../../../docs/03-planning-model.md` §6](../../../docs/03-planning-model.md).
- **Append-only**: never renumber existing items; new items take the next free number. See [`../../../docs/03-planning-model.md` §8](../../../docs/03-planning-model.md).
