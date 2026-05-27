# Workflows — Navo

This file defines **standard operational workflows** for interacting with AI agents (ChatGPT, Claude, optional auditor) during the project lifecycle.

The goal:
- Reduce ambiguity
- Prevent scope creep
- Ensure consistent execution quality
- Make workflows reusable across sessions

---

## 1. Planning Workflow

### Purpose
To transform a work package into a **fully specified, executable plan** before any implementation begins.

### Steps

1. Ensure onboarding is complete
   - Use `<scope>.project-state.md` as the general ChatGPT project-state resource

2. Request planning (Claude)
   - Ask for full task breakdown (T1…TN) — or scope + acceptance only for smaller WPs
   - Require: Description / Boundary / Acceptance / Notes (or scope + acceptance + optional notes)
   - Deliverable is a task file at the canonical path defined in `context/tasks/README.md` — not chat output. If a placeholder or partial outline exists at the target path, Claude must read it first and flag any conflict with the planning direction before drafting further
   - Require: Doctrine touchpoints section filled per the task-file template (architecture / testing strategy / engineering practices); `None applicable` and `N/A — doctrine scaffold-only` are valid per domain where genuinely empty

3. Review output (ChatGPT)
   - Validate:
     - scope alignment
     - constraint adherence
     - missing decisions
     - doctrine touchpoints declared per domain (architecture / testing strategy / engineering practices)

4. Resolve open questions
   - Only **micro-decisions**
   - No redesign

5. Freeze planning
   - Explicit confirmation required
   - No implementation yet
   - Do NOT authorize execution; wait for explicit user approval after review
   - To close the planning session cleanly after freeze (handing off to a fresh implementation session), use `chat/prompts/execution/planning-session-close.md` — verifies the frozen task file, updates the milestone README and `CLAUDE.md` next-step, and prepares a logical-unit commit. Mid-draft pauses (no freeze yet) do NOT need a special prompt — the next session resumes in `wp-drafting`.

---

## 2. Implementation Workflow

### Purpose
To execute a work package **without discovery**, only following the frozen plan.

### Steps

1. Confirm planning is frozen
2. Instruct Claude:
   - Execute the FULL work package (not task-by-task unless explicitly requested)
   - Follow task order + dependency graph strictly
   - Maintain a clean baseline (tests passing at each logical checkpoint)
   - Restate the WP's declared Doctrine touchpoints at execution start; if implementation drifts to an undeclared touchpoint, pause and surface as a doctrine-touchpoint deviation

3. No mid-work-package changes
   - No scope expansion
   - No architectural decisions
   - No package installation, dependency introduction, infrastructure file creation, or environment setup changes without explicit approval

4. Completion report required:
   - What was implemented
   - Test results
   - Any deviations
   - Per-touchpoint outcomes (validated / deviated touchpoint mismatch / incidental violation / not exercised / N/A — doctrine scaffold-only)
   - For full WP completion, follow §5 (Work Package Close) and use `chat/prompts/execution/work-package-close.md`
   - For partial completion (a clean subset of tasks done; WP remains active), close the session cleanly with `chat/prompts/execution/implementation-session-close.md` — verifies acceptance for each closed task, records a durable resume pointer in the task file Notes, updates `CLAUDE.md` next-step, and prepares a logical-unit commit (per `docs/02-git-workflow.md` §5 — no WIP / per-task commits). Resume from the next unfinished task in a fresh Claude session.

5. Git operation ownership: follow `docs/02-git-workflow.md` §1.1
   - ChatGPT may help draft branch names, Conventional Commit subjects/bodies, and PR titles/bodies
   - Claude's execution boundary (draft only vs. also execute) is set by §1.1's configured track — ChatGPT does not authorize Claude to override it

---

## 3. Audit Workflow

### Purpose
To validate that implementation strictly matches planning and constraints.

### Steps

1. Run independent audit (auditor / second AI tool)
2. Compare:
   - Acceptance criteria vs implementation
   - Constraints vs actual behavior
   - Declared Doctrine touchpoints vs actual diff (declared-first validation; then check for incidental violations against undeclared doctrine areas)

3. Identify:
   - gaps
   - risks
   - violations
   - doctrine-touchpoint deviations (declared touchpoint mismatch)
   - incidental doctrine violations (work touched undeclared doctrine area)

4. Fix only:
   - objective gaps (acceptance, correctness, constraints)
   - NOT improvements, optimizations, or redesign

5. Re-run audit if needed

---

## 4. Refinement Workflow

### Purpose
To handle **minor corrections** without reopening planning.

### Steps

1. Collect findings (audit / observation)
2. Classify:
   - bug
   - missing test
   - acceptance mismatch

3. Apply targeted fix
   - No redesign
   - No scope expansion

4. Re-validate

---

## 5. Work Package Close Workflow

