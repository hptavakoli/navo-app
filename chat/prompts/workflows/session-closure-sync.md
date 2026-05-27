# Claude: Session-closure sync

## For the user

- **What this is:** the prompt sent to Claude Code at the end of a working session to verify that all cold-start anchors (`CLAUDE.md`, milestone README, ChatGPT-side resources, `CHANGELOG.md`) reflect post-merge reality with no stale references.
- **When to use:** after one or more PRs have merged, an ADR has been recorded, a workflow rule has changed, or any other event that affects what a fresh cold-start session should know.
- **Target AI:** Claude Code.
- **Input:** none — Claude inspects repo state and recent session changes.
- **Output:** structured review proposal (files modified, files inspected and intentionally untouched, summary of changes, suggested commit / PR draft, manual-step reminders).

--- COPY FROM BELOW ---

This is a documentation and consistency synchronization pass — verify that all cold-start anchors accurately reflect post-merge reality.

This is **not** a planning, implementation, or work-package-closure step. It is a review-and-sync pass.

## What to do

1. Inspect the current repo state (`git log`, recent merges, current branch, status). Confirm what has landed since the last sync.
2. Read each cold-start anchor and identify stale or out-of-date passages:
   - `CLAUDE.md` — opening paragraph, "Recommended next step," authoritative-document references.
   - The active milestone README — work-package status table, roadmap shape, ADR pointers.
   - `chat/resources/app.project-state.md` — Current Status, Next Action, Progress Log (append-only), authoritative-documents pointer list.
   - `chat/resources/playbooks.workflows.md`, `communication.general.md`, `communication.claude.md` — only if a workflow rule or communication rule has changed.
   - `chat/resources/doctrine.architecture.md`, `chat/resources/doctrine.testing-strategy.md`, `chat/resources/doctrine.engineering-practices.md` — only if project-side doctrine docs (`docs/09-architecture.md`, `docs/10-testing-strategy.md`, `docs/11-engineering-practices.md`) have shifted in a way that affects ChatGPT-side reasoning (see step 3 for the drift check).
   - `chat/README.md` — only if the live-repo authoritative-documents list has shifted.
   - `CHANGELOG.md` — append a curated `[Unreleased]` entry for the sync pass if it shipped meaningful workflow / documentation changes.
3. **Doctrine resource sync + doctrine drift check.** Compare `chat/resources/doctrine.architecture.md`, `chat/resources/doctrine.testing-strategy.md`, and `chat/resources/doctrine.engineering-practices.md` (ChatGPT-side reasoning rules) against the project-side doctrine docs `docs/09-architecture.md`, `docs/10-testing-strategy.md`, `docs/11-engineering-practices.md`. If a project-side doctrine has shifted in a way that affects how ChatGPT should reason (topic status changes, new topics that warrant ChatGPT-side guidance, evolution-rule wording changes), surface a proposed update to the corresponding `chat/resources/doctrine.*.md` resource. Doctrine drift surfaces here as a proposal for product-owner approval; do NOT silently rewrite the ChatGPT-side resources. The canonical doctrine-evolution rules in `docs/09-architecture.md` §4 govern.
4. Apply minimal, targeted edits. Preserve structure and prior content.
5. Verify byte-stability of unrelated paths (no accidental edits to runtime code, frozen task files, frozen readiness reports, or unrelated docs).
6. Surface a complete review proposal for the user.

## Constraints (STRICT)

- Do NOT introduce new features.
- Do NOT perform planning or implementation.
- Do NOT expand scope.
- Do NOT redesign any part of the system.
- Do NOT edit frozen artifacts (frozen task files, frozen readiness reports, prior milestone closures).
- Do NOT renumber or rename work packages or milestones.
- Git operation execution follows `docs/02-git-workflow.md` §1.1 — only execute Git writes if the project's configured track permits.

## Update Rules

- Make minimal, precise changes.
- Preserve existing structure and formatting.
- Append-only for `Progress Log`.
- Replace whole blocks only when all items are obsolete.

## Output (MANDATORY)

Provide a concise, structured report:

1. **Files modified** — table or bulleted list, with a one-line description per file.
2. **Files NOT modified** — explicitly list paths that were inspected and intentionally left alone, with a short reason. This makes the byte-stability claim auditable.
3. **Summary of changes** — what was synced and why (1–3 paragraphs).
4. **Outstanding ambiguities** (if any) — questions to resolve before the user merges.
5. **Suggested commit / PR draft** — Conventional Commits subject + body (presented per `docs/02-git-workflow.md` §4.1) + recommended staging set if not obvious. Suggested PR title + body. Whether to execute the Git operation follows `docs/02-git-workflow.md` §1.1 (operation ownership). Do NOT include AI attribution in the message — see `docs/02-git-workflow.md` §1.2.
6. **Reminder for the user** — manual steps after merge (e.g., upload `chat/resources/*.md` files to ChatGPT's resource folder if any of those changed).

## Strict prohibitions

You MUST NOT:
- Rewrite documents or change their structure unnecessarily.
- Modify or reorder previous Progress Log entries.
- Add interpretations, decisions, or commentary inside the synced files.
- Execute Git write operations outside what `docs/02-git-workflow.md` §1.1 permits in the project's configured track.

## Goal

Leave the project in a state where a fresh cold-start session reading `CLAUDE.md` → milestone README → ChatGPT resources gets a coherent, current picture — without needing the conversation history that produced the latest changes.
