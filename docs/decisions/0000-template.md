# ADR NNNN — [Title]

**Status:** Proposed | Accepted | Superseded
**Scope:** foundation | <phase> implementation | tooling | …
**Date:** YYYY-MM-DD
**Supersedes:** — (or `ADR XXXX`)
**Superseded by:** — (or `ADR YYYY`)

---

## Context

What background and constraints make this decision necessary? What forces are at play (technical, product, organizational, regulatory)? What documents inform it (research, prior ADRs, scope baselines)?

Keep this section honest about uncertainty: if a constraint is assumed rather than confirmed, say so.

---

## Decision

The decision itself, stated in one or two sentences first, then unpacked.

If the decision has multiple layers (engine, storage, deployment pattern, etc.), use a small table:

| Layer | Choice |
|---|---|
| ... | ... |

If the decision pins a sticky parameter (dimension, version, schema, etc.), note the rebuild cost of changing it.

---

## Consequences

### Positive

- ...
- ...

### Negative

- ...
- ...

### Neutral

- ...
- ...

---

## Alternatives considered

For each meaningful alternative, name it and record the **decisive reason** it was outranked. Avoid restating every comparison criterion; cite supporting research or prior ADRs.

- **Alternative A** — outranked on [reason].
- **Alternative B** — outranked on [reason].

If alternatives were disqualified by license, maintenance signal, or other gates, group them separately:

- **License-disqualified:** ...
- **Maintenance- or shape-disqualified:** ...

---

## Open questions before acceptance

If `Status: Proposed`, list the conditions for moving to `Accepted`. Each item should be concrete enough that the answer is observable.

1. ...
2. ...

These should not be technical-research gaps — they should be empirical confirmations or stakeholder sign-offs that resolve in finite time.

---

## References

- Internal: prior ADRs, research documents, scope baselines, readiness reports.
- External: vendor docs, license texts, case studies.
