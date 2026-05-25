# Task files — working pattern

Each file in this folder defines the tasks for one work package. This is the durable, structured source of truth for what the work requires and where it stands. **Any session can pick up work cold by reading this folder — no chat history required.**

This file is **not** auto-loaded into every session. It is loaded on demand when starting a planning conversation (to author a new task file) or starting an execution session for an active work package.

For the broader documentation system, see [`../../CLAUDE.md`](../../CLAUDE.md). Working rules at [`../../docs/01-working-rules.md`](../../docs/01-working-rules.md). The readiness-report template lives at [`../sanity-checks/README.md`](../sanity-checks/README.md). Planning model at [`../../docs/03-planning-model.md`](../../docs/03-planning-model.md).

---

## File naming

`context/tasks/<NN>-<milestone-slug>/NN-<work-package-slug>.md`, where:

- `<NN>-<milestone-slug>` — full milestone folder name; the `<NN>` is the milestone's two-digit number (default `01`, `00` reserved for bootstrap/foundation), and `<milestone-slug>` is the kebab-case slug (e.g., `foundation`).
- `NN` — two-digit local number reflecting recommended execution order at planning time. Each milestone restarts at `01`. Numbering rules in [`../../docs/03-planning-model.md` §5–§8](../../docs/03-planning-model.md).
- `<work-package-slug>` — kebab-case work-package slug describing the work.

The matching readiness report goes at `context/sanity-checks/<NN>-<milestone-slug>/NN-<work-package-slug>/readiness-report.md`. The coupling rule ([`03-planning-model.md` §11](../../docs/03-planning-model.md)) keeps the two paths symmetric.

---

## Template

```markdown
# Work package NN: [Display name]

> **Milestone:** <milestone-slug>
> **Status:** planned | active | closed
> **Planned:** YYYY-MM-DD
> **Frozen:** YYYY-MM-DD       ← added at freeze (planning conversation locked)
> **Closed:** YYYY-MM-DD        ← added at close, after readiness-report sign-off
> **Target close:** which version tag is expected to carry this closure (informational)

## Purpose

1–3 paragraphs: what this work package establishes and why.

## Scope

**In scope:** bulleted deliverables.
**Out of scope:** explicit exclusions.

## Acceptance criteria

(a)–(z) lettered, observable, testable. Each criterion is a closure precondition; criteria do not need to map 1:1 to tasks.

## Task list  *(optional — use only if granular tasks help)*

- T1, T2, ... if explicit task structure aids execution; or omit if scope + acceptance suffices.

## Phase mapping  *(optional — use if execution is staged with review checkpoints)*

## Notes  *(optional)*
```

---

## Granularity guidance

- Use granular `Tx` tasks when a work package is large enough that intermediate checkpoints help.
- Use scope + acceptance only (no `Tx` tasks) for smaller or governance-style work packages.
- Do not pad a small work package with synthetic tasks.

---

## Workflow

### Planning a work package

1. Confirm with the user that we are entering planning mode for a specific work package.
2. Re-read the relevant scope baseline (e.g., `docs/<NN>-scope.md`) and [`../../docs/01-working-rules.md`](../../docs/01-working-rules.md).
3. If a placeholder or partial outline already exists at the target path (see File naming above), read it first and treat it as the starting point; flag any conflict between its content and the planning direction, then pause for user confirmation before drafting further. Draft (or update in place) the task file with `Status: planned`, `Frozen:` left blank, and any `Tx` tasks set to `pending`.
4. Resolve open questions in chat before implementation begins.
5. Update the task file if open-question resolutions change scope.
6. The user reviews and approves.
7. At freeze: add the `Frozen:` date. Implementation may begin only after freeze.
8. If the planning session should end here and implementation should happen in a fresh Claude session, use [`../../chat/prompts/execution/planning-session-close.md`](../../chat/prompts/execution/planning-session-close.md) to verify the frozen task file, update the milestone README row and the `CLAUDE.md` next-step, and prepare the commit. Mid-draft pauses (no `Frozen:` yet) do NOT need a special prompt — the next session resumes in `wp-drafting`.

### Executing a work package

1. At session start: read [`../../CLAUDE.md`](../../CLAUDE.md), the milestone README, and the work-package task file.
2. Mirror `pending` and `in progress` tasks (if any) into TodoWrite for live tracking.
3. For each task or logical work unit:
   - Update status `pending → in progress`.
   - Implement (per [`../../docs/01-working-rules.md`](../../docs/01-working-rules.md)).
   - Update status `in progress → done`.
4. Never mark a task done unless its acceptance criteria are met.
5. Never break a working rule silently — use the exception process in [`../../docs/01-working-rules.md` §11](../../docs/01-working-rules.md).
6. If the session ends before the work package is complete (a clean subset of tasks done; WP remains active), use [`../../chat/prompts/execution/implementation-session-close.md`](../../chat/prompts/execution/implementation-session-close.md) to verify acceptance for each closed task, record a durable resume pointer in the task file `Notes`, update the `CLAUDE.md` next-step, and prepare a logical-unit commit per [`../../docs/02-git-workflow.md` §5](../../docs/02-git-workflow.md) (no WIP / per-task commits).

### Closing a work package

1. Verify all acceptance criteria are met.
2. Author the readiness report at `context/sanity-checks/<NN>-<milestone-slug>/NN-<work-package-slug>/readiness-report.md` per [`../sanity-checks/README.md`](../sanity-checks/README.md).
3. At sign-off: add `Closed: YYYY-MM-DD` to the task-file header and update the milestone README work-packages table.
4. Confirm with the user before opening the next work package.

---

## Cold pickup

If resuming work on a milestone with no prior session context:

1. Read [`../../CLAUDE.md`](../../CLAUDE.md).
2. Read the milestone README at `context/tasks/<NN>-<milestone-slug>/README.md` — it shows current status, dependencies, parallelism, and links.
3. Identify the active work package (status `active` in the README).
4. Read its task file end-to-end.
5. If multiple `in progress` tasks exist within a single work package, pause and ask the user before continuing.
6. Mirror `in progress` and `pending` tasks into TodoWrite.
7. Resume from the current `in progress` task.
