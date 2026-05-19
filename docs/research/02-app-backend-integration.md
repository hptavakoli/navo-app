# Research topic 02 — App ↔ backend integration

**Status:** Cited (immutable)
**Date:** 2026-05-19
**Cited by syntheses:** [v1-foundation-direction.md](v1-foundation-direction.md)
**Superseded by:** —

> **This document is cited by [v1-foundation-direction.md](v1-foundation-direction.md) and is therefore immutable.** Errata go in a new research file with a `Supersedes:` pointer, not as edits here.

---

## Question

How should `navo-app` consume `navo-backend`? Specifically: what API-client shape, auth/session lifecycle, offline/caching strategy, audio-delivery integration, push-notification + deep-link contract, and subscription/entitlement-sync model should the app adopt to be a thin, resilient client over backend authority? The boundary is load-bearing: backend owns content, identity, entitlement, and notification authority; the app renders, caches, and orchestrates UX around those APIs.

---

## Scope and assumptions

**In scope:**
- App-side patterns for consuming the backend over the network.
- App-side lifecycle for tokens, refresh, sign-out, multi-device behaviour.
- App-side cache strategy for feed/brief/audio content, and the conditions under which the app needs offline reads.
- App-side push registration, payload handling, deep-link routing, and notification preference UI persistence.
- App-side IAP receipt flow and how backend learns about entitlement state.
- App-side audio delivery: URL fetching, stream vs download, background playback hand-off.

**Out of scope:**
- Backend-side implementation of any of the above — covered in `navo-backend/docs/research/05-identity-monetisation-notifications.md` and `06-ingestion-contract.md`.
- App framework choice itself — covered in [01-app-platform.md](01-app-platform.md). This document assumes Flutter as the working hypothesis, but notes the framework-agnostic vs Flutter-specific aspects of each pattern.
- Specific provider choices for auth / IAP / push — those decisions land in cross-repo ADRs.

**Assumptions:**
- Flutter is the working assumption for the app framework (see [01-app-platform.md](01-app-platform.md)).
- Backend exposes a stable HTTP API. API style (REST + OpenAPI vs GraphQL vs tRPC) is TBD; this document calls out where the recommendation depends on which style the backend picks.
- Network conditions for Navo users are variable: many users on mobile data in DE; some bridge between Germany and Iran/Türkiye with intermittent or filtered connectivity. Resilient caching matters.
- The app is single-user-per-device for v1 (no account switcher).
- Audio content is short-form (60–600 sec items, MP3 or AAC). Stream-first with optional download for offline.

---

## Comparison criteria

| # | Criterion | Why |
|---|---|---|
| C1 | API-style fit and codegen story | Type safety, reduced hand-rolled DTOs, evolution velocity |
| C2 | Token lifecycle robustness (refresh on 401, secure storage) | Bad token UX = users dropping at session expiry |
| C3 | Offline / poor-network resilience | Migrant users on bridge networks; metered data |
| C4 | Push registration + deep-link routing reliability | Notifications are an acquisition + retention channel |
| C5 | IAP receipt → backend entitlement sync correctness | Loss of entitlement on the wrong device = refund risk |
| C6 | Audio delivery (URL signing, range requests, background play) | Core feature; failure modes are painful |
| C7 | Operational simplicity for a one-founder build | Fewer moving parts = fewer 2 a.m. issues |
| C8 | Future evolvability without breaking older app versions | Forced-upgrade story; backwards-compatible API contracts |

---

## Candidates per sub-area

This research is structured by integration concern rather than by single-vendor candidates. For each concern, the document lists the credible options and a recommendation candidate.

### A. API client shape

**Options under Flutter:**

- **Hand-rolled HTTP via `package:http` or `dio`**
  - *Summary* — direct HTTP calls, JSON decode by hand or via `json_serializable`. `dio` adds interceptors (auth, retry, logging), `http` is leaner.
  - *Strengths* — minimal dependency surface; full control; works with any backend API style.
  - *Weaknesses* — DTO duplication between app and backend; manual schema-drift detection; tedious for many endpoints.

