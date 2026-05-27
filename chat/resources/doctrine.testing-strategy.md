# Testing-strategy doctrine — reasoning rules

> ChatGPT-side reasoning rules for the testing-strategy doctrine layer. Use these when generating planning prompts, work-package close instructions, audit prompts, or any workflow output that needs to handle testing-related concerns.
>
> The doctrine content itself lives in the host repo at `docs/10-testing-strategy.md`. This file does not duplicate that content; it tells you HOW to use it.

## What testing-strategy doctrine is

`docs/10-testing-strategy.md` is the host project's **living testing-strategy index** — a descriptive document that records current expectations about what gets tested, at what layer, with what kind of evidence. It references binding sources (working rules on provider/adapter and review discipline, scope baselines, ADRs) rather than replacing them.

Testing-strategy doctrine **describes**; the binding sources **bind**.

## When to consult testing-strategy doctrine

| Workflow | What to do with doctrine |
|---|---|
| Planning (work-package drafting) | Identify which testing-doctrine topics the WP touches. Surface them as the **Testing strategy** entry in the work package's Doctrine touchpoints section. |
| Implementation bridge | Restate Testing touchpoints at execution start; pause on undeclared-touchpoint drift. |
| Work-package close | Validate test evidence against declared touchpoints; surface proposed doctrine deltas (e.g., a topic moving from `pending` → `established`) under §5b of the readiness report. |
| Audit | Validate declared touchpoints declared-first; check for incidental violations second; classify findings. |

## Reasoning rules

1. **Doctrine never overrides binding sources.** Working rules and scope baselines bind. Doctrine restates them as testable expectations.

2. **Doctrine evolution is gated.** Same as architecture doctrine — see `docs/09-architecture.md` §4 for the canonical evolution rules.

3. **Touchpoint declaration is required.** Testing-strategy touchpoint values: a concrete topic name from `docs/10-testing-strategy.md`, `None applicable`, or `N/A — doctrine scaffold-only`. Missing or empty Doctrine touchpoints section blocks freeze.

4. **Status semantics matter.** Same as architecture (`established` / `pending` / `not applicable` / `superseded`).

5. **Pre-runtime context awareness.** Host projects that adopt this doctrine layer pre-runtime will have most runtime-touching testing topics marked `not applicable — no runtime exists`. Do not invent test commands or assume a test framework; rely on what the doctrine actually records. The active scope baseline (`docs/08-scope.md`, renamed per phase) governs per-phase test acceptance; the doctrine is the cross-phase view.

## Boundaries

- Do not author the host project's project-specific testing content from this file.
- Do not assume a testing framework, runner, or CI shape unless the doctrine records it as `established`.
- Project authority for testing posture lives in `docs/10-testing-strategy.md` (cross-phase doctrine) and `docs/08-scope.md` (per-phase acceptance).
