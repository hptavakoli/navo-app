# Navo App — Design overview

**Status:** Scaffold. Activate when entering a design phase; until then, this file is a placeholder.

> The entry point into the design track. Lists the design surfaces that exist (or will exist), what's been decided, and where the canonical owners live. Companion to [`app.design-status.md`](app.design-status.md), which carries the current-status / next-action pointer.

---

## Design surfaces

> Replace with the design surfaces this project has or will have. Each surface is one heading. Examples (delete what doesn't apply):
>
> - Brand identity (logo, palette, typography)
> - Product UI language (interaction patterns, voice, motion stance)
> - Design system / component library
> - Specific surface designs (e.g., dashboard, onboarding, key user flows)
> - Marketing surfaces (landing page, demo material)

For each surface:

- **Status** — `not started` | `pre-flight` | `in design` | `frozen` | `superseded`
- **Canonical owner** — file or folder path inside `context/design/` that owns this surface
- **Open questions** — what's blocking advancement (if anything)

---

## Mode boundaries

Reminder of the boundaries that govern design work:

- Design and implementation are separate modes. See [`../../chat/resources/playbooks.workflows.md`](../../chat/resources/playbooks.workflows.md).
- Design documents are not implementation authority. Promotion to ADR / scope / task happens through the normal workflow.
- Visual / design-system generation does not begin until reviewed pre-flight decisions exist.

---

## Where to start

For an AI session entering design mode cold:

1. Read this file end-to-end.
2. Read [`app.design-status.md`](app.design-status.md) for current state and next action.
3. Read the surface-specific canonical owner relevant to the current task.
4. Read [`../../chat/resources/communication.general.md`](../../chat/resources/communication.general.md) §design-mode if not already loaded.
5. Confirm understanding before starting any design action.