- **OpenAPI-generated client** (`openapi-generator`, `swagger-dart-code-generator`, `chopper` with codegen)
  - *Summary* — backend publishes an OpenAPI spec; the app generates Dart client code from it.
  - *Strengths* — single source of truth; type-safe; cheap to add new endpoints; clean evolution.
  - *Weaknesses* — requires the backend to expose and version a quality OpenAPI spec; generator quality varies (some emit awkward Dart).

- **GraphQL client** (`graphql_flutter`, `ferry`)
  - *Summary* — if backend chooses GraphQL, the app gets selective field-fetching, fragments, and cache primitives out of the box.
  - *Strengths* — over-fetching minimised; one endpoint; built-in cache.
  - *Weaknesses* — Dart GraphQL tooling is smaller than Apollo (RN/web); push notifications + file uploads still need REST escape hatches; cache invalidation is real work.

- **tRPC** — not a viable consumer client from Dart. If the backend chooses tRPC, the app consumes it via the HTTP shape only, losing the TS-shared-type benefit. This is a meaningful constraint on the backend ADR.

**Recommendation candidate** — **OpenAPI-generated client over a `dio`-based base, paired with a backend that publishes a versioned OpenAPI spec.** Confidence: medium. Wins when the backend lands on REST/OpenAPI (likely default). If the backend lands on GraphQL, switch to `ferry` (compile-time codegen, Apollo-like cache). If the backend lands on tRPC, accept hand-rolled or OpenAPI shimmed by the backend — and re-evaluate the backend API style ADR with this cost in mind.

### B. Auth / session lifecycle

**Concerns:**

- **Sign-in surface** — provider sign-in on-device (Apple Sign-In on iOS, Google Sign-In on Android), plus optional email-link / magic-link fallback. The Persian-speaking diaspora in DE may have lower Apple adoption than DE-native demographics; a non-Apple sign-in path is non-optional on iOS too (App Store requires "Sign in with Apple" as an option *if* other social sign-ins are offered, but does *not* require it if only email-link is used). Cross-reference: `navo-backend/docs/research/05-identity-monetisation-notifications.md` sub-topic A for provider choice.

- **Token shape** — short-lived access token (15–60 min) + long-lived refresh token (rotation on use) is the modern default. Both are stored in platform secure storage (`flutter_secure_storage` → Keychain on iOS, Keystore-backed encrypted SharedPreferences on Android). Refresh tokens *must* rotate.

- **Refresh-on-401 interceptor** — single-flight queue: if multiple in-flight requests get 401 simultaneously, only one refresh is attempted, others wait. `dio_smart_retry` or hand-rolled.

- **Sign-out semantics** — `logout` calls backend to revoke refresh token, clears secure storage, clears cached personalised content (but not public-cache content like channel indexes).

- **Multi-device** — backend issues distinct refresh tokens per device; "sign out all devices" is a backend capability the app surface needs.

**Recommendation candidate** — adopt the access+refresh-token pattern with rotation, secure-storage of both, single-flight refresh interceptor, and an explicit revoke-on-logout call. Confidence: high (industry standard). Identity-provider specifics deferred to the backend's identity ADR.

### C. Offline / cache strategy

**Concerns:**

- **What needs offline reads?** — at MVP: the user's recent feed (last N days), bookmarked briefs, downloaded audio. Personalisation queries don't need offline; live search doesn't need offline.

- **How to cache?**
  - HTTP-cache headers (ETag, `Cache-Control: max-age`, `stale-while-revalidate`) → handled by `dio` cache interceptors (`dio_cache_interceptor` with Hive/Isar backend) or by a custom layer.
  - App-level write-through to a local store for "I want to read this on the plane" content.
  - Audio download → file system + index in local store.

- **Local store options for Flutter**: Hive (key-value), Isar (NoSQL with queries), drift (SQLite-backed, type-safe), sqflite (raw SQLite). For Navo's shape (lists of briefs, audio metadata) any will work; drift offers the strongest typing.

