# Claude: Project state update

## For the user

- **What this is:** the prompt sent to Claude Code at the end of a session in which planning, implementation, work-package closure, or any other change with project-state implications has just occurred.
- **When to use:** at session end, when Claude already holds the full context of what changed in this session.
- **Target AI:** Claude Code.
- **Input:** none — Claude reads the file at `chat/resources/context.project-state.md` and applies updates from session context.
- **Output:** a brief structured report (file updated, summary of changes, intentionally preserved items, outstanding ambiguities). Updates land in the repo first; the user uploads / replaces the ChatGPT-side copy after merge.

--- COPY FROM BELOW ---

The repo-tracked source of truth for general project state lives at `chat/resources/context.project-state.md`. ChatGPT loads a manually-uploaded copy from its resource folder; the repo copy is the source of truth. Edits land in the repo first; the user uploads or replaces the ChatGPT-side copy after merge.

This file is NOT the design-track tracker. Detailed design-track progress belongs in `design-status.md`, not here.

## What to do

1. Read the file at `chat/resources/context.project-state.md`.
2. Identify the project-state-relevant changes from the current session context. Read-only repo inspection (`git log`, the active milestone README, recent readiness reports, etc.) is allowed when useful for verification.
3. Apply minimal, targeted updates per the rules below.
4. Return a brief structured report.

## Core rules (MANDATORY)

### 1. DO NOT rewrite the file
- Never regenerate the entire document
- Never change structure or section order
- Preserve all existing valid content; prefer modifying or extending over replacing

### 2. Allowed updates only

- `Current Status` — when work-package or milestone state changes
- `Next Action` — when the next user-visible move changes
- `Progress Log` — append only
- `Operational Constraints` — only when a new project-level rule applies (e.g., a new ownership or governance rule)
- `Key Decisions` — only when a new distilled decision applies

Everything else MUST be preserved as-is.

### 3. Append-only Progress Log (CRITICAL)
- Only ADD new entries
- Never delete, modify, or reorder previous entries
- Preserve chronological order
- Match the existing log style: one line per entry, `YYYY-MM-DD → concise factual description`

### 4. Minimal and precise
- No explanations, commentary, or interpretations inside the file
- No new sections
- Prefer line-level updates over block replacement
- Replace a whole block only when all items in it are obsolete

### 5. No silent assumptions
- If something is unclear, ask a targeted question or state assumptions explicitly
- Do NOT guess

### 6. Design-track status boundary
- Update this file only if the general project state, project-wide governance, or ChatGPT resource-reading rule changed
- Do NOT update this file for ordinary per-surface design audits, tracker row changes, design activity-log updates, or design next-action changes
- Do NOT edit `design-status.md` from this prompt
- If design-track status changed, the repo-side `context/design/design-status.md` should be updated through the normal design workflow, then uploaded or replaced in ChatGPT resources as latest `design-status.md`

## Git Handling

- Operation ownership: follow `docs/02-git-workflow.md` §1.1 (whether to draft only or also execute depends on the project's configured track).
- Always draft the commit subject + body (Conventional Commits format, presented per `docs/02-git-workflow.md` §4.1) and the PR title + body for the user to use or paste.
- Do NOT add AI attribution to any GitHub-facing text — commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries. See `docs/02-git-workflow.md` §1.2 (independent of operation ownership).

## Output (MANDATORY)

Provide a brief, structured report:

1. File updated (path)
2. Summary of changes — which sections were touched and why (concise)
3. Intentionally preserved — anything in the session context that you decided NOT to log, and why
4. Outstanding ambiguities (if any) — questions to resolve before the next update

## Strict prohibitions

You MUST NOT:
- Rewrite the document or change its structure
- Modify or reorder previous Progress Log entries
- Add interpretations, decisions, or commentary inside the file
- Edit `design-status.md`
- Duplicate detailed design-track status from `design-status.md`
- Use `context.project-state.md` as the design-track tracker
- Execute any Git write operations

## Goal

Maintain a stable, accurate, incremental general project memory for ChatGPT — edited by Claude Code under the user's review.
