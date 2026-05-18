# Navo App — Working Rules

**Status:** Approved baseline (template default — replace with your project's approval date once reviewed).

---

## 1. Purpose

This document records the **minimal critical** operational rules that must be respected during current-phase implementation. These rules prevent avoidable mistakes around scope, dependencies, decisions, secrets, and review discipline. They are not exhaustive engineering standards; they are the rules whose violation is most expensive to undo retroactively.

A rule may be broken only via the exception process in §11.

---

## 2. Scope and implementation discipline

- **No application implementation begins** until the project has:
  1. an approved scope baseline document at [`08-scope.md`](08-scope.md) (renamed per phase, e.g., `08-poc-scope.md`),
  2. an approved working-rules document (this document),
  3. any phase-specific preconditions recorded in the scope document.
- **Documentation-first.** Every new technical concern lands in the relevant document before code is written.
- **No package manager files, runtime scaffolding, infrastructure, application code, or new dependencies** unless explicitly approved. This includes `requirements.txt`, `pyproject.toml`, `package.json`, `Dockerfile`, `docker-compose.yml`, `.env.example`, framework configuration files.
- **Keep changes small and reviewable.** No large unreviewable batches.
- **No silent scope expansion.** Do not add features, refactor beyond the task, or introduce abstractions for hypothetical future requirements. Three similar lines is better than a premature abstraction.
- **No infrastructure (HTTP server, background workers, queues, observability stack) unless explicitly authorized** by the active scope document.
- **Task files.** Once a work package is planned, its acceptance criteria and (optionally) tasks live in `context/tasks/<NN>-<milestone-slug>/NN-<work-package-slug>.md`. Format and discipline are documented in [`../context/tasks/README.md`](../context/tasks/README.md). **Do not start a work package that has no frozen task file.** Mirror the active work package's pending and in-progress tasks (if any) into TodoWrite during execution.

---

## 3. Documentation hygiene

- **ADRs follow Proposed → Accepted → Superseded.** Status moves only with explicit approval. Superseded ADRs stay in place with a status pointer to the replacement.
- **ADRs cover binding decisions** that have cross-work-package or cross-phase implications. Every ADR carries a `Scope:` field at the top — values may include `foundation` (engine choice, storage class, etc.) or a phase identifier (e.g., the active phase name). Work-package-local choices without cross-package implications stay in the task file's "Resolved planning decisions" section, not in `docs/decisions/`.
- **Scope baseline document** at [`08-scope.md`](08-scope.md) is authoritative for the phase it covers. It supersedes general-direction documents for matters explicitly listed in its cross-reference section.
- **Research and synthesis discipline** (when [`research/`](research/) is used): research documents are **immutable once cited by a synthesis**. Errata go in a new document, not a retroactive edit. Syntheses are versioned (`v1-...md`, `v2-...md`); old syntheses stay in place when superseded. See [`research/README.md`](research/README.md).
- **Source/ideation documents** in [`../context/product-source/`](../context/product-source/) are **not authority**. They feed the refined product/technical docs at [`05-product-context.md`](05-product-context.md), [`06-feature-blueprint.md`](06-feature-blueprint.md), [`07-technical-direction.md`](07-technical-direction.md). Insights from ideation are promoted through the normal ADR / scope / planning workflow; they do not bind by their presence alone.
- **[`CLAUDE.md`](../CLAUDE.md) at the repo root is the on-load context entry point** for Claude Code and other agent sessions. It is concise on purpose and points to authoritative documents rather than restating them. Rules belong in this document, not in `CLAUDE.md`.

---

## 4. Dependency and pinning rules

> Customize per project. Examples (delete what doesn't apply):
>
> - **Pin core framework to a specific minor version.** Treat every minor upgrade as a real change with a smoke test pass before adoption.
> - **Pin sticky parameters** (embedding dimension, model name conventions, schema versions) explicitly in this section. Document the rebuild cost of changing each.
> - **Configuration-level, never hardcoded** — model names, provider choices, feature flags, environment-specific values.
> - **No new runtime dependency** without explicit approval. Each candidate is scrutinized for license, maintenance signal, and operational footprint before it lands.

---

## 5. Provider / adapter discipline (if applicable)

> Customize per project. Examples (delete if not relevant):
>
> - **Business logic must not import provider-specific modules directly.** All provider calls go through adapter shapes (interface / Protocol).
> - **Mock seam wired up from day one.** A smoke test runs the same logic against the live provider and the mock, to prove the seam is real.
> - **Approved provider list.** Anything else requires explicit review.

---

## 6. Secrets and cost controls

- **API keys live in environment variables only** — local `.env` file at small scale.
- **`.env` is gitignored before the first commit**, not as a cleanup item later.
- **No secrets are committed** in any branch. If a secret enters git history, rotate the key immediately and rewrite history before pushing.
- **Spend caps apply if the project incurs paid usage.** Provider dashboard limits enforce the hard cap; do not rely on monitoring alone. Project default: EUR 10/day soft guideline for early AI/API experiments; hard limits TBD after provider, workload, and architecture decisions.
- **Daily spend monitoring** — not weekly — when paid providers are in use. The monitoring step has an explicit owner.
- **If cumulative spend approaches the hard cap, work pauses** and the cap is reviewed before continuation. The kill-switch is explicit, not implicit.

---

## 7. Review discipline

- **Each work package's outputs are reviewed before close.** A readiness report at `context/sanity-checks/<NN>-<milestone-slug>/NN-<work-package-slug>/readiness-report.md` records the per-task / per-acceptance-criterion outcomes. Template at [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md).
- **Independent audit pass is encouraged for non-trivial work packages** — a separate AI tool (or human reviewer) validates the implementation against the frozen task file's acceptance criteria. Findings are evaluated, not auto-applied.
- **A readiness report is frozen evidence.** Once signed off, do not edit it.

---

## 8. Local environment isolation

- **Use isolated local environments for development.** Default pattern: `.venv/<purpose-slug>/` (Python venv) or the language-equivalent (Node `node_modules/` per directory, etc.). Segregate by phase or work package to prevent dependency drift.
- **Recommended naming** when phases or work packages have distinct dependency sets: `.venv/<phase-slug>/` (e.g., `.venv/poc/`) or `.venv/<milestone-slug>/<NN-wp-slug>/` for finer-grained isolation. Document each environment's purpose in [`04-runbook.md`](04-runbook.md) §2.
- **Never install dependencies into a shared/global environment** for project work. The cost-of-hindsight on this rule is high.
- **Stable user/device facts** (OS, editor, package managers, language runtime install conventions) belong in [`../chat/resources/user-environment.md`](../chat/resources/user-environment.md), not duplicated per project.

---

## 9. Design-track discipline (when active)

- **Design track is optional.** Activate by populating [`../context/design/`](../context/design/) and writing a `design-status.md` pointer index. Until then, treat this section as future-facing structure.
- **Design and implementation are separate modes.** Do not mix them in the same session. See the design lifecycle in [`../chat/resources/playbooks.workflows.md`](../chat/resources/playbooks.workflows.md).
- **Design documents are not implementation authority.** Insights are promoted into ADRs, scope baselines, or work-package task files through the normal workflow — never silently.
- **Visual generation, design-system handoff, or related work** does not begin until reviewed pre-flight decisions exist.

---

## 10. Project-specific rules

> Add domain-specific working rules here as they emerge. Examples (delete if not applicable):
>
> - *Pipeline / processing discipline* — input validation, fallback rules, determinism rules.
> - *Evaluation rules* — how the project measures success; what counts as a hard failure.
> - *Demo / stakeholder rules* — what stakeholder-facing artifacts must do or must not do.
> - *Security and governance honesty* — what is real vs stub; what disclaimers are mandatory.
> - *Logging and observability* — default to plain logging; no observability stack unless authorized.

---

## 11. Exception process

To break a rule in this document or one of its caps:

1. **Record why** the exception is needed, in writing, in the relevant document or as a short note in the repo.
2. **Record who approved it** — the product owner.
3. **Record whether it is temporary**, and if so, what the rebuild date is.
4. **No silent exceptions.** A rule break that is not recorded is a violation.

For exceptions that affect the foundation (engine choice, sticky-parameter pins, ADR commitments), record an ADR amendment or new ADR rather than a one-off exception note.

---

## 12. Key principle

```text
Documentation first. Small, reviewable changes.
Adapter the providers. Pin the sticky parameters.
Validate at the boundary. Drop the unknown.
The hard cap is enforced in the dashboard, not by hope.
Stub fields are not security; the disclaimer is mandatory.
```

> Customize the closing aphorisms to fit your project's discipline.