- **Sync model** — pull-on-resume (when the app foregrounds), pull-on-pull-to-refresh, push-driven refresh-on-notification. No two-way sync at MVP (no offline writes other than bookmark/like, which queue and replay on reconnect).

**Recommendation candidate** — adopt HTTP-cache headers for ephemeral content, layered with a typed local store (drift, Isar) for explicit offline content (bookmarks, downloaded audio). Confidence: medium. The boundary between "HTTP cache" and "local DB" is the right ADR to flesh out at planning time.

### D. Push notifications + deep links

**Push integration concerns:**

- **Token registration** — on app start (after sign-in), the app gets an FCM token (Android) and APNs token via FCM (iOS, if FCM is the chosen provider) and posts it to backend with the device ID, OS, app version, and locale. Backend stores per-user, per-device tokens.

- **Token rotation** — FCM tokens can rotate; the app must re-register on rotation. Backend treats the post as upsert by device-id.

- **Notification payload contract** — backend sends `data` payloads (Navo controls rendering), not `notification` payloads (lets OS render). This is important because Navo needs:
  - Custom deep-link routing (open brief X, channel Y, downloaded audio Z).
  - Suppression logic (do not display if user is currently in-app and looking at the same content).
  - Locale-aware text (the notification text must match user's locale, not server's locale).

- **Deep-link routing** — universal links (iOS) and App Links (Android) map `https://navo.de/brief/abc123` to the app if installed. In-app router (`go_router`) handles the path. Deferred deep links (open app → install from store → resume into the originally requested content) need the website to capture intent and the app to receive it on first install (Branch / Firebase Dynamic Links — but Firebase Dynamic Links is end-of-life in 2025, so Branch or AppsFlyer OneLink or Adjust are the live alternatives).

- **Preference UI** — opt-in/opt-out by category persisted in backend, with a copy in app for offline display.

**Recommendation candidate** — `data`-payload-only push, with backend-driven locale, app-side handler that decides display + routing. Universal/App Links for deep linking; deferred deep links via Branch (or AppsFlyer) only if needed (start without; add when conversion measurement is the next gate). Confidence: medium-high. The deferred-deep-link choice is the most contingent.

### E. Subscriptions / entitlement sync

**Concerns:**

- **Receipt flow** — user purchases inside the app (Apple IAP / Google Play Billing). Receipt token from the store is sent to backend. Backend verifies with Apple/Google server-to-server. Backend records entitlement state for the user.

- **Server-side validation** — never trust the client; always verify receipts with the platform's server-side API. Apple's `verifyReceipt` is deprecated in favour of App Store Server API + App Store Server Notifications; Google uses Play Developer API.

- **Webhook handling on backend** — Apple App Store Server Notifications v2 and Google Play Real-time Developer Notifications keep backend in sync with renewals, cancellations, refunds, and grace periods.

- **Cross-device entitlement** — when the user signs in on a new device, the app fetches entitlement state from backend (not from the local store receipt). Backend is the source of truth.

- **RevenueCat managed layer** vs **direct integration** — RevenueCat handles the IAP plumbing and exposes a unified entitlement API. Direct integration uses `in_app_purchase` (Flutter) plus backend server-to-server validation. Tradeoff explored in `navo-backend/docs/research/05-identity-monetisation-notifications.md` sub-topic B.

- **Promo codes / introductory pricing / EU DMA alternative billing** — these are real concerns in 2026; the app's role is to surface the right product, not to handle the legal mechanics. Backend orchestrates.

