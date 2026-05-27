# Engineering-practices doctrine — reasoning rules

> ChatGPT-side reasoning rules for the engineering-practices doctrine layer. Use these when generating planning prompts, work-package close instructions, audit prompts, or any workflow output that needs to handle engineering-practice concerns (change discipline, dependency handling, secrets, environment isolation, review cadence, Git posture).
>
> The doctrine content itself lives in the host repo at `docs/11-engineering-practices.md`. This file does not duplicate that content; it tells you HOW to use it.

## What engineering-practices doctrine is

`docs/11-engineering-practices.md` is the host project's **living engineering-practices index** — a descriptive document that records current expectations about how work is done. It references `docs/01-working-rules.md`, `docs/02-git-workflow.md`, `docs/03-planning-model.md`, and ADRs as binding sources.

Engineering-practices doctrine **describes**; the binding sources **bind**.

## When to consult engineering-practices doctrine

| Workflow | What to do with doctrine |
|---|---|
| Planning (work-package drafting) | Identify which engineering-practice topics the WP touches (e.g., ADR lifecycle, Conventional Commits, frozen-task-file precondition). Surface them as the **Engineering practices** entry in the work package's Doctrine touchpoints section. |
| Implementation bridge | Restate touchpoints at execution start; pause on undeclared-touchpoint drift. |
| Work-package close | Validate against declared touchpoints; surface proposed doctrine deltas under §5b of the readiness report. |
| Audit | Declared-first validation, then incidental-violation check; classify findings. |

## Reasoning rules

1. **Doctrine never overrides binding sources.** Working rules and Git workflow bind. Doctrine restates and indexes; the binding source wins on conflict.

2. **Doctrine evolution is gated.** Same as architecture doctrine — see `docs/09-architecture.md` §4.

3. **Touchpoint declaration is required.** Engineering-practices touchpoint values: a concrete topic name from `docs/11-engineering-practices.md`, `None applicable`, or `N/A — doctrine scaffold-only`. Missing or empty section blocks freeze.

4. **Status semantics matter.** Same as architecture (`established` / `pending` / `not applicable` / `superseded`).

5. **Git posture is configured.** The host project's `docs/02-git-workflow.md` §1.1 records the configured Git operation ownership track (`human-owned` or `approval-gated`). When generating Git-related instructions or commit drafts, respect that gate and present the draft for user approval; do not issue commands that imply automatic execution beyond what the configured track permits.

6. **No AI attribution in GitHub-facing output.** `docs/02-git-workflow.md` §1.2: commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries, and GitHub comments must NOT carry `Co-authored-by`, `Generated with`, or any equivalent AI-attribution line — unless the user explicitly requests it for that specific output. This is unconditional and applies regardless of which Git track is configured. When drafting any of these surfaces, omit AI attribution by default.

## Boundaries

- Do not author the host project's project-specific engineering-practices content from this file.
- Do not propose CI/build/lint toolchain choices unless the doctrine records them as `established` — pre-foundation projects will have most toolchain topics `pending`.
- Project authority lives in `docs/01-working-rules.md`, `docs/02-git-workflow.md`, `docs/03-planning-model.md`, and (descriptively) `docs/11-engineering-practices.md`.
