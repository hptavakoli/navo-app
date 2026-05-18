# context/design/ — design-track working knowledge

**Optional.** Activate when a project enters a design phase; otherwise leave this folder as scaffold.

> Design-track docs are the project's product/design working knowledge: brand identity, UI language, component decisions, design canon, surface maps, design-system pre-flight, design briefs. They are kept **separate from implementation authority** and from the development task / readiness layer in [`../tasks/`](../tasks/) and [`../sanity-checks/`](../sanity-checks/).

---

## When to activate

Activate the design track when:

- The project has a meaningful UI surface that requires deliberate design (not just functional CRUD).
- Brand / visual identity decisions are part of the product.
- A design system, design canon, or component library is part of the work.
- A dedicated design conversation is healthier than mixing design framing with implementation planning.

Skip when:

- The project is API-only / library / CLI with no UI surface.
- UI is intentionally minimal and follows a generic library default.
- Design and implementation are best handled in the same conversation (very small projects).

---

## Boundary rules

- **Design docs are working knowledge, not implementation authority.** Insights are promoted into ADRs, scope baselines, or work-package task files through the normal workflow — never silently.
- **Do not mix design and implementation modes** in a single session. See the design lifecycle in [`../../chat/resources/playbooks.workflows.md`](../../chat/resources/playbooks.workflows.md).
- **No visual generation, design-system handoff, or related work** until reviewed pre-flight decisions exist.
- **`design-status.md` is a pointer-only status index** — it does not duplicate the canonical design owners; it points at them.
- **Design status updates flow through the design workflow first**, then are reflected in ChatGPT-side resources by replacement (not by editing the resource directly).

---

## Structure

```
context/design/
  README.md             ← this file
  design-overview.md    ← entry point: what design surfaces exist, their status
  design-status.md      ← pointer-only status index (current / next-action)
```

Add files as the design track grows: brand canon, UI language brief, design-system pre-flight document, surface maps, etc. The structure is intentionally minimal at scaffold time.

---

## Cross-references

- [`../../docs/01-working-rules.md` §9](../../docs/01-working-rules.md) — design-track discipline as a working rule.
- [`../../chat/resources/playbooks.workflows.md`](../../chat/resources/playbooks.workflows.md) — design lifecycle and workflow.
- [`../../chat/resources/communication.general.md`](../../chat/resources/communication.general.md) — design-mode communication rules.
- [`../../chat/prompts/workflows/claude-design-track-bridge.md`](../../chat/prompts/workflows/claude-design-track-bridge.md) — ChatGPT → Claude design-track bridge prompt.
