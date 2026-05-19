# v1 — Foundation direction (app)

**Status:** Draft
**Date:** 2026-05-19
**Inputs:** [01-app-platform.md](01-app-platform.md), [02-app-backend-integration.md](02-app-backend-integration.md)
**Supersedes:** —
**Superseded by:** —

> Per [`README.md`](README.md), research documents become **immutable once cited by a synthesis**. The two topical research files listed above are frozen at their state in this repo as of this commit.

---

## Purpose

Consolidate the two app research topics into a single recommended direction for the foundation phase. **Not authority.** Names strong candidates, identifies decision gates, and lists the cross-repo contracts the app depends on.

---

## Inputs

- [01-app-platform.md](01-app-platform.md) — Flutter vs alternatives.
- [02-app-backend-integration.md](02-app-backend-integration.md) — consumer-side patterns for API, auth / session, offline cache, push, in-app purchase, audio delivery.

---

## What this synthesis declares

| Layer | Recommendation | Confidence | Alternative |
|---|---|---|---|
| Application framework | Flutter (Android + iOS first) | High | React Native (Expo) as medium-confidence alternative; KMP + Compose Multiplatform as a "revisit in 2027" option |
| Language | Dart (bound to framework choice) | High | n/a (shifts only if framework shifts) |
| State management / routing / DI | Not selected at this synthesis; opens after the framework ADR | n/a | Bloc / Riverpod / Provider + GetIt — picked together, outside this synthesis |
| Localisation | English + Persian (RTL) at MVP; bundled Vazirmatn font; per-screen `Directionality`; mixed-script bidi handled by inline `dir` attribute | High | Future Arabic + Turkish add cleanly under the same approach |
| API client | OpenAPI-generated client over `dio` with single-flight refresh-on-401 interceptor | Medium-high (depends on backend API style ADR) | `ferry` for GraphQL if backend picks GraphQL; hand-rolled DTO layer if both above are blocked |
| Auth / session | Access + refresh tokens in `flutter_secure_storage`; backend issues both; provider sign-in (Apple / Google) on-device, exchanged with backend for Navo session tokens | High | Email magic-link as fallback sign-in; phone OTP only if a real need emerges |
| Offline / cache strategy | HTTP cache headers (ETag, `Cache-Control`, `stale-while-revalidate`) for ephemeral content; typed local store (drift or Isar) for explicit offline (bookmarks, downloaded audio) | Medium (boundary between cache and DB is itself an ADR-worthy decision) | None |
| Push notifications | `data`-only FCM payloads from backend; app handles display logic, deep-link routing, locale-aware text; preference state in backend with app-side mirror | Medium-high | OneSignal SDK only if backend picks OneSignal |
| Deep links | Universal Links (iOS) + App Links (Android) for installed-user share URLs; `go_router` in-app | High | Deferred deep links (Branch / AppsFlyer / Adjust) deferred until acquisition measurement is the priority |
| Subscriptions | RevenueCat-managed flow for v1, with backend webhook sync | Medium-high (depends on backend subscription ADR) | Direct StoreKit2 + Play Billing only if backend ADR picks the direct path |
| Audio playback | `just_audio` + `audio_service`; signed URLs for premium audio; public CDN URLs for free audio; range-request streaming + optional download for offline | High | None — well-trodden Flutter pattern |
| App-event analytics | Sent to backend (privacy-conscious); backend forwards to whichever analytics provider the website chose, or queries its own DB | Medium | PostHog SDK only if PostHog is chosen across web + app |

---

## Rationale

**1. Flutter is the right framework for Navo's shape.** Audio maturity (`just_audio` + `audio_service`) is best-in-class for content apps; UI consistency across iOS and Android reduces design debt for a one-founder build; RTL works with bundled Vazirmatn; native integrations (FCM, IAP, deep links) are well-trodden. React Native (Expo) is a credible alternative on the "TypeScript everywhere" axis but does not deliver enough cross-stack code-share to overturn Flutter's audio-maturity advantage. KMP + Compose Multiplatform's iOS-side tooling is still less mature than Flutter as of 2026.

