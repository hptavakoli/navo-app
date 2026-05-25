# Claude: Planning-session close (mid-WP, post-freeze handoff)

## For the user

- **What this is:** the prompt sent to Claude Code at the end of a planning session, after the work-package task file has been frozen, to verify the planning output is consistent and the project state is ready for a fresh implementation session.
- **When to use:** at the end of a planning conversation where the task file's `Frozen:` date has just been set and you want to close the current session before starting implementation in a new Claude session.
- **Target AI:** Claude Code.
- **Input:** none — Claude inspects the active WP's task file and the surrounding cold-start anchors.
- **Output:** structured report (task-file consistency, files updated / NOT modified, suggested commit, cold-start invariant, next-session pointer).
- **Not for:**
  - a work package that is fully complete → use [`work-package-close.md`](work-package-close.md);
  - a planning session paused BEFORE freeze → no special prompt needed; the next session resumes in `wp-drafting`.

--- COPY FROM BELOW ---

You have completed planning for this work package and its task file is frozen. The next step is to **close this planning session cleanly** so a fresh Claude Code session can begin implementation without depending on chat history.

This is a **planning-handoff checkpoint**, not a work-package closure. The work package itself remains active for implementation — it is not being closed.

## Pre-flight: freeze gate

Open the active work-package task file. If `Frozen:` is BLANK in the frontmatter, STOP. Report:

> *"This prompt is for post-freeze planning-session close only. The task file has no `Frozen:` date. Freeze the plan first (or, if you want to pause planning mid-draft, simply end the session — the next session will resume in `wp-drafting`)."*

Do NOT proceed with any of the responsibilities below until `Frozen:` is set.

## Objective

Verify the planning output is internally consistent, the milestone README reflects the WP at `Status: planned`, and `CLAUDE.md` points at implementation in a fresh session. Leave the repository ready for a cold-start orientation that reaches `Project state: wp-frozen`.

## Responsibilities

### 1. Task-file consistency check

- Frontmatter: `Status: planned`, `Planned:` set, `Frozen:` set. Confirm.
- Body sections present and non-empty: `Purpose`, `Scope` (with `In scope` / `Out of scope`), `Acceptance criteria`.
- If a `Task list` (Tx) is present: all tasks are `pending`. None `in progress`, none `done`. The planning conversation freezes the plan; execution has not started.
- Internal consistency: each acceptance criterion is observable; each `Tx` (if present) advances one or more acceptance criteria.

If any check fails, flag explicitly in the report. Do NOT silently correct body content authored during planning.

### 2. Milestone README alignment

- The milestone README's work-packages table shows this WP with `Status: planned` and the correct slug.
- If the row says `roadmap (not frozen)` or any other status, update it to `planned` (per `docs/03-planning-model.md` §9).
- If the row is missing, ADD it.
- Do NOT touch any other WP rows in the table.

### 3. Dispatcher next-step update

Update `CLAUDE.md` "Recommended next step for a fresh session" to point at implementation of this WP in a new Claude Code session. Suggested phrasing:

> *"Execute work package `<milestone-slug>/<NN-wp-slug>` in a fresh Claude Code session via `chat/prompts/workflows/claude-onboarding.md` → cold-start orientation report should report `Project state: wp-frozen` → `chat/prompts/workflows/claude-implementation-bridge.md`."*

Replace the prior next-step entry; do not append.

### 4. Cold-start invariant verification

A fresh Claude Code session reading `CLAUDE.md` → active milestone README → this task file MUST be able to produce a cold-start orientation report with `Project state: wp-frozen` and `Next work package: <slug>`. Trace the chain mentally and flag any gap.

### 5. Git handling

- Suggested commit: a logical-unit commit of the frozen task file plus the milestone README work-packages row update plus the `CLAUDE.md` next-step update.
- Type: `docs(planning):` (use `chore(planning):` only if the project prefers `chore` for planning-only changes).
- Subject example: `docs(planning): freeze <milestone-slug>/<NN-wp-slug> task file`.
- Body: 1–3 lines summarizing the freeze and pointing at the task-file path.
- Whether Claude executes the commit is governed by `docs/02-git-workflow.md` §1.1 (operation ownership); always draft the message and recommended staging set.
- Conventional Commits format per `docs/02-git-workflow.md` §4; presentation per §4.1 (single fenced block).
- No AI attribution in any GitHub-facing text per `docs/02-git-workflow.md` §1.2.
- Read-only inspection (`git status`, `git log`, `git diff`) is always allowed.

## Constraints (STRICT)

- Do NOT create or modify a readiness report — the WP is not being closed.
- Do NOT change the WP's `Status` to `active` or `closed`.
- Do NOT mark any `Tx` task as `done` or `in progress`.
- Do NOT touch `CHANGELOG.md`.
- Do NOT trigger the two-PR closure pattern (`docs/03-planning-model.md` §9.1) — that pattern is for work-package close only.
- Do NOT update the milestone README's status / closed-date fields.
- Do NOT rewrite the task file's body content authored during planning; only verify consistency and report findings.
- Do NOT introduce implementation, scope expansion, or refactors.

## Update Rules

- Make minimal, precise edits — only the milestone README work-packages row and `CLAUDE.md` next-step pointer.
- Preserve existing structure and formatting.

## Output (MANDATORY)

Provide a concise report:

1. **Task-file consistency** — confirmed, or a list of flagged issues.
2. **Files updated** — list each file with a one-line description.
3. **Files NOT modified** — paths inspected and intentionally left alone (auditable byte-stability claim).
4. **Suggested commit draft** — Conventional Commits subject + body in a single fenced block per `docs/02-git-workflow.md` §4.1, plus the recommended staging set.
5. **Cold-start invariant** — confirmed (with the expected orientation fields) or flagged.
6. **Next session** — one line: how the user should start the implementation session (cold-start onboarding → implementation bridge).

## Goal

After this step, a fresh Claude Code session can pick up `Project state: wp-frozen` and proceed directly to implementation via the implementation bridge, with no chat-history dependency.

Keep the response concise, structured, and high-signal.
