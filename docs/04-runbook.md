# Navo App — Runbook

**Status:** Placeholder. Fill in as the project grows operational surface area.

> Operational quick-reference. Belongs alongside the working rules and planning model — but unlike those, the runbook evolves continuously. It exists from day one so the habit takes hold; sections start as scaffolds and acquire real content as commands, environments, and demo flows accumulate.

The runbook is **operational**, not authoritative. Working rules and ADRs constrain *what* and *why*; the runbook records *how to actually run things*.

---

## When to update this file

Update whenever any of the following changes:

- A new local environment, virtual environment, or container is introduced.
- A new launchable surface is added (CLI command, web app, smoke runner, evaluation runner, demo script).
- A mode flag, env-var requirement, or startup gate changes.
- A demo procedure is rehearsed or refined.
- A cost calibration is performed against a paid provider.
- A troubleshooting recipe is needed twice — record it the second time.

If you find yourself searching for a command across old chat threads, that command belongs here.

---

## 1. Prerequisites

> List the minimum machine state needed to work on this project from a fresh checkout. Examples (delete what doesn't apply):
>
> - Operating system and version
> - Language runtime and version (e.g., Python 3.11.x, Node 20.x)
> - Package managers / system tools (e.g., Homebrew, `pyenv`, `nvm`)
> - Editor or IDE expectations
> - Required accounts and access (e.g., a project-specific repo, registry, or provider)

Do NOT include the user-global / device-stable facts here — those belong in `chat/resources/user-environment.md` (the project-overlay version of the user's standard machine profile). This section is for project-specific deltas only.

---

## 2. Local environment

> List every local environment / virtual environment used by the project, in order. For each, record:
>
> - **Name / path** (e.g., `.venv/<phase-or-wp-slug>/`)
> - **Purpose** (which phase / work package / runtime concern it serves)
> - **Creation command** (one-liner the user can paste)
> - **Activation command** (or "no activation; invoked by full path")
> - **Locked dependencies** (point at the manifest, e.g., `requirements.txt`, `package.json`, or note "locked at WP-NN close")
>
> Recommended naming convention: `.venv/<purpose>/` — segregated by phase or work package to prevent dependency drift across phases.
>
> Until the first venv exists, this section can read: "No environments yet; first venv will be authored at <WP-slug> task-file freeze."

---

## 3. Common commands

> One-line commands that humans and AI run frequently. Group by surface (e.g., "Smoke runners," "App launches," "CLI commands," "Eval runs"). For each, include:
>
> - The exact command (paste-ready)
> - Required env vars (if any)
> - Mode flags (if any)
> - Expected runtime / cost (if non-trivial)
>
> Until the first runtime exists, this section is a single line: "No runtime commands yet."

---

## 4. Mode semantics (if applicable)

> If the project distinguishes runtime modes (e.g., `mock` / `hosted`, `dev` / `prod`, `offline` / `online`), document them here:
>
> - **Mode name** — when to use, what providers it touches, what it costs, what env vars it requires.
> - **Mode-flag policy** — required-no-default vs default-with-override, and the cost-safety reasoning.
>
> If the project has only one mode, omit this section.

---

## 5. Demo procedures

> Step-by-step procedures for stakeholder-facing demos. Each procedure includes:
>
> - **Audience** — who the demo is for and what they care about.
> - **Pre-flight checklist** — env vars, data, machine state, branch / tag.
> - **Steps** — numbered, paste-ready.
> - **Recovery procedure** — what to do if a step fails mid-demo.
>
> Until the project has a demo-able artifact, omit this section.

---

## 6. Cost calibration (if applicable)

> If the project incurs paid-API spend, record per-operation cost calibration here:
>
> - **Operation name** (e.g., "single retrieval query, hosted mode")
> - **Measured cost** (USD or EUR; date of measurement)
> - **Underlying token / call count** (so the calibration survives provider price changes)
> - **Cap relationship** (how this operation relates to the daily / total caps in `01-working-rules.md`)
>
> If the project does not incur spend, omit this section.

---

## 7. Troubleshooting

> Recipes for recurring issues. Each entry follows:
>
> - **Symptom** — what the user observes.
> - **Likely cause** — short explanation.
> - **Fix** — paste-ready commands or pointers.
>
> Add the recipe the second time you hit the issue, not the first.

---

## 8. References

- [`01-working-rules.md`](01-working-rules.md) — operational rules, including environment isolation discipline.
- [`02-git-workflow.md`](02-git-workflow.md) — Git, releases, CHANGELOG.
- [`03-planning-model.md`](03-planning-model.md) — milestones, work packages, task files.
- [`../chat/resources/user-environment.md`](../chat/resources/user-environment.md) — user-global / device-stable facts that this runbook extends with project-specific detail.
