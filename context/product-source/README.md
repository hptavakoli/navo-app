# Product source — ideation and raw idea documents

**This folder is NOT project authority.**

These are raw ideation / source documents the user writes (often early in the project) to capture business idea, product concept, target users, feature thoughts, pricing, vision, or any other "what could this become" content. AI sessions read them on demand at project start, or when explicitly invoked.

They are **not implementation authority, scope baselines, or binding decisions.** Insights are promoted into the refined product/technical docs (`docs/05–07`), into ADRs (`docs/decisions/`), or into work-package planning through the normal workflow — never silently.

---

## How AI should treat these documents

- **Read on demand**, especially at project start when grounding the broad picture.
- **Cite as context**, not as authority. "The product-source outline says X; the refined feature blueprint commits to Y" is a legitimate observation.
- **Do not bind a decision to a product-source statement alone.** A binding decision goes through an ADR, a scope baseline, or a frozen work-package task file.
- **Flag conflicts** between product-source and refined docs to the user — do not silently prefer one or the other.

---

## Structure

```
context/product-source/
  README.md                ← this file
  outline.md               ← master outline (top-level structure of the idea)
  sections/
    _section-template.md   ← scaffold for a new section
    NN-<topic>.md          ← per-section files (created by the user as the outline grows)
```

The user authors `outline.md` as the project-level master outline, then creates per-section files in `sections/` following the recommended naming pattern below.

---

## Section file naming

- **Pattern:** `NN-<topic>.md` (e.g., `01-business-idea.md`, `02-target-users.md`).
- **Numbering** mirrors the outline order. Two-digit prefix.
- **Append-only:** new sections take the next free number; do not renumber existing files. Section ordering at the conceptual level lives in `outline.md`, not in filenames.
- **`_<name>.md`** — leading underscore reserved for scaffolds and templates that should not be treated as content (sorts to the top of listings).

---

## When this folder is most useful

- The first AI session of a new project, when the user wants to convey product vision in prose before any structured planning happens.
- Early planning conversations that reference back to "what we said in the original idea doc."
- Audit moments late in a phase when the user wants to verify the implementation still serves the original intent.

---

## When this folder is NOT useful

- Day-to-day work-package execution (read the task file, not the source).
- Closure / readiness reports (refer to ADRs and scope baselines, not the source).
- Any moment when authority is needed (refer to refined docs, not the source).

---

## Relationship to other folders

- `outline.md` and `sections/*.md` here → consolidated into [`../../docs/05-product-context.md`](../../docs/05-product-context.md), [`../../docs/06-feature-blueprint.md`](../../docs/06-feature-blueprint.md), and [`../../docs/07-technical-direction.md`](../../docs/07-technical-direction.md) over time.
- The transformation is one-way; refined docs supersede the ideation for AI sessions that need authority.
- Source documents stay in place as historical context; they are not deleted when refined docs supersede them.
