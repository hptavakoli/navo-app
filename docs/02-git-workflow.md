# Navo App — Git workflow

**Status:** Approved baseline (template default — replace with your project's approval date once reviewed).

> Authoritative source for Navo App's Git, branching, commit, merge, tagging, and release conventions.
> For planning, milestones, and work-package conventions see [`03-planning-model.md`](03-planning-model.md).

---

## 1. Purpose and audience

This document tells humans and AI agents how to make changes to the Navo App repository: branches, commits, merges, versions, releases, and the changelog. It does **not** cover what to work on or how to structure that work — that belongs in [`03-planning-model.md`](03-planning-model.md).

If you are an AI agent, follow these rules verbatim. Do not improvise. If a rule conflicts with an instruction, surface the conflict and ask before deviating.

### 1.1 Operation ownership

Git operations on this repository follow an **approval-gated, bounded-operation execution model**. The user approves a *bounded operation* — a named unit of work with explicit scope, an explicit endpoint, and inherited §2–§14 conventions. After approval, the AI agent executes the routine mechanical sequence end-to-end without per-step approval; at the endpoint, the agent returns a single completion report.

**Outline before approval.** Before the AI agent executes anything, it provides a concise outline of the intended sequence (scope + endpoint + any important names not obvious from context). The outline is always required, even if a single sentence suffices for trivial operations.

**Operation tiers.**

| Tier | Examples | Treatment |
|---|---|---|
| Read-only inspection | `git status`, `git log`, `git diff`, `git show`, `gh pr view`, `gh api` GET | Always allowed; no approval required. |
| Inside-scope (bundled inside one approval) | branch create, stage, commit, push topic / milestone branch, open / update PR, merge via merge commit, annotated tag, push tag, publish Release, delete merged topic branch, post-merge dispatcher / CHANGELOG sync on `main` when declared | Routine inside an approved bounded operation. No per-step approval. |
| Pause-and-confirm (separate decision required) | squash / rebase merge, force push, tag delete / re-tag, Release delete, branch delete of `main` / release / active milestone branch, history rewrites on shared branches, branch protection / repo settings / GitHub Actions secrets changes, skipping commit hooks, spike-branch promotion strategy, hotfix patch-version bump, milestone-branch absorption during hotfix, any action outside named scope | AI agent pauses, asks for the specific decision, continues from where it paused after explicit resolution. The user may pre-authorize any of these by listing them explicitly in the outline. |

**Partial failure.** If execution hits a conflict, unexpected state, or any interruption (merge conflict, tag collision, push rejection, missing remote), the AI agent stops, reports the partial state, and waits for the specific decision. No auto-rollback, no retry-with-different-parameters, no improvised completion.

**Approval discipline.**

- One approval = one bounded operation. When the endpoint is reached, the approval is consumed. Subsequent operations require new approvals.
- Approvals are session-local. A new session does not inherit a partial approval; a bounded operation spanning sessions requires re-approval, with the new session's state confirmed against the partial-completion state.
- An approval names a bounded operation and accepts its outline. Generic "ok" / "go ahead" / "looks good" without a named operation does not count; the AI agent restates the operation explicitly and waits for confirmation.

**§1.2 independence.** §1.2 (no AI attribution in GitHub-facing text) is independent of operation ownership. An approval for a bounded operation does NOT authorize AI attribution. §1.2 remains a hard prohibition under either track.

**Foundation-level changes to this rule require an ADR amendment, not a one-off note.**

### 1.2 AI-attribution policy (GitHub-facing output)

AI attribution must NOT appear in any GitHub-facing or repo-history-facing text **unless the user explicitly requests it for that specific output**.

This includes, but is not limited to:

- commit messages and trailers (`Co-authored-by`, `Generated with`, `Authored-by: Claude`, etc.)
- PR titles and PR bodies
- annotated tag messages
- GitHub Release notes
- `CHANGELOG.md` entries
- GitHub issue and PR comments
- any other text that lands in repo history or appears on a public-facing GitHub surface

This applies whether the text was drafted by an AI or by a human — the rule is about what appears in the output, not who composed it.

It is fine for **internal documentation** (e.g., this repo's `CLAUDE.md`, `chat/resources/*`, working rules) to acknowledge that the workflow is AI-assisted — that is project-level information, not GitHub-facing output.

The rule covers all common AI-attribution patterns:

- `Co-authored-by: Claude <noreply@anthropic.com>` — forbidden
- `🤖 Generated with [Claude Code](...)` — forbidden
- `Created with ChatGPT` — forbidden
- "Authored with help from …" footers — forbidden
- Any equivalent footer for any other AI tool — forbidden

**Exception process:** the user may request AI attribution on a specific output (e.g., a public blog post about the workflow). That authorization applies to that specific output only and does not extend forward.

---

## 2. Branching model

Navo App uses **GitHub Flow + version tags**.

- `main` is stable and always shippable. All real work flows through PRs into `main`.
- Short-lived branches are created from `main` for each unit of work, merged back via PR, then deleted.
- Releases are marked by **annotated tags** on `main` commits.
- No `develop`, `production`, or other long-lived branches. They will be added only if a concrete need arises.

---

## 3. Branch prefixes and naming

Branch name pattern: `<prefix>/<short-kebab-slug>`.

Allowed prefixes:

| Prefix | Use when |
|---|---|
| `feat/` | adding or extending product runtime behavior |
| `fix/` | fixing a bug in product runtime behavior |
| `docs/` | documentation-only changes (no code, no behavior change) |
| `chore/` | tooling, configuration, or dependency maintenance with no behavior change |
| `refactor/` | restructuring code without behavior change |
| `spike/` | time-boxed experiment (see §11) |
| `release/` | release preparation work — only when needed |
| `hotfix/` | patching a released/tagged state, branched from the relevant tag (see §10) |
| `milestone/` | active milestone branch under the conditional policy in §14 — only when policy conditions hold |

Slug guidance:

- Short, kebab-case, descriptive without external context: `grounded-generation`, not `gg`.
- Do not encode milestone names in branches *(applies to non-milestone prefixes; the `milestone/` prefix is the exception covered in §14)*; the task file path records that.
- If the same slug genuinely collides across milestones, prefix with the version: `feat/v0.3-grounded-generation`.

---

## 4. Commit messages

Navo App follows **[Conventional Commits](https://www.conventionalcommits.org/)**.

Subject format: `<type>(<scope>): <subject>` (scope optional).

Allowed types:

| Type | Use when |
|---|---|
| `feat` | adding or extending product runtime behavior |
| `fix` | fixing a bug |
| `docs` | documentation-only changes |
| `chore` | tooling / config / dependency maintenance |
| `refactor` | restructuring without behavior change |
| `test` | adding or modifying tests |
| `perf` | performance-only changes |
| `build` | build-system or dependency-version changes |
| `ci` | CI-only changes |
| `style` | formatting-only changes |

Useful scopes are project-defined — typical examples include a runtime package name, an app or service name, a milestone slug, `planning`, `claude`, `adr`.

Examples:

- `feat(api): add streaming response on /ask`
- `docs(planning): add 01-<milestone-slug>/02-<work-package-slug> task file`
- `fix(cli): require --mode flag on eval to prevent accidental hosted runs`
- `chore: bump <dependency> pin to <version>`

The body is optional but encouraged for non-trivial changes. Reference issues / PRs / ADRs / readiness reports as relevant.

### 4.1 Presentation format for AI-drafted commit messages

When an AI agent drafts a commit message for the user to paste into a Git client (e.g., VS Code's commit message box, or `git commit -F -`), it MUST present the full message as a single copy-paste-ready block — subject line, blank line, then body — inside one fenced code block. Do NOT split into separate "Subject:" and "Body:" sections.

The blank line between subject and body is the only signal Git uses to parse them; presenting both as one block lets the user paste once into a single-field UI without re-assembly.

This rule is commit-specific. PR titles and bodies are presented separately — they go into separate GitHub fields, so a single block would not help there.

---

## 5. Commits as logical work units

**Do not commit per task.** Task-level traceability lives in task files, readiness reports, the PR body, and the readiness report's per-task outcomes section — not in the commit history.

A commit represents a **coherent diff that stands on its own** — a checkpoint where the code (or docs) is internally consistent. Useful checkpoints include:

- "scaffolding present, before substantive logic"
- "core logic in place, before evidence/test runs"
- "evidence + sign-off added, ready to close"

A typical work package merges with **1–5 commits**, not one per task.

Forbidden on shared branches:

- `WIP`, `wip`, `checkpoint`, `tmp` commits
- "fix typo" / "fix typo again" / "actually fix it" sequences
- Commits whose subject is a literal task ID (`T2: ...`) without semantic content

If you need to checkpoint locally, do it on a personal branch and squash before opening the PR.

---

## 6. Merge strategy

Default: **merge commit** (`--no-ff`).

- Preserves branch history; merge bubbles show where work joined `main`.
- Branch commits remain visible in history; full audit trail.
- Use `git log --first-parent` for a clean linear feature view when desired.

**Exception: squash merge.** Allowed when a branch's intermediate commits genuinely have no value (e.g., a docs branch with three "fix typo" commits). Decided per-PR and justified in the PR body.

**Rebase merge** is not the default. Reserved for edge cases (e.g., a clean spike-to-feature promotion where preserving the spike's linear history is the right thing). When used, justify in the PR body.

The PR title (in Conventional Commits form) becomes the merge-commit subject by default in GitHub's merge UI.

---

## 7. Tags and versioning

Navo App uses **[Semantic Versioning](https://semver.org/)** with **annotated tags**.

- Format: `v<major>.<minor>.<patch>`. Always annotated (`git tag -a`); never lightweight.
- Tag the merge commit of the release PR (or the milestone-close PR), not a transient branch commit.
- Tag message: a short paragraph describing what the tag captures, plus pointers to [`../CHANGELOG.md`](../CHANGELOG.md) and any relevant readiness reports / docs.

Pre-1.0 conventions:

- `0.x.y` covers the pre-stable development period. Public API and behavior may change between minors.
- **Versions and milestones are decoupled.** Tagging a version does not automatically close a milestone. See [`03-planning-model.md` §15](03-planning-model.md).

The first planned baseline tag for Navo App is v0.1.0.

---

## 8. CHANGELOG.md

Navo App maintains a curated changelog at the repo root: [`../CHANGELOG.md`](../CHANGELOG.md).

Format: **[Keep a Changelog](https://keepachangelog.com/)**.

Sections per release: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`. Use only the ones that apply.

Discipline:

- Each tagged release has exactly one CHANGELOG entry.
- The `[Unreleased]` section accumulates notable changes between releases. At release time, its content moves into the new version's section and `[Unreleased]` is reset to empty.
- Entries are **curated** — they describe notable user-facing or contributor-facing changes, not every commit. Mechanical changes (formatting, typo fixes) are usually omitted.
- The CHANGELOG is editable across versions; it is not frozen evidence.

Relationship to other release-time artifacts:

| Artifact | Purpose | Mutability |
|---|---|---|
| Annotated Git tag | Immutable technical anchor | Immutable once pushed |
| GitHub Release | Human-readable landing page for a tag | Editable |
| `CHANGELOG.md` | Curated per-version summary | Editable across versions |
| Readiness report | Per-work-package audit evidence | Frozen at sign-off |

### 8.1 Date convention

Human-readable dates in workflow artifacts use local-day ISO format `YYYY-MM-DD` in the project owner's timezone (project default: Europe/Berlin). The CHANGELOG release date is the most visible application, but the rule also covers:

- GitHub Release titles or body (when a date is included)
- Tag messages (when a date is included)
- Readiness report `Closed:` fields
- Task-file `Planned:`, `Frozen:`, `Closed:` fields
- Milestone README dates

Rationale: dates record the day the human-experienced action happened. A merge whose underlying UTC timestamp is one day earlier may still be dated for the local day.

Git's own timestamps (commit dates, tag annotation dates, merge dates) remain in UTC as stored by Git; they are unaffected by this convention.

---

## 9. GitHub Releases

Each tagged release gets a corresponding GitHub Release.

- Title: `v<version> — <short label>` (e.g., `v0.1.0 — Baseline`).
- Body: mirrors the matching `CHANGELOG.md` entry, plus links to relevant readiness reports and docs.
- Attach no binaries unless a future release deliberately ships one.

---

## 10. Hotfix procedure

For patching a released/tagged state:

1. Branch from the tag: `git checkout -b hotfix/<slug> v<x>.<y>.<z>`.
2. Make the minimal fix. Keep scope narrow.
3. Open PR → `main`. Merge with merge commit.
4. After merge: tag `v<x>.<y>.<z+1>` (annotated) on the merge commit. Push tag.
5. Add a `CHANGELOG.md` entry for the patch release.
6. Create a GitHub Release for the patch tag.

Hotfix branches are short-lived. They do not become long-lived maintenance branches unless a concrete need arises.

---

## 11. Spike procedure

For time-boxed experiments:

1. Branch as `spike/<question>`. Time-box explicitly (in the branch description or task file).
2. Work freely; commits can be sloppy.
3. At the time-box end, choose one of:
   - extract findings into a `docs/...` PR and delete the branch
   - promote the work into a fresh `feat/...` branch with a clean history
   - delete the branch
4. **Spike branches do not merge raw to `main`.**

---

## 12. Pull requests

Each PR represents a unit of work — typically one work package, but smaller scopes (one bug, one doc revision) are fine.

PR title: Conventional Commits subject form (mirrors what the merge commit will become).

PR body — required sections:

- **Summary** (1–3 bullets) — what changes, why.
- **Work package / planning context** — link to the task file and (when present) the readiness report. Optional for small PRs that are not part of a planned work package.
- **Test plan / verification** — what you ran or checked. For docs-only PRs, this can be "self-review against the discovery map in [`03-planning-model.md` §13](03-planning-model.md)".
- **Out-of-scope notes** (optional) — anything deliberately deferred.

Self-review (or reviewer) checklist:

- Frozen artifacts (closed readiness reports, prior milestone closures) byte-identical (no accidental edits).
- Cross-references resolve.
- Convention demonstration: branch prefix, commit messages, merge strategy.

---

## 13. Where to find what

For Git mechanics: this document.

For everything else (milestones, work packages, task-file structure, readiness reports, ADR conventions, the full discovery map): [`03-planning-model.md`](03-planning-model.md).

---

## 14. Milestone-branch policy (conditional, solo / AI-assisted phase)

**Status:** Available as an opt-in for the current phase. **The default per-WP short-lived-branch + PR model in §2 / §3 / §6 / §12 remains the long-term convention** — this section is a conditional adjustment for the current phase, not a replacement.

When to use which workflow:

- **Default (per-WP-PR)** — every change goes through a short-lived branch and PR into `main`. Use this whenever multiple contributors are active, when CI gates per-PR, or whenever in doubt.
- **Milestone-branch (this policy)** — opt-in alternative for solo / AI-assisted work, where coordinating per-WP PRs creates ceremony without review value.

Milestone branches at `milestone/<NN>-<slug>` — using the milestone identifier defined in [`03-planning-model.md` §4](03-planning-model.md) — are an **allowed alternative** to per-WP short-lived branches. Example: `milestone/00-bootstrap-foundation`.

### 14.1 Conditions (all must hold)

- **Solo or solo-AI workflow.** No other contributors are actively depending on `main` between milestone-close merges.
- **Product-owner opt-in for the specific milestone.** Adoption is per-milestone, not blanket. The opt-in is recorded in the milestone README header (alongside Status / Started / Target close) — the README is the per-milestone status document per [`03-planning-model.md` §12](03-planning-model.md); the policy itself stays in this section.

When any condition no longer holds, the per-WP-PR default in §2 / §3 / §6 / §12 resumes for the next milestone.

### 14.2 Commit discipline (unchanged)

Conventional Commits subjects per §4. Logical work units per §5. No per-task or "WIP" commits. Each work package's planning-freeze, execution checkpoints, and closure remain **separable commits** inside the milestone branch — there is no end-of-milestone megacommit. Work-package boundaries (planning → freeze → execution → closure) carry through identically to the per-WP-branch model; only the surrounding Git ceremony changes.

### 14.3 Merge cadence

The milestone branch merges back to `main` **once, at milestone completion** — not after each work package. The milestone is the unit of stable completion for `main`. Internal work-package closures are useful audit boundaries on the milestone branch but do not by themselves trigger a main merge.

The milestone-close merge is a single PR `milestone/<NN>-<slug>` → `main`. It follows the standard PR template in §12 and carries the annotated version tag for the milestone (per §7) on the merge commit. CHANGELOG `[Unreleased]` entries accumulated on the milestone branch land on `main` at this merge and move into the new version section per §8.

### 14.4 `main` invariants preserved

- `main` remains **always-shippable** — anything that lands on `main` is in a coherent, releasable state.
- Annotated tags go on `main` merge commits, never on milestone-branch commits.
- `main` may be **"behind but stable"** relative to active milestone work — that is the design intent for the current phase, not a regression.

### 14.5 Hotfixes during an active milestone

A critical fix to `main` follows the hotfix procedure in §10. The milestone branch then **absorbs** the change via `git merge main` from inside the milestone branch (preserves history; no rewriting).

Do **not** merge milestone-branch progress to `main` to "carry along" a hotfix — that would expose mid-milestone state to `main`, which violates the merge cadence in §14.3.

### 14.6 Small docs work outside the milestone

Self-contained docs work that is orthogonal to the active milestone (e.g., a standalone ADR amendment, a runbook update, a one-off prompt fix) may still flow as a short-lived branch from `main` during a milestone period.

The milestone branch is **permission, not mandate.** Choose per change: if the work belongs to the milestone, land it on the milestone branch; if orthogonal, use the default short-lived-branch path. When in doubt, prefer the default short-lived branch — it is reversible by abandonment; the milestone branch is not.

### 14.7 Sunset

This policy is reverted to the per-WP-PR default when project conditions change — a second human contributor, CI per-PR gating, or any other shape that benefits from per-PR review boundaries. Reversion is by amendment to this section, recording the date and the trigger.

---

## 15. Future additions (not yet adopted)

Documented here so they're easy to add later:

- **Branch protection on `main`** — require PR, no direct push, require linear-or-merge history. Recommended once this document is approved.
- **CI** — smoke runners as the natural first jobs.
- **commitlint / pre-commit hooks** — enforce Conventional Commits and basic hygiene. Adopt only if drift becomes visible by inspection.
- **Issue / PR templates** — useful once the project grows beyond solo + AI.
