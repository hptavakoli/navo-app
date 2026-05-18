# ChatGPT: Planning bridge — generate planning prompt for Claude

## For the user

- **What this is:** the prompt sent to ChatGPT after Claude has reported orientation. ChatGPT validates alignment, then drafts a clean planning prompt you'll send to Claude.
- **When to use:** between Claude's orientation report and Claude's planning conversation, when you want ChatGPT to generate the planning prompt.
- **Target AI:** ChatGPT.
- **Input:** Claude's orientation report — paste it into the marker block in the prompt body.
- **Output:** alignment check (Farsi, brief) plus a final English planning prompt for Claude in a Writing Block.

--- COPY FROM BELOW (after pasting Claude's orientation report into the marker block) ---

I have onboarded Claude Code in a new session and Claude has full access to the project code and documentation. Claude's orientation report is in the marker block below.

Your goal: **validate alignment**, then **generate a clean planning prompt for Claude** to start planning the next work package.

This template is for **development-mode planning only**. Do not use it for design-mode work, brand / identity work, visual exploration, or design-documentation refinement.

<<<CLAUDE_ORIENTATION_REPORT>>>
[Paste Claude's orientation report here, replacing this line.]
<<<END_CLAUDE_ORIENTATION_REPORT>>>

If the marker block is empty, do NOT proceed. Stop and tell the user the orientation report is missing.

## What to do

1. **State guard (BEFORE any alignment check):**
   - Read the orientation report's `Project state:` field.
   - If `Project state` is `milestone-open-no-wp`: continue to step 2.
   - If `Project state` is `milestone-pending`: STOP. Tell the user to use `claude-milestone-opening-bridge.md` first — a milestone must be opened before a work package can be planned.
   - If `Project state` is `wp-drafting`, `wp-frozen`, `wp-executing`, `wp-closing`, or `milestone-closing`: STOP. There is already a WP / closure in flight; tell the user which bridge or in-session continuation applies.
   - If `Project state` is `unknown`: STOP. Tell the user the orientation is ambiguous; have Claude re-orient with a discrete `Project state` value.
   - In every STOP case, do NOT generate any prompt for Claude.

2. **Check alignment (concise):**
   - Does Claude's understanding match the project state (purpose, current milestone / work-package status, next action)?
   - Flag only **real mismatches or ambiguities** (if any). Keep it short.

3. **If aligned (or after noting small issues), generate the prompt for Claude** following the sub-route logic in the next section.

## Requirements for the generated prompt

Follow the communication rules in project resources:
- `communication.general.md`
- `communication.claude.md`

Keep the prompt concise, non-directive (no assumptions of correctness), and scope-safe (no implementation).

Keep it development-mode only:
- do not treat design documents as implementation authority
- do not ask Claude to modify `context/design/` unless the work-package scope explicitly requires it
- do not start visual / design-system work or design handoff

### Sub-route selection (based on `Next work package` field)

- If `Next work package:` is a concrete slug → sub-route `wp-draft`: generate a prompt that asks Claude to draft the task file for that specific WP.
- If `Next work package:` is `undefined` → sub-route `wp-select-then-draft`: generate a prompt that asks Claude to (1) read the milestone README's `Work packages` table and `Roadmap shape`, (2) recommend the next WP slug to start with reasoning, (3) wait for user confirmation, then (4) draft the task file per the constraints below.

**Literal `{WORK_PACKAGE}` MUST NOT appear in the generated prompt.** If the slug is not concrete, route to `wp-select-then-draft`; never paper over with a placeholder.

### Common constraints (both sub-routes)

The prompt MUST:
- start from planning only (no coding)
- require the draft to be written as a file at the task-file path defined in `context/tasks/README.md` (no execution; the draft is an artifact on disk, not chat content)
- require adherence to `context/tasks/README.md` structure
- require Claude to first check whether a placeholder or partial outline already exists at the target path. If yes: read it, treat it as the starting point, and flag any conflict between its content and the planning direction — pausing for user confirmation **before** drafting further
- require `Status: planned` with `Frozen:` left blank, signalling a pre-freeze draft pending review
- require Claude's chat output to be a brief summary plus the file path — no task-file content pasted inline

## Output format (STRICT)

### 1) Alignment Check (Farsi, very brief)
- 2–4 bullets max
- Only mismatches/notes (if any). If none, say it's aligned.

### 2) Prompt for Claude (English, final only)
- Provide **only the final prompt** to send to Claude.
- Place the prompt inside a Writing Block with `variant="chat_message"` so it can be easily copied.

## Prompt shape (guide)

The generated prompt should follow this structure (adapt as needed):

- Acknowledge orientation
- State the planning task: either drafting the task file for a named work-package slug (`wp-draft` sub-route), or selecting the next WP from the milestone roadmap and then drafting (`wp-select-then-draft` sub-route)
- Define goal: complete breakdown (tasks, boundaries, acceptance) once the WP slug is fixed
- Enforce task-file structure from `context/tasks/README.md`
- Explicitly forbid implementation
- Require the draft to be written to its task-file path (not pasted into chat); review happens on the file

## Do NOT

- Do NOT include long explanations
- Do NOT restate the whole project context
- Do NOT suggest redesigns
- Do NOT mix development-mode planning with design-mode work
- Do NOT include multiple prompt variants
- Do NOT instruct Claude to paste the task-file content into chat — the deliverable is a file at the path defined in `context/tasks/README.md`

## Goal

Produce a minimal, production-ready planning prompt the user can copy directly into Claude.
