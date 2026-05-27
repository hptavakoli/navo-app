# Navo App — Claude Onboarding

Navo App is the end-user application for Navo, an AI-powered migrant life intelligence platform for Germany. It gives immigrants and foreign residents a personalised, source-linked, translated, practical, and optionally listenable feed of life-relevant updates.

**Project status:** Initialised from the foundation template. First milestone is `01-foundation` (placeholder — not yet opened); no work packages have been opened or executed. Flutter (Android/iOS first, possible Flutter Web/desktop later) is the proposed direction, validated during `01-foundation`. Three boundaries are explicit: the app consumes `navo-backend` APIs and does not own backend product logic; public SEO/AEO is owned by `navo-website`; the external content/data creation pipeline is out of scope and is consumed via the backend.

The user is the product owner / decision maker. You are the engineering partner, reviewer, and challenger. Confirm before proceeding to any next step. After completing any milestone, report back with a structured summary and wait for direction.

---

## Session mode

Sessions are either **development-oriented** (default) or **design-oriented** (when the design track at [`context/design/`](context/design/) is active). The two modes do not mix in the same session.

- **Development mode**: read this dispatcher, the active milestone README, and the active work-package task file. Skip [`context/design/app.design-status.md`](context/design/app.design-status.md) unless explicitly relevant.
- **Design mode**: enter via [`context/design/design-overview.md`](context/design/design-overview.md) and [`context/design/app.design-status.md`](context/design/app.design-status.md). Working rule §9 in [`docs/01-working-rules.md`](docs/01-working-rules.md) governs the boundary.

---

## Implementation status

> No work packages closed yet.

---

## Authoritative documents

In priority order for current concerns:

**Operational discipline (always present):**

1. [`docs/01-working-rules.md`](docs/01-working-rules.md) — operational rules for current implementation work. The hard rules below are extracted from this document; the working rules are the source of truth.
2. [`docs/02-git-workflow.md`](docs/02-git-workflow.md) — Git workflow, branches, commits, merges, tags, releases, CHANGELOG conventions.
3. [`docs/03-planning-model.md`](docs/03-planning-model.md) — milestones, work packages, local numbering, task-file conventions, version-vs-milestone decoupling rule, canonical discovery map.
4. [`docs/04-runbook.md`](docs/04-runbook.md) — operational quick-reference (commands, environments, demo flows).

**Project-shaping (fill in as direction firms up):**

5. [`docs/05-product-context.md`](docs/05-product-context.md) — who/what/why for the project.
6. [`docs/06-feature-blueprint.md`](docs/06-feature-blueprint.md) — capability outline.
7. [`docs/07-technical-direction.md`](docs/07-technical-direction.md) — bootstrap-time stack and initial architecture direction. The living architecture counterpart is [`docs/09-architecture.md`](docs/09-architecture.md).
8. [`docs/08-scope.md`](docs/08-scope.md) — frozen scope baseline for the active phase (renamed per phase, e.g., `08-poc-scope.md`).

**Engineering doctrine (living):**

9. [`docs/09-architecture.md`](docs/09-architecture.md) — architecture doctrine; living counterpart to `07-technical-direction.md`.
10. [`docs/10-testing-strategy.md`](docs/10-testing-strategy.md) — testing-strategy doctrine.
11. [`docs/11-engineering-practices.md`](docs/11-engineering-practices.md) — engineering-practices doctrine.

**Decisions and research:**

- [`docs/decisions/`](docs/decisions/) — ADRs (binding decisions). Only the template (`0000-template.md`) is present until real ADRs are authored.
- [`docs/research/`](docs/research/) — optional. Research and synthesis trail. Activate when scoping requires evidence-gathering before commitments.

**Templates** (load on demand):

- [`context/tasks/README.md`](context/tasks/README.md) — task-file template.
- [`context/sanity-checks/README.md`](context/sanity-checks/README.md) — readiness-report template.
- [`context/tasks/_milestone-template/README.md`](context/tasks/_milestone-template/README.md) — milestone README scaffold.

**Optional folders:**

- [`context/design/`](context/design/) — design-track working knowledge. Activate by populating; until then, treat scaffolds as future-facing.
- [`context/product-source/`](context/product-source/) — raw ideation / source documents. **NOT project authority** — read on demand for grounding; promote insights through ADR / scope / planning workflow.

**Non-authoritative AI-tooling resources** (do NOT treat as a source of truth):

- [`chat/`](chat/) — AI-coordination tooling: ChatGPT-uploaded resources (`chat/resources/`) and session prompt templates for Claude / ChatGPT / optional auditor (`chat/prompts/`). See [`chat/README.md`](chat/README.md) for the resources/prompts split and the explicit "not project authority" boundary. Project state in `chat/resources/app.project-state.md` is a point-in-time ChatGPT-side snapshot and may be behind reality; live authority remains the documents listed above.

---

## Hard rules

At-a-glance cross-references to the project's authoritative rule sources: [`docs/01-working-rules.md`](docs/01-working-rules.md) (current implementation rules); [`docs/02-git-workflow.md`](docs/02-git-workflow.md) and [`docs/03-planning-model.md`](docs/03-planning-model.md) (workflow and planning discipline). Those documents remain the source of truth.

### Implementation discipline
- **No application implementation, runtime structure, dependencies, infrastructure, or deployment** without explicit approval.
- **No package manager files** (`requirements.txt`, `pyproject.toml`, `package.json`, `Dockerfile`, `docker-compose.yml`, `.env.example`, etc.) without explicit approval.
- **Documentation-first.** New technical concerns land in the relevant document before code is written.
- **No silent scope expansion.** Don't add features, refactor beyond the task, or introduce abstractions for hypothetical future requirements.
- **Keep changes small and reviewable.**

