# Navo App — Planning model

**Status:** Approved baseline (template default — replace with your project's approval date once reviewed).

> Authoritative source for Navo App's milestones, work packages, task-file conventions, readiness-report conventions, and the relationship between versions and milestone closures.
> For Git, branching, commits, merges, tagging, releases, and changelog conventions see [`02-git-workflow.md`](02-git-workflow.md).

---

## 1. Purpose and audience

This document tells humans and AI agents how Navo App's work is organized, planned, named, and closed: milestones, work packages, status discipline, filesystem layout, and the relationship between versions and milestone closures.

If you are an AI agent, follow these rules verbatim. Do not improvise. If a rule conflicts with an instruction, surface the conflict and ask before deviating.

---

## 2. Historical exceptions (if applicable)

> Most projects do not need this section. Delete it unless the project carries a closed legacy planning model that future maintainers need to recognize.
>
> If kept, the pattern is: name the closed model, point at the frozen artifacts, and state explicitly that future work follows the model defined below.

---

## 3. Milestones and work packages

The planning unit is the **milestone**, with **work packages** inside.

| Concept | Definition |
|---|---|
| **Milestone** | A release-aligned scope of work. Lives at `context/tasks/<NN>-<milestone-slug>/` and `context/sanity-checks/<NN>-<milestone-slug>/`. Closes at a tagged version when its scope is complete. |
| **Work package** | A named, locally-numbered unit of work inside a milestone. Has its own task file and readiness report. Closes via its readiness report. |

Milestones group related work that ships together. Work packages can be sequential, parallel, or alternative within a milestone.

---

## 4. Milestone naming and numbering

- **Pattern:** `NN-<kebab-case-slug>/` where `NN` is a two-digit number reflecting overall project progression (e.g., `01-foundation/`).
- **Default start: `01`.** Most projects begin at `01`.
- **Reserve `00-` for explicit bootstrap / foundation phases** that exist before product work begins (e.g., `00-bootstrap`, `00-foundation-and-conventions`). Use `00-` only when the phase is genuinely pre-product; otherwise start at `01-`.
- **Display name:** human-readable (e.g., "Foundation").
- The number and slug should be stable for the milestone's lifetime; do not rename or renumber mid-milestone.
- Used as a folder name and as a scope in commit messages (e.g., `feat(01-foundation): ...`).

The milestone number means: **overall project progression order at planning time**. It does NOT mean strict dependency, priority, or closure order — work between milestones can overlap when the planning conversation allows it.

---

## 5. Work-package naming and numbering

- **Pattern:** `NN-<kebab-case-slug>.md` (e.g., `01-bootstrap-conventions.md`).
- **Default start: `01`.** Each milestone restarts at `01`; no global numbering across milestones.
- **Reserve `00-` for explicit bootstrap WPs** (e.g., a governance / conventions WP that precedes substantive work). Use only when warranted; otherwise start at `01-`.
- The slug describes the work, not the order — the leading number does that.

---

## 6. Task numbering

- **Pattern:** `T0..TN`, used inside a work package's task file when the WP is large enough that explicit tasks help.
- **`T0` is reserved** for setup / scaffolding / pre-flight tasks within a WP. Substantive task work starts at `T1`.
- Smaller WPs may omit explicit `Tx` tasks entirely and rely on scope + acceptance criteria. See [`../context/tasks/README.md`](../context/tasks/README.md) for guidance.

---

## 7. Local numbering semantics (work packages and tasks)

The leading number on a work package or task means: **recommended execution order at planning time**.

What it does NOT mean:

- Strict dependency order ("02 must close before 03 starts"). Items can be parallel, skipped, or reordered at planning revisions.
- Priority ("01 is more important than 03").
- Closure order ("01 must close before 03 closes"). They can close in any order.

What it DOES mean:

- A reading order that filesystem listings and tab completion will surface.
- A presumed order at planning time, useful as a default if no other dependency information exists.

The **milestone README is the authoritative source** for current order, status, dependencies, and parallelism inside a milestone. Filenames mirror the README's planned order; they are not load-bearing on their own. If the README and the filenames disagree, the README wins.

---

## 8. Append-only insertion rule

When a new milestone, work package, or task is identified mid-flight:

- **Add it at the next free number.** If `01`, `02`, `03` exist, the new one is `04`, regardless of where it logically fits.
- **Do not insert** with `02b-...`, decimal numbers, or by renumbering existing items.
- **Use the parent README** to capture dependency notes if the new item logically belongs "between" existing ones.

Renumbering existing items is allowed only at a deliberate planning-conversation revision, and triggers the coupling rule in §11.

---

## 9. Status discipline

Work packages:

| Status | Meaning |
|---|---|
| `roadmap (not frozen)` | Pre-planning roadmap outline. Not a frozen task file; no acceptance criteria authored yet; not executable. Used to capture forward-looking shape for cold-start discoverability. |
| `placeholder` | Roadmap stub for a candidate work package in a not-yet-opened milestone. Lighter than `roadmap` — milestone itself is `placeholder`. Not executable. |
| `planned` | Task file frozen at planning conversation; execution not yet started. |
| `active` | Execution in progress. |
| `closed` | Readiness report signed off; work package is done. |

Milestones:

| Status | Meaning |
|---|---|
| `placeholder` | Milestone not yet opened. Captures forward-looking structure (typically a future-milestone README plus its candidate work-package files at `placeholder` status). No work executes under a placeholder milestone. |
| `open` | Milestone is in scope; new work packages may be added. |
| `closed` | Milestone scope is complete. No further work packages. Marked by a tagged version. |

Status changes are recorded in the milestone README and in the work-package task file's frontmatter.

### 9.1 Closure metadata pattern

A work package's `Closed:` date, sign-off line, and merge-commit SHA cannot be known until the closing PR is merged. The convention is a **two-PR flow**:

1. **Closing PR** — ships the work plus the readiness report with explicit placeholders (`Status: closed PASS — pending PR sign-off`, `Closed: (filled in at PR sign-off)`, `Sign-off: product owner`). Reviewed and merged.
2. **Sign-off patch PR** — single-commit follow-up (`docs/`-prefixed branch; subject `docs(planning): sign off <milestone>/<NN-slug> closure`) that fills the placeholders against the closing PR's actual merge commit, sets task-file `Status: closed`, adds the `Closed:` date, backfills `Frozen:` if not already present, and updates the milestone README work-packages table.

If a version tag is carried by this work-package closure, tag **the sign-off patch's merge commit** so the tagged state is self-consistent (the readiness report visible at the tag does not say "pending sign-off").

The sign-off patch is the act of sign-off. After it merges, the readiness report is **frozen** per [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md).

Under the conditional milestone-branch policy in [`02-git-workflow.md` §14](02-git-workflow.md), per-WP closures are commits on the milestone branch (no per-WP PR). The closure-metadata placeholders are filled by a follow-up commit on the milestone branch instead of a separate sign-off patch PR. Only the milestone-close PR merges back to `main`.

---

## 10. Filesystem layout

```
context/
  tasks/
    README.md                                         # task-file template
    <NN>-<milestone-slug>/
      README.md                                       # milestone index (status, deps, links)
      01-<work-package-slug>.md
      02-<work-package-slug>.md
      ...
  sanity-checks/
    README.md                                         # readiness-report template
    <NN>-<milestone-slug>/
      01-<work-package-slug>/
        readiness-report.md
        # plus per-package evidence: smoke logs, result files, etc.
      02-<work-package-slug>/
        readiness-report.md
      ...
```

---

## 11. Coupling rule: rename task → rename readiness folder

Renaming or renumbering a work-package task file requires renaming the matching readiness-report folder **in the same PR**. The two paths must remain symmetric.

Example: renaming `context/tasks/01-foundation/03-storage-baseline.md` → `04-storage-baseline.md` requires renaming `context/sanity-checks/01-foundation/03-storage-baseline/` → `04-storage-baseline/` in the same PR.

---

## 12. Milestone README template

Required sections:

- **Header** — number, slug, display name, status (`open` / `closed`), started date, target close (a future tagged version).
- **Purpose** — 1–3 paragraphs. What this milestone groups, why it exists, what it follows.
- **Relationship to versions** — explicit per-milestone notes if any version tag has special meaning during the milestone (e.g., a baseline tag taken mid-milestone).
- **Work packages** — a status table with columns `#`, `Slug`, `Status`, `Closes at` (which version tag, if known), `Task file` (link), `Readiness report` (link or pending).
- **Notes** (optional) — dependency notes, parallelism notes, carry-forwards from prior milestones, candidate future work packages.

The README is updated whenever a work package is added, status changes, or the planning conversation revises the order.

A scaffold milestone README lives at [`../context/tasks/_milestone-template/README.md`](../context/tasks/_milestone-template/README.md).

---

## 13. Discovery map — where to find what

| Question | Source of truth |
|---|---|
| How do I name/use branches, commits, tags, merges? | [`02-git-workflow.md`](02-git-workflow.md) |
| What goes in `CHANGELOG.md` vs GitHub Release vs readiness reports? | [`02-git-workflow.md` §8](02-git-workflow.md) |
| What is a milestone? When do I propose one? How do I name it? | this document, §3–§4 |
| What is a work package? How do I number it? | this document, §5, §7, §8 |
| What is a task? How do I number it? | this document, §6, §7, §8 |
| What goes in a milestone README? | this document, §12 |
| What goes in a task file? | [`../context/tasks/README.md`](../context/tasks/README.md) |
| What goes in a readiness report? | [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) |
| Which doc is source of truth for X? | [`../CLAUDE.md`](../CLAUDE.md) (dispatcher) → this map |
| What changed in `v<X.Y.Z>`? | [`../CHANGELOG.md`](../CHANGELOG.md) |
| What's the per-package evidence for a closure? | the readiness report itself |
| How do I cleanly close a mid-WP planning session (post-freeze handoff)? | [`../chat/prompts/execution/planning-session-close.md`](../chat/prompts/execution/planning-session-close.md) |
| How do I cleanly close a mid-WP implementation session (partial completion)? | [`../chat/prompts/execution/implementation-session-close.md`](../chat/prompts/execution/implementation-session-close.md) |
| How do I close a completed work package? | [`../chat/prompts/execution/work-package-close.md`](../chat/prompts/execution/work-package-close.md) |
| What's the binding architectural decision behind X? | [`decisions/`](decisions/) (ADRs) |
| What's the project context / purpose? | [`05-product-context.md`](05-product-context.md) |
| What's the capability outline? | [`06-feature-blueprint.md`](06-feature-blueprint.md) |
| What's the bootstrap-time stack and initial architecture direction? | [`07-technical-direction.md`](07-technical-direction.md) |
| What's the current living architecture doctrine? | [`09-architecture.md`](09-architecture.md) (living counterpart to `07`) |
| What's the current living testing-strategy doctrine? | [`10-testing-strategy.md`](10-testing-strategy.md) |
| What's the current living engineering-practices doctrine? | [`11-engineering-practices.md`](11-engineering-practices.md) |
| How does engineering doctrine evolve (proposing, accepting, superseding)? | [`09-architecture.md` §4](09-architecture.md) (canonical) · restated in `10` §4 and `11` §4 |
| What's the active phase scope baseline? | [`08-scope.md`](08-scope.md) (renamed per phase) |
| Where do operational commands and environments live? | [`04-runbook.md`](04-runbook.md) |
| Are there ideation / source documents? | [`../context/product-source/`](../context/product-source/) — not authority |
| Is there research / synthesis? | [`research/`](research/) — when used; immutable once cited by a synthesis |
| When does a milestone branch merge to `main`, if used? | [`02-git-workflow.md` §14](02-git-workflow.md) — conditional policy; merges at milestone completion only |
| Where does design-track work live? | [`../context/design/`](../context/design/) — optional; activate when needed |

> Add project-specific rows as scope baselines or specialty docs are added.

---

## 14. ADR conventions

Architecture Decision Records live at `docs/decisions/<NNNN>-<slug>.md`. They follow a Proposed → Accepted → Superseded lifecycle. The template lives at [`decisions/0000-template.md`](decisions/0000-template.md).

Every ADR carries a `Scope:` field at the top. Common values:

- `foundation` — engine choice, storage class, sticky decisions that constrain all later layers.
- `<phase> implementation` — phase-specific implementation choice; the `<phase>` is the project's own phase name.
- `tooling` — CI, build, dev environment.

Each ADR documents its `Open questions before acceptance` (if any), the decision itself, and the consequences (positive / negative / neutral). See the template for the full structure.

---

## 15. What closes a milestone — versions and milestones are decoupled

A milestone closes when its scope is complete. The close is **marked** by a tagged version (e.g., `v0.1.0` for the first milestone); it is not **caused** by tagging.

**Versions and milestones are decoupled:**

- Tagging a version does not automatically close any milestone. Status changes happen by deliberate planning revision.
- A milestone may have **work packages closing at different version tags**. A small bootstrap WP may close at `v0.1.0`, while later WPs close at `v0.1.1`, `v0.1.2`, … or at the milestone's main close tag.
- **Intermediate version tags within an open milestone are allowed** and snapshot progress without implying closure.
- A milestone does **not** require its own readiness report. Per-work-package readiness reports plus the CHANGELOG entry at the closing tag are sufficient evidence.

When closing a milestone:

1. Verify all in-scope work packages are `closed` per their readiness reports.
2. Update the milestone README header status to `closed` and record the closing tag.
3. Tag the closing version (per [`02-git-workflow.md` §7](02-git-workflow.md)).
4. Add a CHANGELOG entry summarizing the milestone scope.

---

## 16. Sister documents

- [`02-git-workflow.md`](02-git-workflow.md) — Git mechanics that this document does not cover.
- [`../context/tasks/README.md`](../context/tasks/README.md) — task-file template.
- [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) — readiness-report template.
- [`../CLAUDE.md`](../CLAUDE.md) — dispatcher; lists this document under "Authoritative documents".
