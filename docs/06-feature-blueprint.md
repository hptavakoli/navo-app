# Navo App — Feature blueprint

**Status:** Initial capability outline authored at bootstrap from the foundation brief. Each capability is concept-level; specific technical commitments land in [`07-technical-direction.md`](07-technical-direction.md) and ADRs once foundation research validates them.

> Concept-level capability outline. Answers "what can the project do?" without committing to "how it's built." Sits between the broad product framing in [`05-product-context.md`](05-product-context.md) and the technical commitments in [`07-technical-direction.md`](07-technical-direction.md).

---

## Capability map

### Onboarding and preferences

Help the user choose language (English/Persian first; later Arabic/Turkish/others), city, state, topics, notification timing, audio preference, and transport-route preferences. These choices flow through to the backend's user/preference APIs; the app stores them locally only for offline / pre-auth UX.

### Personalised feed and detail views

Display relevant source-linked updates fetched from the backend. Each item carries summary, location relevance, affected group, urgency, action need, dates, and trust labels (source reliability, review state, correction/outdated indicator). Detail views preserve source links and provenance.

### Audio-first experience

Per-item audio playback, daily-brief queues, weekly-brief content, connector phrases, and premium listening surfaces where supported. Premium-tier audio is gated by entitlement state served by the backend; signed-URL or CDN-reference patterns are handled by the backend.

### Search and AI-assist UX

User-facing search/chat entry points backed by backend retrieval and source-linked answers. The app does not invent unsupported advice — it surfaces what the backend's search/RAG gateway returns, with provenance preserved.

### Bookmarks, buckets, highlights, and notes

Let users save updates, organise them into buckets, highlight useful sections, and return to important deadlines or references. State syncs through the backend so the same user on a new device sees the same saved set.

### Notifications and reminders

Daily briefs, urgent alerts, topic/location updates, transport alerts, saved-item changes, and calendar reminders. Triggered through backend orchestration and external push providers (FCM/APNs). Frequency, channel, and locale come from user preferences.

### Account and entitlement surfaces

Provider-based sign-in (Apple/Google or other managed identity), profile/preferences editing, subscription state display, premium audio access, and account controls. Identity maps to backend user profiles; the app does not manage account/subscription truth.

### Trust, safety, and localisation UX

Make source links, reliability labels, review status, correction/outdated state, and non-legal-advice boundaries visible and understandable across languages. RTL support, font/typography for Persian/Arabic, and translation-state markers are part of this surface.

---

## Out of scope

- **Backend product logic.** Users/preferences/content/search/notifications/subscription business rules live in `navo-backend`. The app calls those APIs; it does not duplicate them.
- **Public website / SEO surfaces.** Landing pages, blog/glossary content, public discovery, and AEO are owned by `navo-website`.
- **Automated content/data creation.** Source monitoring, scraping, extraction, summarisation, translation, audio generation, image generation, and publishing preparation belong to a future external Hermes-style pipeline.
- **Server-side rendering of content.** The app is a client; the backend determines content shape and entitlement.
- **Legal-advice mediation.** The app labels and surfaces; it does not advise.
- **Direct user-to-user social features.** No follows, no public profiles, no UGC surface beyond community reports plumbed through the backend.

---

## Capability priority

To be assigned during `01-foundation` milestone planning, once foundation research and ADRs identify which capabilities anchor the first runtime milestone. Initial expectation: onboarding/preferences, personalised feed, source-linked detail views, and basic notifications are core to a usable v0; audio-first, search/AI-assist, bookmarks/highlights, and subscription surfaces follow based on phase scoping and backend readiness.

---

## Where this document fits

This document is the connective tissue between [`05-product-context.md`](05-product-context.md) (audience and goals) and [`07-technical-direction.md`](07-technical-direction.md) (stack and architecture). It also feeds the scope baseline at [`08-scope.md`](08-scope.md) when a phase begins.

## When to fill this in

Fill in when the project has multiple meaningful capabilities and benefits from a single page that names them. For a single-purpose tool or library, a one-line summary in [`05-product-context.md`](05-product-context.md) may be sufficient and this file can stay minimal — or be deleted, with the gap accepted (numbering is stable; reuse is forbidden).
