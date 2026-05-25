# ChatGPT: Implementation bridge — generate implementation prompt for Claude

## For the user

- **What this is:** the prompt sent to ChatGPT after Claude has reported orientation and a work-package task file is frozen. ChatGPT validates alignment, then drafts a clean implementation prompt you'll send to Claude.
- **When to use:** between Claude's orientation report and Claude's implementation execution.
- **Target AI:** ChatGPT.
- **Input:** Claude's orientation report — paste it into the marker block in the prompt body.
- **Output:** alignment check (Farsi, brief) plus a final English implementation prompt for Claude delivered as a ChatGPT Canvas document (or equivalent editable single-document panel).

--- COPY FROM BELOW (after pasting Claude's orientation report into the marker block) ---

I have onboarded Claude Code in a new session and Claude has full access to the project code and documentation. Claude's orientation report is in the marker block below.

Your goal: **validate alignment**, then **generate a clean implementation prompt for Claude**.

This template is for **development-mode implementation only**. Do not use it for design-mode work, brand / identity work, visual exploration, or design-documentation refinement.

<<<CLAUDE_ORIENTATION_REPORT>>>
[Paste Claude's orientation report here, replacing this line.]
<<<END_CLAUDE_ORIENTATION_REPORT>>>

If the marker block is empty, do NOT proceed. Stop and tell the user the orientation report is missing.

## What to do

1. **State guard (BEFORE any alignment check):**
   - Read the orientation report's `Project state:` field.
   - If `Project state` is `wp-frozen`: continue to step 2 (normal start-of-execution case).
   - If `Project state` is `wp-executing`: continue to step 2 (resuming execution after a session break — task file already has `Status: active` from a prior session; this is distinct from `wp-frozen`, which is the start-of-execution case).
   - If `Project state` is `milestone-pending`, `milestone-open-no-wp`, or `wp-drafting`: STOP. There is no frozen task file to execute against; tell the user which bridge applies (milestone-opening or planning).
   - If `Project state` is `wp-closing` or `milestone-closing`: STOP. Closure is in progress; the implementation bridge does not apply — use `session-closure-sync.md`.
   - If `Project state` is `unknown`: STOP. Have Claude re-orient with a discrete `Project state` value.
   - In every STOP case, do NOT generate any prompt for Claude.

2. **Check alignment (concise):**
   - Does Claude's understanding match the project state (purpose, current milestone / work-package status, next action)?
   - Flag only real mismatches or ambiguities (if any). Keep it short.

3. **If aligned (or after noting small issues), generate the implementation prompt for Claude.**

## Requirements for the generated prompt

Follow communication rules:
- `communication.general.md`
- `communication.claude.md`

Keep the prompt concise, non-directive, scope-safe.

### Implementation Rules (CRITICAL)

The generated prompt MUST enforce:

1. **Execution Source of Truth** — Claude must implement strictly based on `context/tasks/{MILESTONE}/{WORK_PACKAGE}.md`. Do NOT redefine or reinterpret tasks.

2. **Phase Discipline** — No planning. No redesign. No new task creation.

3. **Scope Control** — No scope expansion. No future-work-package implementation.

4. **Architecture Safety** — Respect the active scope baseline (e.g., `docs/<NN>-scope.md`), `docs/01-working-rules.md`, and `docs/decisions/`. Avoid breaking changes.

5. **Execution Behavior** — Execute ALL tasks for the work package in one continuous flow (do NOT stop between tasks). Follow the correct implementation order as defined in the task file (not necessarily the listed order). Implement in a logical and controlled sequence based on dependencies. Keep changes minimal and targeted.

6. **Uncertainty Handling** — If something is unclear: do NOT guess. Explicitly flag it. Ask or report it.

7. **Failure Handling** — If blocked: stop execution. Report the blocker clearly.

8. **Reporting Requirement (MANDATORY)** — Claude MUST return a structured report including: what was implemented; what is incomplete (if any); blockers or uncertainties; any important notes.

9. **Git Handling**:
   - Operation ownership: follow `docs/02-git-workflow.md` §1.1 (whether Claude drafts only or also executes Git writes depends on the project's configured track).
   - Conventions: follow `docs/02-git-workflow.md` §2–§14 for branch naming, Conventional Commits, PR shape, merge strategy, tags, CHANGELOG, hotfix, spike, and milestone-branch policy.
   - DO NOT add AI attribution to any GitHub-facing text — commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries, GitHub comments. See `docs/02-git-workflow.md` §1.2 (this rule is independent of operation ownership).
   - Always draft Git output for the user: branch name, Conventional Commit subject + body (presented per `docs/02-git-workflow.md` §4.1), PR title + body, and (if not obvious) the staging set. Whether the same agent then executes the operation is governed by §1.1.
   - Read-only inspection (`git status`, `git log`, `git diff`, `gh pr view`, `gh api` GET, etc.) is always allowed, regardless of track.

## Output format (STRICT)

### 1) Alignment Check (Farsi, very brief)
- 2–4 bullets max
- Only mismatches/notes (if any). If none, say it's aligned.

### 2) Prompt for Claude (English, final only)
- Provide only the final prompt
- Deliver it as a ChatGPT Canvas document (or equivalent editable single-document panel) so the user can copy it cleanly; the canvas body must contain only the prompt itself — no preamble, no commentary

## Prompt shape (guide)

The generated prompt should:
- Acknowledge orientation
- State the implementation task using the concrete WP slug from the orientation report's `Next work package:` field. Literal `{WORK_PACKAGE}` MUST NOT appear in the generated prompt; the state guard ensures the slug is concrete (`wp-frozen` or `wp-executing` both imply a real WP).
- Define execution scope based on task file
- Enforce phase and scope constraints
- Define uncertainty and failure handling
- Require structured reporting at the end

## Do NOT

- Do NOT include long explanations
- Do NOT restate the full project context
- Do NOT suggest redesigns
- Do NOT mix implementation-mode execution with design-mode work
- Do NOT include multiple prompt variants

## Goal

Produce a minimal, clear, ready-for-execution implementation prompt the user can copy directly into Claude.