### Purpose
To finalize a work package and prepare for the next one.

### Steps

1. Confirm:
   - All acceptance criteria met
   - Tests/build/lint clean (where applicable)

2. Update (Claude, project-side):
   - The work-package task file (closure metadata), the matching readiness report, and the milestone README's work-packages table
   - Readiness report §5 Doctrine validation — §5a Touchpoint validation outcomes; §5b Proposed doctrine deltas (non-blocking by default; `gating` flag for corrective changes that must land before closure)
   - The `CLAUDE.md` dispatcher next-step pointer, when reality has moved
   - `CHANGELOG.md` only when a version-tag boundary applies under the Git workflow
   - The closure-metadata pattern (closing PR with placeholders, then sign-off patch PR) is documented in `docs/03-planning-model.md`; details belong there, not here

3. Update (ChatGPT-side resource):
   - `<scope>.project-state.md` — incremental updates only when general project state changes; do not rewrite the full file

4. Ensure:
   - cold-start readiness (no chat dependency)
   - consistency across all discovery paths
   - any proposed doctrine deltas routed via readiness §5b are surfaced for product-owner decision; `gating` deltas resolve before sign-off

5. Return a concise work-package-close report for review (no assumptions of completion)
   - Files updated, summary of changes, outstanding issues
   - Doctrine validation summary — per-touchpoint outcomes from readiness §5a; proposed doctrine deltas from §5b (each: target doctrine doc, topic, proposed change, rationale, `gating` flag)
   - Closing PR draft (title in Conventional Commits format + body + recommended staging set if not obvious) — when operating under the per-WP-PR default; under the conditional milestone-branch policy at `docs/02-git-workflow.md §14`, replace with a closing-commit subject + body on the milestone branch (per-WP work does not get its own PR; only the milestone-close PR merges back to `main`)
   - Sign-off patch follow-up note (whether one is needed and what placeholders the closing PR's readiness report should carry) — under the milestone-branch policy, the placeholders are filled by a follow-up commit on the milestone branch

---

## 6. Next Work Package Workflow

### Purpose
To safely transition into the next work package.

### Steps

1. Start new conversation(s) with clean context

2. Onboard each AI:
   - Claude (project-side cold-start anchors): `CLAUDE.md` (dispatcher) → active milestone README → relevant readiness report
   - ChatGPT (resource-side onboarding): `<scope>.project-state.md`, `communication.general.md`, `communication.claude.md`, `playbooks.workflows.md` (this file), `doctrine.architecture.md`, `doctrine.testing-strategy.md`, `doctrine.engineering-practices.md`

3. Confirm understanding before any planning or execution

4. Confirm or choose the next work package via planning conversation (when a milestone roadmap names it, the conversation freezes the named WP rather than picking from candidates). Then start the Planning Workflow (do not auto-start)

---

## 7. Design Workflow

Note:
- Design workflow is supported but optional.
- Activate the design track only when a project has explicitly entered a design phase.
- Until then, design-track scaffolds in `context/design/` remain placeholders; do not use them as if they were active.

### Purpose
To develop product/design direction without mixing it with implementation planning or code execution.

### Steps

1. Start a design-oriented session explicitly
   - Claude Code sessions should follow the repository design-mode router
   - Entry point: `context/design/design-overview.md`
   - For current design-track status / cold-start alignment, read latest `<scope>.design-status.md` when available

2. Work from the design layer
   - Use `context/design/` as the design working-knowledge area
   - Treat design docs as product/design context, not implementation authority
   - Treat `<scope>.design-status.md` as a pointer-only status index; canonical design owners remain in `context/design/`

3. Resolve design pre-flight before visual generation
   - Brand identity
   - UI language stance
   - Key visual / design-system blockers

4. Keep implementation separate
   - Do not create code, tasks, ADRs, or docs amendments from design work without explicit promotion
   - Promote design decisions only through the normal workflow when implementation becomes relevant

5. Review before generation
   - Do not start visual design or design-system generation until the relevant pre-flight decisions are reviewed

---

## Global Rules

- Planning → then Implementation (never mix)
- Design → then visual generation / implementation promotion (never mix)
- Use `<scope>.design-status.md` only for design-oriented status / cold-start alignment, not as a development tracker
- No execution without explicit user approval
- Git operation ownership is governed by `docs/02-git-workflow.md` §1.1; ChatGPT may help draft branch names, Conventional Commit subjects/bodies, and PR titles/bodies, but does not authorize Claude to execute Git writes — that authority comes from the user under §1.1's configured track
- No dependency, package, infrastructure, or environment changes without explicit approval
- No scope creep
- No silent decisions
- No auto-freeze or auto-start
- Always review before execution
- Keep prompts concise, context-driven
- Prefer guidance over directives when communicating with Claude
