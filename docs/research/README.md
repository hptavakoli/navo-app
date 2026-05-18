# Research and synthesis

**Optional folder.** Activate when the project's early-stage workflow includes evidence gathering before technical commitments.

> Research documents are *evidence*. Synthesis documents are *consolidated direction*. ADRs are *binding decisions*. The three layers are distinct and the rules are different.

---

## When to use this folder

Use research/synthesis when:

- A foundation-level decision (engine, storage class, framework, deployment shape) requires comparing several candidates with non-trivial tradeoffs.
- The project is in early scoping and the technical direction is not yet committed.
- The user wants a durable record of *why* a direction was chosen, beyond what fits in an ADR's "Alternatives considered" section.

Skip this folder when:

- The project is small / single-purpose and direction is obvious.
- All technical commitments are routine.
- A single ADR can carry the full rationale.

---

## Workflow

```
open question  →  topical research  →  synthesis (versioned)  →  ADR  →  03-technical-direction.md / 08-scope.md
```

1. **Open question** identified during scoping (e.g., "which engine should we use?").
2. **Topical research document** at `docs/research/<NN>-<topic>.md` — surveys candidates, evidence, tradeoffs.
3. **Synthesis** at `docs/research/v1-<focus>-direction.md` — consolidates one or more research docs into a recommended direction.
4. **ADR** in [`../decisions/`](../decisions/) records the binding choice; references the synthesis.
5. **`03-technical-direction.md`** carries the committed direction at narrative level; cites the ADR.

---

## Discipline rules

- **Research documents are immutable once cited by a synthesis.** A research doc is evidence frozen at the time of citation. Errata go in a *new* document, not a retroactive edit. The new doc references the prior one.
- **Syntheses are versioned.** New evidence or revised direction → new synthesis (`v2-...md`, `v3-...md`). Old syntheses stay in place with a `Superseded by:` pointer at the top.
- **Research is not authority.** A research finding is a signal, not a binding rule. Binding happens through ADRs and scope documents.
- **Synthesis ≠ ADR.** A synthesis recommends; an ADR commits. Synthesis can shift; an ADR's status moves only Proposed → Accepted → Superseded with explicit approval.

---

## File naming

| Pattern | Purpose |
|---|---|
| `<NN>-<topic>.md` | Topical research (e.g., `01-engine-options.md`, `02-storage-options.md`) — `01..NN` numeric prefix; append-only, no renumbering |
| `v<N>-<focus>-direction.md` | Synthesis (e.g., `v1-foundation-direction.md`); version prefix is part of the filename |
| `_topic-template.md` | Scaffold for a new research topic |
| `_synthesis-template.md` | Scaffold for a new synthesis |

The leading underscore on template files keeps them sorted to the top of filesystem listings and signals "scaffold, not content."

---

## Cross-references

- [`../01-working-rules.md` §3](../01-working-rules.md) — research/synthesis discipline as a working rule.
- [`../07-technical-direction.md`](../07-technical-direction.md) — receives the committed direction from synthesis.
- [`../decisions/`](../decisions/) — receives the binding decisions.
