# Readiness reports

Per-work-package closure evidence and audit records. One folder per work package; the report itself is `readiness-report.md` inside that folder.

> For the planning model, milestone structure, and discovery map see [`../../docs/03-planning-model.md`](../../docs/03-planning-model.md).
> For Git workflow and changelog conventions see [`../../docs/02-git-workflow.md`](../../docs/02-git-workflow.md).

This file is **not** auto-loaded into every session. It is read on demand when authoring or reviewing a readiness report.

---

## When to write one

Write a readiness report when closing a work package. The work package's `Status` becomes `closed` only after the readiness report is signed off.

A readiness report is **frozen evidence**: once signed off, do not edit it. Carry-forwards, errata, or follow-up findings go in the next work package's readiness report or in a separate doc.

---

## Filesystem location

```
context/sanity-checks/
  <NN>-<milestone-slug>/
    NN-<work-package-slug>/
      readiness-report.md
      # plus per-package evidence as needed: smoke logs, result files, etc.
```

The folder name must match the work-package task-file name without the `.md` suffix. The coupling rule ([`03-planning-model.md` §11](../../docs/03-planning-model.md)) requires renames to happen in the same PR.

---

## Required sections

A readiness report covers, at minimum:

1. **Header**
   - Work package: name + slug
   - Milestone: slug
   - Status: `closed` (or `closed PASS` / `closed FAIL` if explicit PASS/FAIL is being recorded)
   - Closed: ISO date
   - Sign-off: who signed (product owner / engineering partner)
   - Links: to the task file and the milestone README

2. **Summary** — 1–3 paragraphs. What was delivered, headline outcome, any notable empirical evidence.

3. **Final module / artifact inventory** — what files were created, modified, and (if applicable) verified byte-identical. Make scope visible at a glance.

4. **Per-task outcomes** — table or bulleted list mapping each task in the task file (or each acceptance criterion if the task file is lightweight) to PASS / FAIL / N/A with a short note. Reference specific files, commits, or evidence artifacts.

5. **Working-rule frictions** — places where existing rules ([`../../docs/01-working-rules.md`](../../docs/01-working-rules.md), [`../../docs/02-git-workflow.md`](../../docs/02-git-workflow.md), [`../../docs/03-planning-model.md`](../../docs/03-planning-model.md), or ADRs) created friction during execution. Empty section is fine; do not omit the heading.

6. **Forward-looking flags / carry-forwards** — observations relevant to future work: deferred items, open questions reaffirmed, new risks surfaced. Each flag is a candidate input to a future planning conversation.

7. **Sign-off checklist** — a checked list confirming the task-file acceptance criteria are met, each marked complete with a short note or evidence link.

---

## Optional sections (use when applicable)

- **Walkthrough / smoke checklists with PASS/FAIL outcomes** — for work that includes a manual or automated demo.
- **Spend record** — for paid-provider work, dashboard-confirmed cost delta against the relevant cap.
- **Dependency-manifest delta** — for work that adds, removes, or pins a dependency.
- **Provider-seam discipline** — for work that touches an adapter boundary.
- **Byte-identity verification of prior artifacts** — when the work package is required to leave earlier artifacts unchanged.
- **Evidence index** — quick links to logs, result files, screenshots, or other per-package evidence in the same folder.
- **CLAUDE.md update checklist** — when the work changes anything `CLAUDE.md` references.
- **Appendix** — raw outputs that don't fit the body but are worth preserving.

---

## Writing guide

- **Be concrete.** Reference specific files, commits, and outputs by path or hash. "PASS — see `smoke-mock.log` row 12" is more useful than "PASS".
- **Be honest.** A readiness report is audit evidence; "FAIL" or "N/A with reason" is preferable to a soft PASS.
- **Be terse.** Three lines per task outcome is usually enough. Long narrative belongs in an appendix.
- **Cross-reference, don't restate.** Link to the task file, ADRs, working rules, etc. Do not duplicate their content.

---

## Relationship to other artifacts

| Artifact | Purpose | Mutability |
|---|---|---|
| Task file | Frozen plan + acceptance criteria | Frozen at planning conversation |
| **Readiness report (this template)** | Per-work-package closure evidence | Frozen at sign-off |
| PR body | Summary + verification for the merge unit | Editable on PR |
| `CHANGELOG.md` entry | Curated per-version summary across work packages | Editable across versions |
| GitHub Release | Human-readable release page | Editable |

The readiness report owns the audit detail. The CHANGELOG entry summarizes across multiple work packages and is not the audit detail.
