# Navo App — Scope

**Status:** Draft. This file is **renamed per phase** when scope is frozen — for example `08-poc-scope.md` for a Proof-of-Concept phase, `08-mvp-scope.md` for an MVP, etc. The number `08` is reserved for the active scope baseline; later phases extend with `09-...md`, `10-...md` if needed.

> Frozen scope baseline for the active phase. Distinct from the broader [`05-product-context.md`](05-product-context.md): this document records the **specific subset** of capability that the current phase commits to. It is authoritative and supersedes general direction docs for matters explicitly listed below.

---

## 1. Phase

> Name the phase this scope covers — the project's own phase name (e.g., "Proof of Concept", "Pilot", "Beta", or another phase label that fits this project). Date the freeze.

## 2. Inputs to this scope

> List the documents this scope is built on:
>
> - [`05-product-context.md`](05-product-context.md) — product framing
> - [`06-feature-blueprint.md`](06-feature-blueprint.md) — capability outline
> - [`07-technical-direction.md`](07-technical-direction.md) — technical commitments
> - [`research/`](research/) synthesis (if applicable)
> - Specific ADRs that bind into this scope

## 3. What this scope supersedes

> Where this document overrides general direction (e.g., "supersedes `07-technical-direction.md` §X.Y for the duration of this phase"). Be explicit; the supersession is part of the scope's authority.

## 4. Capabilities in scope

> Numbered list. Each capability:
>
> - **Name** — what it is.
> - **Description** — 1–2 sentences.
> - **Acceptance signals** — what makes it "done" at phase close.

## 5. Capabilities out of scope

> List the capabilities deliberately deferred. Each entry should reference where it goes next (a future scope doc, a placeholder milestone, a candidate work package in a later milestone, etc.).

## 6. Constraints carried into this scope

> Working rules, dependency pins, cost caps, license constraints, demo rules, governance rules that apply throughout the phase. Reference [`01-working-rules.md`](01-working-rules.md); this section captures phase-specific overlays only.

## 7. Acceptance for phase close

> Observable conditions that mark the phase closed. Typically a small set of headline criteria plus a pointer to the milestone close discipline in [`03-planning-model.md` §15](03-planning-model.md).

## 8. Open questions deferred to later phases

> Decisions that this phase explicitly does not make. Each item names where the decision will be made (next phase scope, ADR slot, future research).

---

## Where this document fits

The scope baseline is **authoritative for the phase it covers**. ADRs may refine or constrain it; working rules apply throughout; milestones and work packages execute against it. When the phase closes, this document is preserved as the historical baseline; the next phase authors a sibling document (`09-<phase>-scope.md`) rather than mutating this one.

## When to fill this in

Fill in at the start of a phase, once the phase's product and technical inputs are stable. Freeze when planning conversation aligns. After freeze, treat as immutable for the phase's duration; corrections go through ADR amendment or the exception process in [`01-working-rules.md`](01-working-rules.md).
