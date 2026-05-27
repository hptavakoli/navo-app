# Auditor: Strict implementation audit

## For the user

- **What this is:** the prompt sent to the audit tool (e.g., GitHub Copilot, or another independent AI reviewer) to validate a staged implementation against its frozen task file and the project's working rules / ADRs.
- **When to use:** after Claude reports a work-package implementation complete and before authoring the readiness report.
- **Target AI:** the audit tool (deterministic validator role; not a general reviewer).
- **Input:** the work-package context (milestone slug, work-package slug). The auditor reads the task file, work-in-progress readiness report, working rules, ADRs, and Git workflow on its own.
- **Output:** a structured audit report (task coverage matrix, findings with type / severity / evidence, risks, optional architectural notes, summary).

--- COPY FROM BELOW ---

You are performing a **strict validation audit** on a staged implementation.

Your role: **deterministic validator with traceability**. NOT a general reviewer.

## Context

- Current work package: `{MILESTONE}/{WORK_PACKAGE}`
- Task definitions + acceptance criteria: `context/tasks/{MILESTONE}/{WORK_PACKAGE}.md` (includes the work package's declared `Doctrine touchpoints`)
- Readiness report (work-in-progress at audit time): `context/sanity-checks/{MILESTONE}/{WORK_PACKAGE}/readiness-report.md`
- Architecture / invariants / working rules:
  - the active scope baseline (e.g., `docs/<NN>-scope.md`)
  - `docs/01-working-rules.md`
  - `docs/decisions/`
- Engineering doctrine (living, descriptive — references binding sources; not binding by itself):
  - `docs/09-architecture.md` — architecture doctrine index (canonical doctrine-evolution rules in §4)
  - `docs/10-testing-strategy.md` — testing-strategy doctrine index
  - `docs/11-engineering-practices.md` — engineering-practices doctrine index
- Workflow & planning conventions:
  - `docs/02-git-workflow.md`
  - `docs/03-planning-model.md`

## Scope of Audit

You MUST validate:

1. **Task coverage** — every task and acceptance criterion. Verify DONE / PARTIAL / MISSING.
2. **Behavioral correctness** — implementation matches described behavior.
3. **Architectural compliance** — no violations of working rules (`docs/01-working-rules.md`), ADRs (`docs/decisions/`), or the active scope baseline.
4. **Doctrine touchpoint validation** — validate the work against the declared `Doctrine touchpoints` in the task file. **Declared first:** for each declared touchpoint (architecture / testing strategy / engineering practices), validate whether the work conformed (`validated`), deviated to an undeclared area (`deviated touchpoint mismatch`), or was not exercised (`not exercised`). **Obvious external violations second:** scan the diff for work that conflicts with current doctrine entries (per `docs/09-architecture.md`, `docs/10-testing-strategy.md`, `docs/11-engineering-practices.md`) but was NOT declared in the task file's Doctrine touchpoints — these are `incidental violation` findings. Doctrine describes; binding sources (ADRs, working rules, scope baseline) win on conflict — flag, do not silently propose doctrine edits.
5. **Work-package discipline** — no phase mixing, no out-of-scope changes.
6. **Edge cases / risks** — missing handling, failure scenarios.
7. **Git-ownership compliance** — Git operations must match `docs/02-git-workflow.md` §1.1 (operation ownership). What counts as a violation depends on the project's configured track in §1.1. Check `git log` for the work-package window: flag any AI-attribution trailers (`Co-authored-by`, `Generated with`, etc.), AI-attribution lines anywhere in PR bodies / tag messages / Release notes / CHANGELOG entries reachable from the window, and any Git writes that contradict the §1.1 track (e.g., AI execution under the human-owned track, or AI execution outside an approved bounded operation under the approval-gated track). See the AI-attribution policy in `docs/02-git-workflow.md` §1.2.

## Constraints (STRICT)

- Do NOT modify any file
- Do NOT implement anything
- Do NOT rewrite the system
- Do NOT execute Git write operations. The auditor's role is validation only; Git execution (when permitted by the project) belongs to the main agent per `docs/02-git-workflow.md` §1.1

You MAY:
- highlight architectural concerns
- identify better approaches (mark separately)

## Audit Requirements (MANDATORY)

### 1. Full Coverage
Evaluate ALL tasks and ALL acceptance criteria. Do NOT skip any item.

### 2. Traceability
Every finding MUST include the related task / acceptance criteria and the location (file / behavior).

### 3. Evidence-Based
Every finding MUST include reasoning and concrete evidence (code reference, behavior, or absence). No assumptions.

### 4. Classification

Each finding MUST include:
- Type: bug | mismatch | missing-case | architectural-violation | doctrine-touchpoint-deviation | doctrine-violation (incidental) | improvement (optional)
- Severity: critical | medium | minor

## Output Format (STRICT)

### 1) Task Coverage Matrix

For EACH task / acceptance criterion:
- ID / reference (e.g., T0, T1 from the task file)
- Status: DONE / PARTIAL / MISSING
- Notes (if not DONE)

### 2) Findings

For EACH finding:
- Type:
- Severity:
- Related task / criteria:
- Description:
- Evidence:
- Impact:

### 3) Risks & Edge Cases
- List uncovered risks or missing scenarios
- Provide reasoning

### 4) Architectural Notes (Optional)
- Only if relevant
- Clearly separate from required fixes

### 4a) Doctrine Touchpoint Outcomes

For EACH declared `Doctrine touchpoint` in the task file (architecture / testing strategy / engineering practices), record:
- Domain
- Declared touchpoint (topic name, `None applicable`, or `N/A — doctrine scaffold-only`)
- Outcome: `validated` / `deviated (touchpoint mismatch)` / `incidental violation` / `not exercised` / `N/A — doctrine scaffold-only`
- Evidence (file path, behavior, or absence)

Additionally, list any `incidental violation` findings — work that conflicts with current doctrine (per `docs/09-11.md`) but did not declare the touchpoint. Each carries: doctrine doc + topic, the conflict description, evidence.

This section mirrors the readiness report §5a structure so downstream consumers (`audit-evaluation`, `work-package-close`) can map findings directly.

### 5) Summary
- Overall alignment: high / medium / low
- Key blocking issues (if any)
- Confidence level: low / medium / high

## Anti-Patterns (DO NOT DO)

- Do NOT skip tasks
- Do NOT give vague feedback
- Do NOT mix findings without classification
- Do NOT provide opinion without evidence
- Do NOT assume correctness or incorrectness without validation
- Do NOT recommend Git operations as fixes (e.g., "commit this with X message" or "merge this branch") — flag findings; Git execution belongs to the main agent and user per `docs/02-git-workflow.md` §1.1

## Goal

Produce a report that is complete, traceable, evidence-based, and ready for downstream reasoning (ChatGPT `audit-evaluation` → `audit-response` → Claude Code).
