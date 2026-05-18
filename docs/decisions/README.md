# Architecture Decision Records (ADRs)

Binding decisions live here. Each ADR is a Markdown file at `<NNNN>-<slug>.md`.

> Naming note: some teams use `docs/adr/` instead of `docs/decisions/`. Either is fine — the convention is the same. This template uses `decisions/` for plain-English readability.

## Lifecycle

`Proposed` → `Accepted` → `Superseded`

- **Proposed** — recorded with rationale; awaiting empirical or stakeholder confirmation.
- **Accepted** — confirmed; binding for future work until superseded.
- **Superseded** — replaced by a later ADR; stays in place with a status pointer to the replacement.

Status moves only with explicit approval.

## Scope

Every ADR carries a `Scope:` field. Examples:

- `foundation` — engine choice, storage class, sticky decisions.
- `<phase> implementation` — phase-specific implementation choice; the `<phase>` is the project's own phase name.
- `tooling` — CI, build, dev environment.

## When to write an ADR

Write one when a decision:

- has cross-work-package or cross-phase implications,
- is expensive or destabilizing to reverse,
- needs to outlive the conversation that produced it.

Work-package-local choices stay in the task file, not here.

## Numbering

`0000-template.md` is the template. Real ADRs start at `0001`.

Append-only: add new ADRs at the next free number. Do not renumber. Superseding ADRs reference the prior number explicitly in their header.

## Template

Use [`0000-template.md`](0000-template.md) as the starting point.
