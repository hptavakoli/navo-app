# Architecture doctrine — reasoning rules

> ChatGPT-side reasoning rules for the architecture doctrine layer. Use these when generating planning prompts, work-package close instructions, audit prompts, or any workflow output that needs to handle architecture-related concerns.
>
> The doctrine content itself lives in the host repo at `docs/09-architecture.md`. This file does not duplicate that content; it tells you HOW to use it.

## What architecture doctrine is

`docs/09-architecture.md` is the host project's **living architecture index** — a descriptive document that records current architectural principles, seams, and invariants. It references binding sources (ADRs, working rules, scope baselines) rather than replacing them.

Architecture doctrine **describes**; ADRs / working rules / scope baselines **bind**. When you generate prompts or reason about architecture for the host project, treat doctrine as the consolidated view and treat the binding sources as authoritative.

## When to consult architecture doctrine

| Workflow | What to do with doctrine |
|---|---|
| Planning (work-package drafting) | Identify which architecture-doctrine topics the proposed WP touches. Surface them as the **Architecture** entry in the work package's Doctrine touchpoints section. |
| Implementation bridge | Restate the WP's declared Architecture touchpoints at execution start. If implementation drifts to an undeclared touchpoint, pause and surface as a doctrine-touchpoint deviation. |
| Work-package close | Validate the work against the declared Architecture touchpoints. Surface any proposed doctrine delta (new topic, status change, wording refinement) under §5b of the readiness report. |
| Audit | Validate declared Architecture touchpoints against the actual diff (declared-first). Then check for obvious external doctrine violations (work that conflicts with current doctrine but did not declare the touchpoint). Classify findings as `doctrine-touchpoint-deviation` or `doctrine-violation (incidental)`. |

## Reasoning rules

1. **Doctrine never overrides binding sources.** If doctrine appears to conflict with an ADR or working rule, the binding source wins. Flag the conflict and propose a doctrine update; do not edit doctrine silently.

2. **Doctrine evolution is gated.** Architecture-doctrine changes are accepted via product-owner approval inside a planning conversation, audit response, or work-package closure. They are NEVER applied silently. The canonical evolution rules live in `docs/09-architecture.md` §4.

3. **Touchpoint declaration is required.** Every frozen task file carries a Doctrine touchpoints section with three domain bullets. For architecture, the value is either a concrete topic name from `docs/09-architecture.md`, `None applicable`, or `N/A — doctrine scaffold-only`. Missing or empty section blocks freeze.

4. **Status semantics matter.** When citing an architecture topic:
   - `established` — the topic is recorded in doctrine and bound by an existing source. Use as-is in reasoning.
   - `pending` — the topic is identified but not yet resolved (typically waiting on a foundation ADR or specific binding source). Treat as an open question; do not assume a decision.
   - `not applicable` — the topic does not apply at the host project's current stage. Skip unless the stage changes.
   - `superseded` — replaced by a later doctrine update or ADR. Follow the pointer to the replacement.

5. **Pre-foundation status awareness.** Host projects often adopt this doctrine layer before all foundation ADRs have landed. In that state, most architecture-stack topics will be `pending` rather than `established`. Surface this dependency explicitly in generated planning prompts; do not treat pending topics as resolved decisions.

## Boundaries

- Do not author the host project's project-specific doctrine content from this file. That content lives in `docs/09-architecture.md` and is maintained through the host repo's planning / review workflows.
- Do not import architecture topics from other projects. Each host project's doctrine reflects its own bound sources and pending questions.
- Do not treat this file as authoritative for the host project. It is a ChatGPT-side reasoning rulebook; project authority lives in the docs.