### Documentation discipline
- **Decisions become ADRs.** Binding architectural choices follow the Proposed → Accepted → Superseded lifecycle. Status moves only with explicit approval.
- **Source/ideation documents in [`context/product-source/`](context/product-source/) are NOT authority.** They feed refined product/technical docs through promotion, not by their presence.
- **Research documents are immutable once cited by a synthesis.** Syntheses are versioned. See [`docs/research/README.md`](docs/research/README.md) when the research track is active.
- **CLAUDE.md is concise on purpose.** Rules belong in `docs/01-working-rules.md`, not here.

### Workflow discipline
- **Git, branches, commits, merges, tags, releases, changelog:** see [`docs/02-git-workflow.md`](docs/02-git-workflow.md). GitHub Flow + version tags; merge-commit default with squash by exception; Conventional Commits; logical-unit commits (no commit-per-task); SemVer + annotated tags; Keep-a-Changelog format.
- **Git operation ownership:** governed by [`docs/02-git-workflow.md` §1.1](docs/02-git-workflow.md). Track for this project: `approval-gated`. Claude drafts Git output (branch names, Conventional Commit subject + body, PR title + body, staging set) and executes routine Git writes end-to-end after the user approves a named bounded operation. Read-only Git/GitHub inspection is always allowed.
- **No AI attribution in GitHub-facing output.** Commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries, and GitHub comments must not carry `Co-authored-by`, `Generated with`, or any equivalent AI-attribution line — unless the user explicitly requests it for that specific output. Internal documentation may acknowledge AI-assisted workflow; GitHub-facing output may not. See [`docs/02-git-workflow.md` §1.2](docs/02-git-workflow.md).
- **Milestones, work packages, task-file structure:** see [`docs/03-planning-model.md`](docs/03-planning-model.md). Milestones use `NN-<slug>/` (default start `01-`; `00-` reserved for explicit bootstrap phases). Work packages restart at `01-` per milestone. Tasks use `T0..TN`. **Versions and milestones are decoupled** (a tag marks but does not cause milestone closure).
- **Templates:** [`context/tasks/README.md`](context/tasks/README.md) (task files); [`context/sanity-checks/README.md`](context/sanity-checks/README.md) (readiness reports).
- **Discovery map:** [`docs/03-planning-model.md` §13](docs/03-planning-model.md) is the single source of truth for "where to find rule X".

### Mode discipline
- **Development mode and design mode do not mix.** Working rule §9 in [`docs/01-working-rules.md`](docs/01-working-rules.md) governs the boundary.
- **Design documents are not implementation authority.** Insights are promoted through ADRs, scope baselines, or work-package task files — never silently.

### Project-specific hard rules

> Add hard rules specific to this project below — provider/model lock-ins, dependency pin policies, evaluation rules, security rules, cost caps, etc. Examples (delete what doesn't apply):
>
> - *Provider, model, dimension* — e.g., "Embedding dimension pinned at 1024."
> - *Secrets and cost* — e.g., "API keys via environment variables only. `.env` is gitignored. Spend cap: EUR 10/day soft guideline for early AI/API experiments; hard limits TBD after provider, workload, and architecture decisions."
> - *Domain rules* — e.g., evaluation discipline, security rules, demo rules, governance constraints.
>
> Each rule should be one sentence with the *why* implied or referenced.

### Exception process
- Rule breaks are recorded (why / who approved / temporary or not). No silent exceptions. Foundation-level exceptions get an ADR amendment, not a one-off note.

---

## Common commands

> Quick reference. The runbook at [`docs/04-runbook.md`](docs/04-runbook.md) owns the per-environment, per-mode, per-cost detail. Until the first runtime exists, this section is empty.

---

## Recommended next step for a fresh session

The immediate next action is:

1. **Open the first milestone planning conversation for `01-foundation`.** The milestone README at [`context/tasks/01-foundation/README.md`](context/tasks/01-foundation/README.md) is currently a placeholder; the planning conversation defines the foundation work packages (app framework validation ADR, app surface strategy, state management/architecture ADR, localisation ADR, auth integration ADR, API contract dependency planning, audio approach research, notification ADR, subscription ADR, MVP-scope decision) before any runtime implementation begins.

For a fresh session orienting from cold:

- Read [`docs/05-product-context.md`](docs/05-product-context.md) (when filled), [`docs/01-working-rules.md`](docs/01-working-rules.md), [`docs/02-git-workflow.md`](docs/02-git-workflow.md), [`docs/03-planning-model.md`](docs/03-planning-model.md). Read [`docs/04-runbook.md`](docs/04-runbook.md) for operational quick-reference. Read the engineering-doctrine docs ([`docs/09-architecture.md`](docs/09-architecture.md), [`docs/10-testing-strategy.md`](docs/10-testing-strategy.md), [`docs/11-engineering-practices.md`](docs/11-engineering-practices.md)) when the session involves architecture, testing posture, or engineering practices.
- Read the active milestone README at `context/tasks/<NN>-<milestone-slug>/README.md` for status, dependencies, and parallelism.
- Do not begin execution of any work package at `roadmap (not frozen)` or `placeholder` status without an explicit planning conversation that freezes the task file. Do not auto-start anything; explicit product-owner direction is required.
- If the user asks "what's next?", confirm the working-rules baseline still holds and offer the action recorded in the section above.
