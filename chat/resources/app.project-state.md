# Project State — Navo App

## Project Overview (Stable)

Navo App is the end-user application for Navo, an AI-powered migrant life intelligence platform for Germany. It gives immigrants and foreign residents a personalised, source-linked, translated, practical, and optionally listenable feed of life-relevant updates.

Core characteristics:

- Documentation-first and context-first project setup
- AI-assisted development with human decision ownership
- Minimal now, portable later; avoid premature infrastructure

---

## Current Status (Replace on update)

Current Status:

- Repo bootstrap: complete (first commit `9a168a9`, 2026-05-19).
- ChatGPT resource layout: per-repo project-state and design-status files scope-prefixed (commit `4ec6f25`); shared resources restored to scope-agnostic `<scope>` form and byte-identical across the three repos (commit `12d4d95`).
- Active milestone: `01-foundation` — `placeholder` status; planning conversation not yet opened.
- Active work package: none.
- Design track: inactive (scaffold only at `context/design/`).
- Most recent decision: shared ChatGPT resources are byte-identical across the three Navo repos for single-upload in the shared ChatGPT Project; per-repo project-state and design-status files carry the `<scope>.` prefix.

Project model:
- Milestone + work-package, with local NN- numbering. See `docs/03-planning-model.md`.
- Milestones use `NN-<slug>/` (default start `01-`; `00-` reserved for explicit bootstrap phases). Work packages restart at `01-` per milestone. Tasks use `T0..TN`.
- Work-package status enum: `roadmap (not frozen)` → `planned` → `active` → `closed`. Milestone status enum: `placeholder` → `open` → `closed`.

Project-side authoritative documents (live; may be ahead of this ChatGPT-side snapshot — flag divergence rather than silently inferring):
- `CLAUDE.md` (dispatcher), `docs/01-working-rules.md`, `docs/02-git-workflow.md`, `docs/03-planning-model.md`, `docs/04-runbook.md`, `docs/05-product-context.md`, `docs/06-feature-blueprint.md`, `docs/07-technical-direction.md`, `docs/08-scope.md` (renamed per phase), `CHANGELOG.md`, the active milestone README at `context/tasks/<NN>-<milestone-slug>/README.md`
- Optional, when used: `docs/research/` (research + synthesis), `context/design/` (design track), `context/product-source/` (raw ideation, not authority)

---

## Next Action (Replace on update)

Next Action: open the `01-foundation` planning conversation to identify and outline the foundation work packages. Per `context/tasks/01-foundation/README.md`, likely candidates include: app framework validation ADR (Flutter vs native vs alternatives), app surface strategy ADR (Flutter Web/desktop in roadmap or deferred), state management / architecture ADR, localisation strategy ADR (English/Persian first, RTL, expansion), auth integration ADR (cross-repo with `navo-backend`), API contract dependency planning, audio player approach research, push notification ADR, subscription integration ADR (cross-repo), and MVP app scope for the next milestone. Order, granularity, and parallelism are decided at planning time.

Do NOT auto-start. Branch creation and Git writes follow `docs/02-git-workflow.md` §1.1 (operation ownership — `navo-app` is on the approval-gated track).

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

