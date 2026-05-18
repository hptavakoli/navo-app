# Navo App — Product context

**Status:** Initial draft authored at bootstrap from the foundation brief. Refinement expected during the `01-foundation` milestone as boundaries firm up.

> The "what is this project, who is it for, why does it exist" document. Distinct from raw ideation in [`../context/product-source/`](../context/product-source/) — this is the **refined** product framing. AI sessions read this to ground every other piece of context.

---

## What this is

Navo App is the end-user application for Navo, an AI-powered migrant life intelligence platform for Germany. It gives immigrants and foreign residents a personalised, source-linked, translated, practical, and optionally listenable feed of life-relevant updates.

Navo as a whole is organised as a multi-repo workspace with `navo-backend`, `navo-app` (this repo), and `navo-website`. Each repo is independently maintained; the three communicate over network protocols. This repo owns the **user-facing application experience**: helping users choose their language, city, state, topics, notification preferences, and audio preferences, then receive relevant updates in a concise, source-linked, translated, and practical format. The app should make it easy to read, listen, search, bookmark, report errors, and understand whether an update may require action.

Three boundaries shape what this repo does *not* own:

1. **Backend product logic** belongs to `navo-backend` — the canonical API and product data authority. The app consumes those APIs; it does not duplicate users/preferences/content/search/notifications/subscription business rules locally.
2. **Public SEO/AEO surfaces** belong to `navo-website` (Next.js). The app does not need to be discoverable via search engines, render public pages, or duplicate website acquisition surfaces.
3. **Automated content/data creation** belongs to a future separate Hermes-style pipeline/agent project — source monitoring, crawling/scraping where permitted, extraction, summarisation, translation, audio generation, image generation, and publishing preparation. The app consumes processed content via the backend; it does not produce that content.

## Who it's for

End users of Navo:

- **Initial audience:** Persian/Farsi-speaking immigrants and foreign residents in Germany.
- **English** is the base language for project documentation, product structure, and technical coordination. Persian/Farsi is the first non-English user-facing launch community.
- **Later expansion:** Arabic, Turkish, and additional language communities (each adding language and likely RTL/script considerations to the UI).

The app is the surface these users actually touch — mobile-first (Android and iOS) with possible Flutter Web or desktop surfaces later, *if* foundation research/ADRs validate that direction.

## Broad goals

- A user in Germany sees a feed of relevant updates in their own language, with sources preserved, translated where appropriate, and labelled by reliability and review state.
- The app helps users *understand whether an update may require action* — surfacing dates, affected groups, urgency, location relevance, and trust signals without inventing advice.
- Audio-first listening (per-item playback, daily briefs, weekly briefs) is a first-class experience, not an afterthought, with entitlement gating where premium audio applies.
- Search and AI-assist UX are user-facing, grounded in source-linked backend retrieval — the app never invents unsupported advice on top of those answers.
- Bookmarks, buckets, highlights, notes, and calendar reminders let users return to important deadlines or references without re-finding them.
- Localisation, source labels, correction/outdated state, and non-legal-advice boundaries are visibly part of the UI across languages, not hidden in disclaimers.

## Boundaries (what this is NOT)

- **Not the backend.** API contracts, business rules, content storage, search indexing, notification orchestration, subscription/entitlement state, and admin tooling live in `navo-backend`. The app is a client.
- **Not the public website.** Public SEO/AEO, landing pages, blog/glossary surfaces, and public acquisition are owned by `navo-website`.
- **Not a content production system.** Crawling, scraping, extraction, summarisation, translation, audio/image generation, and publishing preparation are out of scope; they may later live in a separate Hermes-style pipeline.
- **Not a generic news reader.** Navo is source-aware and trust-oriented; the app presents provenance and review state, never strips them.
- **Not a legal-advice surface.** The app labels content; it does not provide or proxy legal advice.
- **Not a social platform.** No user-to-user content surface, no public profiles, no follow graph. Community-report moderation is a trust signal upstream, not a social feature here.
- **Not the entire Navo system.** This repo is one of three; cross-repo concerns land as ADRs in each affected repo, not centralised here.

---

## Where this document fits

This is the broadest "what is this project" doc. Detailed technical decisions live in ADRs at [`decisions/`](decisions/). Detailed scope lives in [`08-scope.md`](08-scope.md) (or a phase-specific renamed version) authored when the project enters that phase. Working discipline lives in [`01-working-rules.md`](01-working-rules.md). Planning shape lives in [`03-planning-model.md`](03-planning-model.md). Capability outline lives in [`06-feature-blueprint.md`](06-feature-blueprint.md).

## When to fill this in

Almost every project benefits from at least a one-paragraph version of this doc. A pure tooling library may stay terse; a user-facing application will likely expand to 2–3 pages. Update when the audience or goals shift materially — not for cosmetic edits.