**2. The app is a thin client over backend authority.** Auth tokens, entitlement state, push preference state, search authority, content authority — all live in the backend. The app renders, caches, and orchestrates. Six cross-repo contracts (API style, token model, push payload, subscription receipt, audio URL signing, deep-link URL space) form the entire integration surface.

**3. `data`-payload-only push gives Navo locale-correct, in-app-aware notifications.** Server-rendered `notification`-style payloads would lock display text to send-time locale and prevent in-app suppression. `data` payloads put the decision logic in the app, which is correct for Navo's multilingual + in-app-context concerns.

**4. RevenueCat for v1, with explicit awareness of alternatives.** Velocity matters at MVP; RevenueCat's 1% MTR tax above $2.5k is acceptable; cross-platform entitlement sync is non-trivial to build from scratch. If backend later picks direct StoreKit2 + Play Billing (Phase-2 cost optimisation per the backend synthesis), the app's RevenueCat-vs-direct integration is the smallest part of that migration.

**5. Universal / App Links for installed users; defer deferred-deep-links.** Branch / AppsFlyer / Adjust have meaningful monthly minimums above the EUR 10/day MVP budget. Universal / App Links cover the primary need (installed users opening shared URLs in the app); deferred attribution can wait until conversion measurement is the next priority.

---

## What remains open

1. **Dart fluency / appetite** — founder confirmation that Dart is acceptable. Confirmed → no shift. Not confirmed → reopens the framework call (RN Expo is the next-best fit).
2. **State management / routing / DI** — opens after the framework ADR. Not blocking foundation.
3. **Offline cache library** — drift vs Isar vs Hive vs sqflite. Depends on what concrete offline content lives on-device (bookmarks definitely; downloaded audio likely; ?).
4. **Backend API style** — REST + OpenAPI (default working assumption) vs GraphQL vs tRPC. App's recommended candidate (OpenAPI-generated client) depends on the backend ADR; if tRPC wins on the backend side, the app loses TS-shared-type benefit and the recommendation degrades.
5. **Premium audio in MVP** — drives URL-signing decision. Yes → signed short-lived URLs everywhere; no → public CDN URLs at MVP, signed URLs only when premium tier launches.
6. **Subscription path** — RevenueCat (recommended) vs direct StoreKit2 + Play Billing (Phase-2). App-side change is small either way, but the receipt-flow shape differs.

---

## Cross-repo dependencies

Six contracts cross repo boundaries. Each is a future cross-repo ADR.

| Contract | Co-owned with | Direction-setter |
|---|---|---|
| API style | `navo-backend` | backend platform ADR |
| Auth token / session model | `navo-backend` | backend identity ADR |
| Push payload schema + deep-link routing | `navo-backend` | backend push ADR + app integration ADR |
| Subscription receipt → entitlement sync | `navo-backend` | backend subscription ADR + app integration ADR |
| Audio URL signing (free vs entitlement-gated) | `navo-backend` | backend audio-storage ADR |
| Universal Links / App Links URL space (well-known files on website, associated-domains in app) | `navo-website` | website deep-link ADR |

---

## Carry-forward into binding decisions

**App-internal:**

1. App framework (Flutter).
2. State management / routing / DI conventions.
3. Localisation and RTL conventions (bundled fonts, locale-fallback chain, language-switch UI, persistence).
4. Offline cache library + scope policy.

**Cross-repo (co-developed with backend / website):**

5. API-client shape.
6. Auth token lifecycle.
7. Push payload contract.
8. Deep-link / Universal-Link URL space.
9. Subscription receipt + entitlement-sync flow.
10. Audio URL signing strategy.

---

## References

- Topical research files cited in **Inputs** above (frozen by this commit).
- Workspace cross-repo context: [`../../../workspace-notes.md`](../../../workspace-notes.md) (non-authoritative).
- Related syntheses: [`../../../navo-backend/docs/research/v1-foundation-direction.md`](../../../navo-backend/docs/research/v1-foundation-direction.md), [`../../../navo-website/docs/research/v1-foundation-direction.md`](../../../navo-website/docs/research/v1-foundation-direction.md).
