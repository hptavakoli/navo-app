# Navo App

> Important Germany updates, in your language.

Navo App is the end-user application for Navo, an AI-powered migrant life intelligence platform for Germany. It gives immigrants and foreign residents a personalised, source-linked, translated, practical, and optionally listenable feed of life-relevant updates.

## Status

Pre-implementation. Initialised from the foundation template; first milestone is `01-foundation` (placeholder, not yet opened); no work packages have been opened. Flutter (Android/iOS first) is the proposed direction; the final framework decision is an open question for foundation research/ADRs.

## What this is

Navo App is one of three repositories that make up the Navo product:

- **`navo-backend`** — TypeScript/NestJS backend that owns product data, APIs, personalisation, search, notifications, and subscription/entitlement state.
- **`navo-app`** (this repo) — Flutter mobile application (proposed) that consumes the backend's APIs.
- **`navo-website`** — Next.js public website that owns SEO/AEO and public acquisition.

The three repositories are independently maintained and communicate over network protocols. This repo owns the user-facing application experience: onboarding and preferences, personalised feed and detail views, audio-first listening, search/AI-assist UX, bookmarks/buckets/highlights/notes, notifications and reminders, account/entitlement surfaces, and trust/safety/localisation UX. Backend product logic, public website surfaces, and external content/data creation are explicitly out of scope.

Detailed framing lives in [`docs/05-product-context.md`](docs/05-product-context.md).

## Getting started

The repo is documentation-first and currently carries no runtime code. Implementation begins only after the `01-foundation` milestone validates Flutter (or revises the framework choice), defines first UX surfaces, identifies backend API dependencies, and lands the foundation ADRs. Operational guidance, once a runtime exists, will live in [`docs/04-runbook.md`](docs/04-runbook.md).

```
# No runtime commands yet.
```

## Documentation

- [`CLAUDE.md`](CLAUDE.md) — cold-start dispatcher for AI sessions.
- [`docs/`](docs/) — authoritative project documentation:
  - [`docs/01-working-rules.md`](docs/01-working-rules.md), [`docs/02-git-workflow.md`](docs/02-git-workflow.md), [`docs/03-planning-model.md`](docs/03-planning-model.md), [`docs/04-runbook.md`](docs/04-runbook.md) — operational discipline.
  - [`docs/05-product-context.md`](docs/05-product-context.md), [`docs/06-feature-blueprint.md`](docs/06-feature-blueprint.md), [`docs/07-technical-direction.md`](docs/07-technical-direction.md), [`docs/08-scope.md`](docs/08-scope.md) — project shape.
  - [`docs/decisions/`](docs/decisions/) — Architecture Decision Records (ADRs).
  - [`docs/research/`](docs/research/) — research and synthesis (when used).
- [`context/tasks/`](context/tasks/) — milestones and work packages.
- [`context/sanity-checks/`](context/sanity-checks/) — readiness reports per work package.
- [`CHANGELOG.md`](CHANGELOG.md) — release history.