**Recommendation candidate** — RevenueCat-managed flow for v1 (high confidence given one-founder operational constraint), with a fallback ADR for direct integration if RevenueCat cost or vendor-lock becomes a blocker. Confidence: medium (depends on the backend's subscription ADR).

### F. Audio delivery

**Concerns:**

- **URL acquisition** — backend issues a stable URL for the audio asset. URL may be public (CDN, no auth) or signed (short-lived signature). Signed URLs are needed if the audio is gated by entitlement; public URLs are fine for free content.

- **Streaming vs download** — stream by default (HTTP range requests); user can download for offline. Cached chunks via the audio library or the app's own file store.

- **Background / lock-screen** — `audio_service` (Flutter) wraps platform background-audio APIs (MediaSession on Android; AVAudioSession on iOS) and surfaces lock-screen controls, artwork, and queueing. `just_audio` provides the actual player.

- **Multilingual audio** — Persian and English audio variants for the same brief may exist. App requests the variant matching user locale; backend exposes a canonical "this brief, this language, this audio URL" resolution.

- **Audio TTS provenance** — Navo audio is TTS-generated by the external Hermes-style pipeline. Pipeline-side concerns are out of scope for this repo; the app only sees URLs.

- **Analytics on play** — play / pause / complete events feed personalisation and content moderation (which briefs got listened to all the way through). Privacy-conscious analytics provider TBD; cross-reference `navo-website/docs/research/03-hosting-analytics-privacy.md`.

**Recommendation candidate** — `just_audio` + `audio_service` for playback, signed URLs for entitlement-gated audio, public CDN URLs for free audio, range-request streaming with optional download to local storage. Confidence: high.

---

## Comparison

| Sub-area | Recommendation candidate | Confidence | Backend-side dependency |
|---|---|---|---|
| API client | OpenAPI-generated over `dio`; GraphQL via `ferry` if backend picks GraphQL | Medium | Yes — backend API style ADR |
| Auth / session | Access+refresh-token with rotation, secure storage, single-flight refresh interceptor | High | Yes — identity provider ADR |
| Offline / cache | HTTP cache + typed local store (drift / Isar) for explicit offline | Medium | No (app-side decision) |
| Push + deep links | `data` payloads, app-side routing, universal/app links | Medium-high | Yes — push provider + payload contract ADR |
| Subscriptions | RevenueCat for v1 with backend webhook sync | Medium | Yes — subscription ADR |
| Audio | `just_audio` + `audio_service` with signed-or-public URLs | High | Yes — URL signing contract |

---

## Disqualified

- **tRPC as the app's API style** — Dart cannot consume tRPC natively. If the backend picks tRPC, the app loses the shared-types benefit and consumes a raw HTTP shape. Either tRPC is dropped (and an OpenAPI/REST or GraphQL is used) or the app's developer-velocity argument for tRPC vanishes. This is a backend ADR consequence to be visible to.

- **Firebase Dynamic Links for deferred deep links** — end-of-life in 2025; do not adopt. Branch / AppsFlyer OneLink / Adjust are the live alternatives.

---

## Findings and tradeoffs

1. The **backend's API style ADR** is the single largest external dependency for app integration. REST/OpenAPI gives the smoothest Flutter codegen story. GraphQL is workable. tRPC is a poor fit for a Dart client. The app's recommendation candidate (OpenAPI-generated) is contingent on the backend's pick.
2. **Auth/session patterns are well-trodden** — there is no novel risk in the access+refresh model. The provider choice is the open question, not the protocol shape.
3. **Offline strategy is a design choice, not a technical limit.** The decision is "what does the user expect to read offline" — and that's a product decision. The technical patterns are mature.
4. **Push payload shape is sticky.** Whatever shape the first version ships will be hard to evolve because older app versions in the field can't update their payload parser. Recommend versioning the payload shape from day one (`v: 1` field) and forcing-upgrade on breaking changes.
5. **RevenueCat is the operationally simple choice** but introduces vendor dependency on a third-party for a critical revenue path. The ADR must weigh that.
6. **Audio signing strategy depends on entitlement model.** If all audio is free at v1, signed URLs are unnecessary overhead. If premium audio launches at v1, signed URLs are mandatory. Sequence matters.
7. **Deferred deep links are a "later" feature** — universal/app links for installed users cover the primary need. Install-time attribution can wait until acquisition measurement becomes the next priority.

---

## Recommendation candidates

| Candidate | Confidence | Wins when |
|---|---|---|
| **OpenAPI-driven REST integration** with `dio` + secure-storage tokens + RevenueCat + `data`-payload push + `just_audio`+`audio_service` | High | Backend picks REST/OpenAPI; RevenueCat acceptable for v1; audio is core to launch |
| **GraphQL integration** with `ferry` and otherwise the same shape | Medium | Backend picks GraphQL; codegen quality holds up; cache invalidation is acceptable cost |
| **Hand-rolled `dio` + custom DTO layer**, no codegen | Low (fallback) | If both above are blocked; trade dev-velocity for fewer dependencies |

---

## Risks and unknowns

- **Backend API-style ADR may pick tRPC** despite the cost to the app's codegen story — this should be flagged in the API-style ADR cross-referencing back here.
- **iOS subscription policy drift** (EU DMA alternative billing rules) may force the app to surface a different purchase flow than US/RoW; RevenueCat handles much of this but not all.
- **Firebase Dynamic Links replacement** has no clean OSS option; Branch / AppsFlyer / Adjust all have meaningful monthly minimums above the EUR 10/day MVP budget. Worth flagging when acquisition measurement opens.
- **Audio CDN choice** (Cloudflare R2, Bunny, Hetzner, etc.) — currently out of scope; cross-reference `navo-backend/docs/research/04-jobs-cache-storage.md`. App-side change cost when CDN changes: low (URL substitution); but caching rules at the app may need re-tuning.
- **Locale-aware push payloads** depend on backend knowing per-device locale; ensure registration call carries locale + locale-update path.

---

## Implications for boundaries

- **app ↔ backend** — this entire document. Six contracts to ADR or document: API style, token model, push payload, IAP/entitlement, audio URL signing, deferred-deep-link attribution.
- **app ↔ website** — universal/app links share a URL space (`https://navo.de/brief/abc123`). The website must (a) include the correct iOS / Android `apple-app-site-association` and `assetlinks.json` files at hosting; (b) render server-side meta for previews when shared from the app. See `navo-website/docs/research/01-framework-and-rendering.md`.
- **app → ingestion contract** — none directly. The app consumes processed content from backend; it never touches the Hermes-style pipeline. See [`navo-backend/docs/research/06-ingestion-contract.md`](../../../navo-backend/docs/research/06-ingestion-contract.md) for the upstream boundary.

---

## ADR candidates / future foundation decisions

- **ADR — App API-client shape** (cross-repo with backend API-style ADR).
- **ADR — Auth token lifecycle for app ↔ backend** (cross-repo with backend identity ADR).
- **ADR — Push payload contract** (cross-repo with backend push ADR).
- **ADR — Deep-link / universal-link URL space** (cross-repo with website ADR).
- **ADR — Subscription receipt flow + entitlement sync** (cross-repo with backend subscription ADR).
- **ADR — Audio URL signing strategy and stream-vs-download policy** (cross-repo with backend audio storage ADR).
- **Future research — App-side offline cache library choice** (drift vs Isar vs Hive).
- **Future research — Acquisition / deferred-deep-link attribution provider** (when conversion measurement opens).

---

## Open follow-ups

- Confirm whether premium audio is part of MVP — drives the URL-signing decision.
- Confirm whether the website will own any auth surface — drives the cross-domain token / cookie posture.
- Confirm whether the app will support "sign-in via email-only" (no Apple/Google) for users without store accounts — affects auth provider ADR.

---

## References

- `dio` HTTP client — https://pub.dev/packages/dio
- `flutter_secure_storage` — https://pub.dev/packages/flutter_secure_storage
- `go_router` — https://pub.dev/packages/go_router
- `just_audio` and `audio_service` — https://pub.dev/packages/just_audio, https://pub.dev/packages/audio_service
- `drift` (SQLite ORM for Dart) — https://drift.simonbinder.eu/
- Apple App Store Server API and Notifications v2 — https://developer.apple.com/documentation/appstoreserverapi
- Google Play Developer API and RTDN — https://developers.google.com/android-publisher/api-ref/rest
- RevenueCat docs — https://www.revenuecat.com/docs/
- Universal Links (iOS) — https://developer.apple.com/ios/universal-links/
- App Links (Android) — https://developer.android.com/training/app-links
- Firebase Dynamic Links sunset notice (2025) — https://firebase.google.com/support/dynamic-links-faq
