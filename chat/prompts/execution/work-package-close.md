# Claude: Work-package closure sync

## For the user

- **What this is:** the prompt sent to Claude Code after a work package's substantive work is complete, to synchronize project documentation and state with reality before the closing PR.
- **When to use:** at the end of a work package, after acceptance criteria are met and before opening the closing PR.
- **Target AI:** Claude Code.
- **Input:** none — Claude inspects the active work package and the surrounding cold-start anchors.
- **Output:** structured report (files updated, summary of changes, outstanding issues, closing PR draft, sign-off-patch follow-up note).

--- COPY FROM BELOW ---

You have completed this work package. The next step is to **synchronize the project documentation and state** to reflect current reality. This step is a **synchronization checkpoint**, not a planning or implementation task.

## Objective

Ensure that all relevant project documentation is consistent, up to date, and aligned with the actual progress.

## Responsibilities

### 1. Consistency Check

Review and verify that:
- The work-package task file reflects the final outcome (Status, all `Tx` task notes, any frozen-state metadata)
- The work-package readiness report exists and is internally consistent (sign-off block, evidence inventory, working-rule frictions, forward-looking flags)
- The milestone README's work-packages table reflects current status
- `CLAUDE.md` next-step pointer matches reality
- `CHANGELOG.md` has an `[Unreleased]` entry (or, at version-tag boundaries only, a curated section per the closing tag) — see `docs/02-git-workflow.md` §8
- Decisions, ADRs, and working rules referenced by the work package are still valid

If any mismatch is found: correct it (if safe), otherwise clearly flag it.

### 2. Documentation Sync

Update ONLY the relevant files:
- Any related context, decision, or governance files (if impacted)
- Follow the two-PR closure pattern documented in `docs/03-planning-model.md` §9.1 (closing PR with explicit placeholders, then a sign-off patch PR that fills them against the closing PR's actual merge commit)

Rules:
- Do NOT rewrite entire files
- Only update what is necessary
- Preserve existing structure and formatting

### 3. Memory Alignment

Ensure that all documentation reflects the current state so a new session (without chat history) can resume correctly via the cold-start anchors: `CLAUDE.md` (dispatcher) → active milestone README → relevant readiness report.

Avoid hidden assumptions or reliance on conversation context.

### 4. Drift Detection

If you detect inconsistencies between documentation and actual implementation / planning outcome: either resolve them (if safe) or explicitly report them.

### 5. Doctrine validation and deltas

Validate the work package's outcome against its declared `Doctrine touchpoints` (per the frozen task file). Non-blocking by default; surfaces deltas for product-owner decision unless explicitly marked `gating`.

**§5a Touchpoint validation.** For each declared touchpoint (architecture / testing strategy / engineering practices), record one of:
- `validated` — work conformed to the doctrine entry as currently stated.
- `deviated (touchpoint mismatch)` — work touched a doctrine area not declared in the task file.
- `incidental violation` — work conflicted with current doctrine without declaring the touchpoint.
- `not exercised` — touchpoint was declared but not exercised by the executed work.
- `N/A — doctrine scaffold-only` — the relevant doctrine entry is still a scaffold.

Mirror these outcomes into the readiness report's §5a Touchpoint validation section (per `context/sanity-checks/README.md` §5).

**§5b Proposed doctrine deltas.** Any proposed updates to `docs/09-architecture.md`, `docs/10-testing-strategy.md`, or `docs/11-engineering-practices.md` surfaced by this work — new topics, topic status changes (`pending → established`, etc.), wording refinements. Each proposal carries: target doctrine doc, topic, proposed change, rationale, `gating` flag (default non-blocking; mark `gating` only if a corrective change must land before closure is meaningful). Mirror into readiness report §5b. Apply approved deltas in a follow-up `docs:` commit per `docs/09-architecture.md` §4 — not in the closing PR unless explicitly gating.

### 6. Git Handling

Closure-time Git operations (creating the closing PR, the sign-off patch PR, any tag, any GitHub Release update) follow `docs/02-git-workflow.md` §1.1 (operation ownership). Whether Claude drafts only or also executes depends on the project's configured track.

For this closure step:
- Always draft the closing PR title (Conventional Commits format per `docs/02-git-workflow.md` §4 / §4.1) and the closing PR body for the user to paste — or to apply if §1.1's configured track permits Claude to execute.
- Indicate, where not obvious, which files should be staged into the closing PR.
- Note whether a sign-off patch PR will be required after the closing PR merges (per `docs/03-planning-model.md` §9.1) and prepare the placeholders the closing PR's readiness report should carry.
- Do NOT add AI attribution to any GitHub-facing text — commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries. See `docs/02-git-workflow.md` §1.2 (independent of operation ownership).
- Read-only inspection (`git status`, `git log`, `git diff`, `gh pr view`, etc.) is always allowed.

## Constraints (STRICT)

- Do NOT introduce new features
- Do NOT perform planning or implementation
- Do NOT expand scope
- Do NOT redesign any part of the system
- Do NOT edit frozen artifacts (frozen task files, frozen readiness reports)
- Do NOT renumber or rename work packages or milestones (see `docs/03-planning-model.md` §8 append-only insertion rule and §11 coupling rule)

This step is strictly for synchronization and validation.

## Update Rules

- Make minimal, precise changes
- Do NOT modify unrelated sections
- Do NOT remove valid historical context

## Output (MANDATORY)

Provide a concise report including:

1. **Files updated** — list each file modified.
2. **Summary of changes** — what was updated and why.
3. **Outstanding issues** (if any) — any ambiguity or unresolved mismatch.
4. **Doctrine validation summary** — per-touchpoint outcomes mirroring readiness report §5a (architecture / testing strategy / engineering practices), plus any proposed doctrine deltas mirroring readiness report §5b. For each proposed delta: target doctrine doc, topic, proposed change, rationale, `gating` flag. Non-blocking by default; only `gating` deltas block sign-off.
5. **Closing PR draft** (for the user to paste):
   - Title (Conventional Commits format)
   - Body (Summary, work-package context, test plan / verification, out-of-scope notes)
   - Recommended staging set (if not obvious)
6. **Sign-off patch follow-up** (if applicable):
   - Whether a sign-off patch PR is needed after the closing PR merges
   - Placeholders the closing PR's readiness report should carry (so the sign-off patch can fill them later)

## Goal

After this step: project documentation must fully represent the current state, and the system must be ready for a new session without relying on chat history.

Keep the response concise, structured, and high-signal.
