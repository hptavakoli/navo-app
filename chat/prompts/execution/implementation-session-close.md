# Claude: Implementation-session close (mid-WP, partial completion)

## For the user

- **What this is:** the prompt sent to Claude Code at the end of an implementation session that completed a clean subset of the active work package's tasks, to close the session cleanly so a fresh Claude Code session can resume from the next unfinished task.
- **When to use:** at the end of an implementation conversation where one or more tasks reached a clean acceptance boundary but the work package is not yet complete — you want to stop here and resume the remaining tasks in a new Claude Code session.
- **Target AI:** Claude Code.
- **Input:** none — Claude inspects the active WP's task file and surrounding cold-start anchors.
- **Output:** structured report (per-task acceptance evidence, resume pointer added, files updated / NOT modified, suggested commit, cold-start invariant).
- **Not for:**
  - a work package that is fully complete → use [`work-package-close.md`](work-package-close.md);
  - a planning session close → use [`planning-session-close.md`](planning-session-close.md).

--- COPY FROM BELOW ---

You have completed a clean subset of the active work package's tasks in this session. The WP remains active — it is **not** being closed. The next step is to **close this implementation session cleanly** so a fresh Claude Code session can resume from the next unfinished task without depending on chat history.

This is an **implementation-handoff checkpoint**, not a work-package closure.

## Objective

Verify the tasks marked `done` in this session have met acceptance, leave the task file in a clean resume state with a durable next-task pointer, update `CLAUDE.md` to point at the next task, and prepare a logical-unit commit per `docs/02-git-workflow.md` §5 (no WIP / per-task commits).

## Responsibilities

### 1. Task-state verification (CRITICAL)

For each task you intend to mark or have marked `done` in this session:
- Confirm the task's acceptance signal (or the referenced acceptance criterion (a)–(z)) is actually met.
- Cite the evidence concretely — a file path, a test name, a commit, an artifact. The `context/tasks/README.md` rule *"Never mark a task done unless its acceptance criteria are met"* applies here.

If any `done` mark cannot be backed by acceptance evidence, REVERT it to `in progress` (or `pending` if implementation hasn't substantively started) and flag in the report. Do NOT close the session with unsubstantiated `done` marks.

### 2. Clean resume invariant

- The task file MUST have **at most one** task in `in progress` status at the end of this session; **zero is preferred** when the completed subset reaches a clean acceptance boundary.
- If multiple tasks are `in progress`, resolve them — `done` (with acceptance verification above) or revert to `pending` — to leave exactly the intended state. Flag this explicitly in the report.
- The next unfinished task — the one a fresh session will resume from — must be unambiguously identifiable as the lowest-numbered `pending` or `in progress` task.

### 3. Durable resume pointer in the task file

Append (or update) a one-line resume pointer in the task file's `Notes` section:

```
Resume from `Tx` (next session) — completed through `T<prev>` on YYYY-MM-DD.
```

If the task file has no `Notes` section, add one. Keep the pointer terse. This is the durable signal that survives independently of `CLAUDE.md` drift.

### 4. Dispatcher next-step update

Update `CLAUDE.md` "Recommended next step for a fresh session" to:

> *"Resume work package `<milestone-slug>/<NN-wp-slug>` at `Tx` in a fresh Claude Code session via `chat/prompts/workflows/claude-onboarding.md` → cold-start orientation report should report `Project state: wp-executing` → `chat/prompts/workflows/claude-implementation-bridge.md`."*

Replace the prior next-step entry; do not append.

### 5. Cold-start invariant verification

A fresh Claude Code session reading `CLAUDE.md` → active milestone README → this task file MUST produce a cold-start orientation report with `Project state: wp-executing`, `Next work package: <slug>`, and unambiguously identify `Tx` as the next task. Trace the chain mentally and flag any gap.

### 6. Git handling

- Suggested commit: a logical-unit commit covering the completed tasks plus the task-file resume-pointer and dispatcher updates.
- Conventional Commits subject framed by **semantic block completed**, not by task IDs (per `docs/02-git-workflow.md` §5 forbidding WIP / per-task commits). Reference the task ID range in the body if helpful.
- Subject example: `feat(<NN-wp-slug>): <semantic block description>` (e.g., `feat(02-grounded-generation): wire mock provider and chunk loader`). Body may include "Covers T1–T5 of `<NN-wp-slug>`."
- Type chosen per the actual change shape (`feat`, `fix`, `refactor`, `docs`, etc.).
- Whether Claude executes the commit is governed by `docs/02-git-workflow.md` §1.1 (operation ownership); always draft the message and recommended staging set.
- Conventional Commits format per `docs/02-git-workflow.md` §4; presentation per §4.1 (single fenced block).
- No AI attribution in any GitHub-facing text per `docs/02-git-workflow.md` §1.2.
- Read-only inspection is always allowed.

## Constraints (STRICT)

- Do NOT create or modify a readiness report — the WP is not being closed.
- Do NOT change the WP's `Status` from `active` to anything else.
- Do NOT update the milestone README's work-packages row to `closed` — it stays at `active`.
- Do NOT touch `CHANGELOG.md`.
- Do NOT trigger the two-PR closure pattern (`docs/03-planning-model.md` §9.1) — that pattern is for work-package close only.
- Do NOT introduce scope expansion, new features beyond the completed task subset, or refactors outside the task scope.
- Do NOT commit WIP / per-task / "checkpoint" / "tmp" commits (`docs/02-git-workflow.md` §5).
- Do NOT mark a task `done` without acceptance verification.

## Update Rules

- Make minimal, precise edits — task-file `Tx` status fields, task-file Notes (resume pointer), and `CLAUDE.md` next-step pointer.
- Preserve existing structure and formatting.
- The body of the task file (Purpose, Scope, Acceptance criteria, Tx descriptions) is NOT edited.

## Output (MANDATORY)

Provide a concise report:

1. **Tasks closed this session** — list with one-line acceptance evidence per task (file path, test, commit, or artifact).
2. **Tasks reverted** (if any) — any `done` mark backed out due to missing acceptance evidence, with reason.
3. **Resume pointer** — exact `Tx` the next session should pick up from; cite the Notes-section line.
4. **Files updated** — list each file with a one-line description.
5. **Files NOT modified** — paths inspected and intentionally left alone (auditable byte-stability claim).
6. **Suggested commit draft** — Conventional Commits subject + body in a single fenced block per `docs/02-git-workflow.md` §4.1, plus the recommended staging set.
7. **Cold-start invariant** — confirmed (with expected orientation fields) or flagged.

## Goal

After this step, a fresh Claude Code session can pick up `Project state: wp-executing`, read the resume pointer, and continue implementation from `Tx` with no chat-history dependency.

Keep the response concise, structured, and high-signal.
