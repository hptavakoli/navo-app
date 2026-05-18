# Project State — Navo App

## Project Overview (Stable)

Navo App is the end-user application for Navo, an AI-powered migrant life intelligence platform for Germany. It gives immigrants and foreign residents a personalised, source-linked, translated, practical, and optionally listenable feed of life-relevant updates.

> Replace with 4–8 bullets describing the project's stable identity — what it is, who it's for, what's distinctive about its approach. This block changes rarely; the per-session updates land in `Current Status` and `Progress Log` below.

Core characteristics:

- Documentation-first and context-first project setup
- AI-assisted development with human decision ownership
- Minimal now, portable later; avoid premature infrastructure

> Add or replace bullets as they become true for the project.

---

## Current Status (Replace on update)

Current Status:

- No milestones opened yet — replace this block when the first milestone is opened.
- Active milestone: — (none)
- Active work package: — (none)
- Most recent decision: — (none yet)

Project model:
- Milestone + work-package, with local NN- numbering. See `docs/03-planning-model.md`.
- Milestones use `NN-<slug>/` (default start `01-`; `00-` reserved for explicit bootstrap phases). Work packages restart at `01-` per milestone. Tasks use `T0..TN`.
- Work-package status enum: `roadmap (not frozen)` → `planned` → `active` → `closed`. Milestone status enum: `placeholder` → `open` → `closed`.

Project-side authoritative documents (live; may be ahead of this ChatGPT-side snapshot — flag divergence rather than silently inferring):
- `CLAUDE.md` (dispatcher), `docs/01-working-rules.md`, `docs/02-git-workflow.md`, `docs/03-planning-model.md`, `docs/04-runbook.md`, `docs/05-product-context.md`, `docs/06-feature-blueprint.md`, `docs/07-technical-direction.md`, `docs/08-scope.md` (renamed per phase), `CHANGELOG.md`, the active milestone README at `context/tasks/<NN>-<milestone-slug>/README.md`
- Optional, when used: `docs/research/` (research + synthesis), `context/design/` (design track), `context/product-source/` (raw ideation, not authority)

---

## Next Action (Replace on update)

Next Action:
> Replace with the immediate next concrete user-visible action. Examples: "Open the first milestone planning conversation," or "Freeze WP-NN's task file," or "Author readiness report for WP-NN." Be specific.

Do NOT auto-start. Branch creation and Git writes follow `docs/02-git-workflow.md` §1.1 (operation ownership — Claude's execution boundary depends on the project's configured track).

---

## Operational Constraints (Critical for reasoning)

- ChatGPT responses to the user should be in Farsi
- Prompts generated for Claude should be in English
- Documentation-first before implementation
- Implementation follows the active scope baseline and working rules in `docs/01-working-rules.md` — these are agreed; no new implementation outside an authorized work package
- No global package installation
- No new dependencies without explicit approval
- No NEW Dockerfile, Compose file, package-manager file, or runtime structure without explicit approval
- Use virtual environments for local development when applicable
- Avoid premature frontend, backend, database, or deployment decisions
- Avoid over-engineering; build the smallest useful proof first
- Claude Code should act as engineering partner / reviewer, not autonomous product owner
- Claude Code should review context before writing code
- Keep changes small, reviewable, and aligned with project documents
- Git operation ownership is governed by `docs/02-git-workflow.md` §1.1; ChatGPT may help draft branch names, Conventional Commit subjects/bodies, and PR titles/bodies, but does not authorize Claude to execute Git writes — that authority comes from the user under §1.1's configured track

> Add project-specific operational constraints below as they emerge.

---

## Key Decisions (Distilled)

> Add 1-line distilled decisions as they become real. Examples (delete the placeholders):
>
> - Product name → Navo App
> - Tagline → Important Germany updates, in your language.
> - Repository model → https://github.com/hptavakoli/navo-app
> - Technical direction → Proposed direction is a Flutter app for Android and iOS first, with possible Flutter Web or desktop surfaces later if justified; the app consumes navo-backend APIs and should not own backend business logic, public SEO rendering, or external content pipeline automation. This is proposed, not final.
> - Environment strategy → local virtual environment for application code; infrastructure containers only when needed
> - Documentation discipline → ADRs for binding decisions; working rules govern implementation; scope baselines per phase

---

## Progress Log (Append-only)

> Add one line per session-relevant event:
> `YYYY-MM-DD → concise factual description.`
>
> Examples (delete; this section starts empty):
> - YYYY-MM-DD → Project initialized from foundation template.
