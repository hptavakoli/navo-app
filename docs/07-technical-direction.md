# Navo App — Technical direction

**Status:** Bootstrap-time stack and initial architecture direction. **All technical choices below are proposed, not final.** Validation through foundation research and ADRs is a precondition of the first implementation milestone; until then, treat this document as a working hypothesis rather than a binding commitment.

> Bootstrap-time stack, initial architecture direction, key technical commitments. Distinguishes "what we've decided technically" from "what we're still researching." When research is part of the workflow, this document is informed by [`research/`](research/) synthesis output and supersedes synthesis once committed.

> **Living architecture counterpart:** as architecture commitments emerge from planning conversations, ADRs, and executed work, the **current** architecture is tracked in [`09-architecture.md`](09-architecture.md) (living doctrine). This document is preserved as the as-of-bootstrap snapshot.

---

## Stack (proposed)

| Layer | Proposed choice |
|---|---|
| Application framework | Flutter (Android + iOS first; possible Flutter Web / desktop later if justified) |
| Language | Dart (proposed alongside Flutter; not independent of the framework choice) |
| State management / architecture | Open — to be selected during `01-foundation` |
| Routing / navigation | Open — chosen alongside state management |
| Backend integration | Consumes `navo-backend` APIs over HTTP; contract is owned by the backend |
| Auth | Provider sign-in (Apple/Google or other) on-device, identity mapped to backend user profile |
| Push notifications | FCM (Android) + APNs (iOS); tokens registered with the backend; payload/deep-link contract owned with backend |
| Subscriptions | App-store-native flow or RevenueCat — open, decided alongside the backend's subscription architecture |
| Audio playback | Background-audio capable Flutter solution; choice and offline/cache strategy open |
| Localisation | English + Persian first; RTL UI; later Arabic/Turkish/other expansion |

This is the proposed initial direction; rationale and final commitments will live in ADRs once foundation research validates each choice.

## Architecture direction (proposed)

The app is a **thin client over `navo-backend`'s APIs**. Business rules, content authority, search authority, notification triggering, and subscription/entitlement state live in the backend; the app renders, caches, and orchestrates the user experience around those APIs.

Three boundaries are load-bearing:

1. **No backend product logic locally** — the app does not duplicate user/content/search/notification/subscription business rules. It calls and renders.
2. **No public SEO/AEO surfaces** — those are owned by `navo-website`. The app does not need server-rendering, sitemaps, or open-graph metadata for public discovery.
3. **No content production** — crawling, scraping, extraction, summarisation, translation, audio/image generation, and publishing preparation are out of scope. The app consumes processed content via backend APIs.

External services accessed by the app (push providers, identity providers, payment providers) are wrapped behind adapter shapes so business logic in the app does not import provider SDKs directly. The specific providers may shift; the adapter seam should not.

## Key technical commitments

None are binding yet. Sticky decisions (Flutter major-version pin, Dart version, state management library, routing solution, RTL conventions, minimum platform versions, etc.) will accumulate here as ADRs land during `01-foundation`. Until then, this section is intentionally empty.

## Open questions

Each is answered via a foundation ADR, research synthesis, or planning conversation during `01-foundation`.

- **App framework decision** — Validate Flutter against native Android/iOS (Kotlin/Swift), React Native, or other alternatives. Answered by: a foundation ADR informed by `research/`.
- **App surface strategy** — Decide whether Flutter Web or desktop should be part of the app roadmap, or whether web remains only the Next.js website. Answered by: product/technical ADR.
- **State management and architecture** — Choose app architecture, state management, routing/navigation, dependency injection, and testing approach. Answered by: app technical ADR.
- **Localisation strategy** — Define English/Persian first-language support, terminology handling, right-to-left UI needs, and later Arabic/Turkish expansion. Answered by: localisation ADR.
- **Auth integration** — Decide how the app signs in with Apple/Google and how identity maps to backend user profiles. Answered by: app-backend auth ADR (cross-repo with `navo-backend`).
- **API contract dependency** — Define how the app consumes backend feed, content, audio, search, bookmark, notification, and subscription APIs. Answered by: backend/app contract planning; ADRs landed in each affected repo.
- **Audio player approach** — Decide playback architecture, background audio needs, queue behaviour, caching/offline support, and platform constraints. Answered by: audio UX/technical research.
- **Push notification approach** — Decide FCM/APNs integration boundaries, token handling, notification preferences, and deep-link behaviour. Answered by: notification ADR.
- **Subscription integration** — Decide RevenueCat/app-store subscription handling and backend entitlement sync. Answered by: monetisation/subscription ADR (cross-repo with the backend's own subscription ADR).
- **MVP app scope** — Decide which app capabilities belong in the first runtime milestone after foundation. Answered by: `01-foundation` milestone planning before the next milestone opens.

---

## Where this document fits

This document points down at ADRs ([`decisions/`](decisions/)) for binding rationale, sideways at [`05-product-context.md`](05-product-context.md) and [`06-feature-blueprint.md`](06-feature-blueprint.md) for product alignment, and up at [`research/`](research/) when applicable for evidence. The scope baseline at [`08-scope.md`](08-scope.md) cites this document for phase-specific commitments.

## When to fill this in

Fill in when:

- Research has produced enough signal to commit to a direction, or
- Initial scoping requires a stack to make further progress, or
- A binding decision in an ADR needs a home above the ADR for narrative context.

Until then, this document carries a one-line "to be decided after <trigger>" stub.
