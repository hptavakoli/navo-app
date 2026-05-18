# ChatGPT: Audit evaluation

## For the user

- **What this is:** the prompt sent to ChatGPT to evaluate a completed work-package audit cycle by comparing Claude's implementation report against the auditor's findings.
- **When to use:** after both reports exist — Claude's implementation report (from the implementation session) and the auditor's audit report (from `audit-run.md`).
- **Target AI:** ChatGPT.
- **Input:** Claude's implementation report and the auditor's audit report — paste each into its marker block in the prompt body.
- **Output:** semi-structured evaluation in Farsi (key issues, risks, conflicts, false positives, unclear points, overall assessment). Does NOT generate a Claude response prompt — that is the next step (`audit-response.md`).

--- COPY FROM BELOW (after pasting both reports into their marker blocks) ---

You are evaluating a completed work-package implementation using two inputs: Claude's implementation report and the auditor's independent audit report. Both are in marker blocks below.

Your role: **technical evaluator + interpreter + decision support**. NOT a prompt generator.

<<<CLAUDE_IMPLEMENTATION_REPORT>>>
[Paste Claude's implementation report here, replacing this line.]
<<<END_CLAUDE_IMPLEMENTATION_REPORT>>>

<<<AUDIT_REPORT>>>
[Paste the auditor's audit report here, replacing this line.]
<<<END_AUDIT_REPORT>>>

If either marker block is empty, do NOT proceed. Stop and tell the user which input is missing.

## Context

- Use current conversation context.
- Use `app.project-state.md` as source of truth.
- Planning source: `context/tasks/{MILESTONE}/{WORK_PACKAGE}.md` (attached to this conversation).
- Readiness report (work-in-progress at audit time): `context/sanity-checks/{MILESTONE}/{WORK_PACKAGE}/readiness-report.md` (if available).

If critical information is missing, explicitly flag it. Do NOT assume.

## Objectives

You MUST:

1. **Compare Claude vs Audit** — identify agreements; identify conflicts.
2. **Validate findings** — which audit points are valid (evidence-based); which are likely false positives.
3. **Detect gaps** — missing implementation vs acceptance criteria; missing edge cases.
4. **Identify risks** — real forward-looking risks (not redesign suggestions).
5. **Evaluate Git-ownership and AI-attribution findings** — validate any audit findings about AI-tooling Git operations that conflict with `docs/02-git-workflow.md` §1.1 (operation ownership) — what counts as a violation depends on the project's configured track — and any AI-attribution lines in GitHub-facing text (commit messages, PR titles/bodies, tag annotations, Release notes, CHANGELOG entries, GitHub comments) per `docs/02-git-workflow.md` §1.2. Treat substantiated findings as confirmed issues, NOT as redesign suggestions.

## Rules (STRICT)

- Do NOT generate any prompt for Claude.
- Do NOT assume any side is correct.
- Do NOT guess when information is missing.
- If something is unclear: explicitly say it is unclear; suggest what needs clarification.
- Cover ALL meaningful audit findings; avoid skipping technical points; remove redundancy.

## Output Style

- Language: **Farsi**
- Keep explanations: simple, concise, understandable
- For technical concepts: explain in simple terms; provide example ONLY if needed (complex cases)
- Avoid: long essays, unnecessary repetition

## Output Structure (Semi-Structured)

### 1) Key Issues (Confirmed Problems)

For each issue:
- What the issue is
- Why it matters (simple Farsi explanation)
- Which side is more likely correct (if clear)
- If NOT clear → mark as "needs further investigation"

### 2) Risks / Edge Cases
- List real risks (non-blocking)
- Explain simply why they matter

### 3) Conflicts (Claude vs Audit)
- Highlight disagreements
- Explain both sides briefly
- Suggest what should be verified

### 4) False Positives (if any)
- Audit findings that are likely incorrect
- Explain why (simple reasoning)

### 5) Unclear / Needs Clarification
- List items where data is insufficient
- Suggest what to ask Claude

### 6) Overall Assessment
- Work-package status: ready / needs fixes
- Confidence: low / medium / high
- Short reasoning in simple Farsi

## Goal

Help the user understand technical findings, make informed decisions, and identify what to send back to Claude — without generating the final prompt.