- Product → Navo App, end-user mobile application for the Navo product.
- Repository model → https://github.com/hptavakoli/navo-app (one of three independently maintained repos).
- Multi-repo split (not monorepo) → three repos: `navo-backend` (TypeScript/NestJS backend, proposed), `navo-app` (this one), `navo-website` (Next.js public, proposed). Each repo independently authoritative; cross-repo decisions become ADRs in each affected repo, not centralised.
- Repo boundary → owns the user-facing application experience: onboarding and preferences, personalised feed and detail views, audio-first listening, search/AI-assist UX, bookmarks/buckets/highlights/notes, notifications and reminders, account/entitlement surfaces, and trust/safety/localisation UX. Backend product logic is owned by `navo-backend`; public web / SEO is owned by `navo-website`.
- External pipeline boundary (Hermes) → the content/data creation pipeline (source monitoring, scraping where permitted, extraction, summarisation, translation, audio/image generation) is **out of scope for this repo**. The app consumes processed content via `navo-backend` APIs; the upstream pipeline may later live as a separate Hermes-style project that integrates with the backend through a controlled ingestion/import contract.
- Technical direction (proposed, not final) → Flutter for Android/iOS first, with possible Flutter Web/desktop surfaces later if justified. The app consumes navo-backend APIs and should not own backend business logic, public SEO rendering, or external content pipeline automation. To be validated through `01-foundation` research and ADRs before any implementation milestone begins.
- Git operation ownership → approval-gated track per `docs/02-git-workflow.md` §1.1 (Claude drafts and executes Git operations only with explicit approval).
- Cost discipline → EUR 10/day soft guideline for early AI/API experiments; hard limits TBD after provider, workload, and architecture decisions.
- Documentation discipline → ADRs for binding decisions (Proposed → Accepted → Superseded); working rules govern implementation; scope baselines per phase.
- ChatGPT Project model → single shared Navo ChatGPT Project carries the four shared resources (one upload each, byte-identical across repos) plus the per-repo `<scope>.project-state.md` files (and `<scope>.design-status.md` once each design track activates).

---

## Open Questions (For `01-Foundation`)

Repo-local — to validate or revise in `01-foundation`:

- App framework choice — Flutter proposed; alternatives like native Android/iOS, React Native, or other still open.
- App surface strategy — Flutter Web / desktop in roadmap, or deferred in favour of `navo-website` for web.
- State management and architecture — state library, routing/navigation, dependency injection, testing approach.
- Localisation strategy — English/Persian first, RTL UI, later Arabic/Turkish/other expansion, terminology handling.
- Audio player approach — background audio, queue behaviour, caching/offline, platform constraints.
- Push notification approach — FCM/APNs, token handling, preferences, deep-links.
- MVP app scope — first implementation milestone scope, decided at foundation close.

Cross-repo — to land as ADRs in each affected repo:

- App ↔ backend auth integration — with `navo-backend`.
- App ↔ backend API contract surface — with `navo-backend`.
- Subscription / entitlement sync — with `navo-backend`.
- Notification deep-link contract — with `navo-backend`.

---

## ChatGPT Project Upload Set

Single shared Navo ChatGPT Project carries the following resources. From this repo (`navo-app`), the only repo-specific upload today is `app.project-state.md` (this file).

- Shared (one upload each, sourced from any repo since byte-identical):
  - `communication.claude.md`
  - `communication.general.md`
  - `playbooks.workflows.md`
  - `user-environment.md`
- Per-repo scoped project-state (one upload each):
  - `backend.project-state.md` (from `navo-backend/chat/resources/`)
  - `app.project-state.md` (this file, from `navo-app/chat/resources/`)
  - `website.project-state.md` (from `navo-website/chat/resources/`)
- Deferred until each design track activates (per-repo, one upload each):
  - `backend.design-status.md`, `app.design-status.md`, `website.design-status.md`

Total uploads at current state: 7 files. After design tracks activate: 10 files.

---

## Progress Log (Append-only)

- 2026-05-19 → `navo-app` bootstrapped from foundation template; first commit `9a168a9`.
- 2026-05-19 → ChatGPT resource files renamed with `app.` scope prefix for shared-Project compatibility; commit `4ec6f25`.
- 2026-05-19 → Shared ChatGPT resources (`communication.*`, `playbooks.workflows.md`, `user-environment.md`) restored to scope-agnostic `<scope>` form; byte-identical across the three repos; commit `12d4d95`.
- 2026-05-19 → Early-context source documents (product scope; technology proposal, dated 2026-05-18) filed under `context/product-source/snapshots/` as frozen non-authority snapshots. See `context/product-source/README.md`.
