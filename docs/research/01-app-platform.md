# Research topic 01 — App platform

**Status:** Cited (immutable)
**Date:** 2026-05-19
**Cited by syntheses:** [v1-foundation-direction.md](syntheses/v1-foundation-direction.md)
**Superseded by:** —

> **This document is cited by [v1-foundation-direction.md](syntheses/v1-foundation-direction.md) and is therefore immutable.** Errata go in a new research file with a `Supersedes:` pointer, not as edits here.

---

## Question

Which application-framework family should `navo-app` use to deliver the Navo mobile experience on Android and iOS — Flutter (the current proposed direction), a JavaScript-based cross-platform stack (React Native with Expo, Capacitor/Ionic), a Kotlin-Multiplatform shape (KMP + Compose Multiplatform), full native (Kotlin + Swift), or a PWA-first approach? The decision is sticky: it constrains the talent pool, library ecosystem, performance ceiling, and the integration shape of native concerns (push, audio, in-app purchase, deep links).

---

## Scope and assumptions

**In scope:**
- Cross-platform framework families viable in 2026 for a single-engineer build of a content-consumption app with audio playback, push notifications, in-app subscriptions, and deep-link content navigation.
- DX, ecosystem maturity, integration depth for the native concerns Navo needs.
- Talent / hiring signal — not the main constraint at founder phase but matters once a second engineer is needed.

**Out of scope:**
- Detailed state-management / routing / DI choices within whichever framework is picked — separate decision after the framework lands.
- Backend integration patterns — covered in [02-app-backend-integration.md](02-app-backend-integration.md).
- Specific subscription / notification / audio provider choices — covered in `navo-backend/docs/research/05-identity-monetisation-notifications.md` and (later) a dedicated audio research file.

**Assumptions:**
- Android + iOS are the launch platforms. Flutter Web / desktop are flagged as possible later in [docs/07-technical-direction.md](../07-technical-direction.md) but not part of MVP scope.
- The app is a thin client over `navo-backend`; no offline-only product, no on-device ML inference, no heavy native graphics. The platform's main duty is rendering content lists, playing audio, handling push and deep links, and presenting subscription flows.
- Content includes Persian/Farsi RTL UI; mixed-script content (English source quotes inside Persian briefs) is common. The framework's text and bidi handling matters.
- Founder is the sole engineer at foundation; toolchain stability and "one stack to learn deeply" matter more than ecosystem breadth.
- Primary audience: migrants and foreign residents in Germany, often on older Android devices with metered data plans; app size and runtime efficiency matter.

---

## Comparison criteria

| # | Criterion | Why it matters for Navo |
|---|---|---|
| C1 | UI fidelity / consistency across Android + iOS | Brand integrity for a multilingual content app; mixed-script rendering must look clean |
| C2 | RTL + bidi support; Persian/Arabic text rendering | Persian is a first-class launch language; bad bidi/font handling is shippable-blocking |
| C3 | Audio playback maturity (background, lock-screen, queue, offline cache) | TTS audio is a core Navo feature; flaky audio breaks the value proposition |
| C4 | Native integration cost (push, IAP, deep links, App Store review) | All four are mandatory; framework hiding too much native makes them brittle |
| C5 | Ecosystem maturity (library count, library quality) | Single-engineer build can't reinvent everything |
| C6 | Hot-reload DX and iteration speed | Single engineer needs the fastest inner loop available |
| C7 | App size and runtime performance | Older devices, metered data; sub-50 MB ideal |
| C8 | Talent pool and hiring signal | Future hire-ability for someone to take over or augment |
| C9 | Long-term framework risk (vendor backing, governance, deprecation history) | Multi-year commitment |
| C10 | Cross-platform code-share % (practical, not marketing) | Real reuse for a small team |

---

## Candidates

### Flutter (Dart) — current proposed direction

- **Summary** — Google-backed cross-platform UI toolkit. Single Dart codebase compiles to ARM-native binaries for Android and iOS (and JS/Wasm for Web, separate native binaries for desktop). Current stable line as of early 2026 is Flutter 3.x. License: BSD-3-Clause. Active development; strong release cadence; Material 3 and Cupertino widget sets maintained in-tree.
- **Strengths**
  - Pixel-precise UI consistency across platforms — the same widget tree on Android and iOS reduces design debt for a one-founder team.
  - Solid RTL story (`Directionality`, `TextDirection.rtl`); Persian and Arabic fonts well-supported once a bundled font ships (system Persian fonts have known coverage gaps, especially on iOS — solved by shipping Vazirmatn / IRANSans).
  - Mature audio ecosystem: `just_audio` + `audio_service` is the de-facto stack for background playback with lock-screen controls; widely battle-tested.
  - Strong DX: stateful hot reload is genuinely faster than competitors' equivalents; toolchain stability (`flutter doctor`, `flutter build`) is reliable.
  - Native integration via platform channels is well-documented; FCM (`firebase_messaging`), APNs, in-app purchase (`in_app_purchase` and RevenueCat's `purchases_flutter`), deep links (`go_router` + `app_links`/`uni_links`) are all standard.
  - Build size acceptable (typically 15–40 MB release APK for a Navo-shaped app).
  - One language to learn (Dart) for the whole app.
- **Weaknesses**
  - Dart is niche outside Flutter. Sharing code with backend (TypeScript) or website (TypeScript) is not practical.
  - Flutter Web has SEO / accessibility issues; not viable for the SEO-critical public website (that's `navo-website`), which rules out a "one stack everywhere" temptation.
  - Some platform-specific edge cases still require channel code (e.g., specific iOS audio routing, lock-screen artwork sizing).
  - Single-vendor governance (Google), mitigated by BSD license and large community.
  - In-app webviews and platform UI elements (share sheets, store sheets) sometimes feel less native than RN's bridged equivalents.
- **Evidence** — flutter.dev release notes Q1 2026; `just_audio` 0.10.x; `firebase_messaging` 14.x; `purchases_flutter` (RevenueCat) 7.x. Repository signal: flutter/flutter has >160k GitHub stars and an active release cadence (a stable per quarter, plus beta/dev).

### React Native (TypeScript / JavaScript) with Expo

- **Summary** — Meta-backed cross-platform framework using React for UI; renders to native components on each platform. The "New Architecture" (Fabric, TurboModules, Bridgeless) is stable across Expo SDK 51+. Expo (the managed RN toolchain) is now strongly recommended even by Meta. License: MIT. Active development; release cadence aligned with React.
- **Strengths**
  - TypeScript across app, website (Next.js), and backend (if NestJS/TS) — meaningful type / utility code-share.
  - Huge ecosystem; almost every native concern has at least one well-maintained library.
  - Expo's tooling (EAS Build, OTA updates, prebuild) is genuinely good DX for one-founder teams.
  - Talent pool is large; React engineers transition into RN quickly.
  - Native components mean platform UI conventions are easier to honour (less "everything looks the same" risk).
- **Weaknesses**
  - Cross-platform UI consistency is less automatic than Flutter; styling differences (shadows, fonts, default spacing) appear without careful design.
  - "Bridge / New Architecture" migration debt — most popular libraries are migrated by 2026 but corners still exist.
  - RTL has historically been fragile; improved (RN core supports RTL automatic flip via `I18nManager`) but every navigation/animation library needs verification.
  - Audio: `react-native-track-player` is the standard but less battle-tested than Flutter's `just_audio` for complex queueing edge cases.
  - Two languages active in the build pipeline (JS/TS + occasional native module work in Kotlin/Swift).
- **Evidence** — React Native 0.74+ (early 2026); Expo SDK 51-52; `react-native-track-player` v4+; recent OneSignal/RevenueCat SDKs.

### Kotlin Multiplatform + Compose Multiplatform

- **Summary** — JetBrains-backed cross-platform stack. Compose Multiplatform brings Jetpack Compose UI to iOS (alpha → stable progression through 2024–2025; iOS support marked stable in 2025). Native binaries on each platform via Kotlin/Native. License: Apache-2.0.
- **Strengths**
  - Native performance ceiling; no JS bridge or VM overhead per draw.
  - Strong typing (Kotlin); first-class on Android.
  - Native interop on iOS via Kotlin/Native; no second language for shared logic.
  - Jetpack Compose is genuinely modern UI for Android engineers.
- **Weaknesses**
  - iOS UI maturity is younger than Flutter's; expect more rough edges in animations, gestures, native iOS UI conventions.
  - Smaller ecosystem of cross-platform libraries; some Android-first libraries need re-implementing for iOS.
  - Hiring: Kotlin engineers are plentiful, but engineers experienced specifically in KMP+CMP for iOS are rarer than RN/Flutter equivalents.
  - Build times — KMP iOS builds are slow; iteration loop is the worst of the three viable cross-platform options as of 2026.
- **Evidence** — JetBrains Compose Multiplatform release notes 2025–2026; kotlinx-coroutines 1.8+; Ktor client; SQLDelight.

### Native (Kotlin + Swift, no shared UI)

- **Summary** — Two separate codebases (Android: Kotlin/Jetpack Compose; iOS: Swift/SwiftUI). Optional shared logic via KMP for ViewModels/data layer.
- **Strengths**
  - Best-in-class platform fidelity and performance.
  - First-class access to every platform feature on day one.
  - No cross-platform framework risk; both platforms are first-class vendor-owned.
- **Weaknesses**
  - Two codebases is a major multiplier on dev time for a one-founder build.
  - UI / feature parity drift is constant ongoing work.
  - Realistic only if budget for a second engineer exists soon, or if a specific native feature (rare) makes cross-platform infeasible.
- **Evidence** — Standard; not in serious contention for a single-engineer foundation build.

### Capacitor / Ionic (web tech wrapped)

- **Summary** — Ionic Framework UI components inside a Capacitor (or older Cordova) WebView wrapper. License: MIT.
- **Strengths**
  - Web devs ship app fast using existing web skills.
  - Could share UI components with the Next.js website in theory.
- **Weaknesses**
  - WebView performance ceiling is below native — feels off on older devices, which is exactly Navo's audience profile (Android mid/low-end is common in DE migrant demographics).
  - Audio playback in a WebView is fragile for background/lock-screen scenarios — and TTS audio is Navo's canary feature.
  - App Store review can be cranky about thin webview wrappers.
  - Bidi/RTL is fine (it's web), but interactions feel "almost native, but not."
- **Evidence** — Ionic 8 / Capacitor 6 (2025–2026).

### PWA-only (no app store presence at MVP)

- **Summary** — Progressive web app installed via "Add to Home Screen"; no app stores.
- **Strengths**
  - Zero app-store gatekeeping; instant updates.
  - Conceivably one codebase shared with the public website.
- **Weaknesses**
  - iOS PWA support is still limited in 2026 (no real push parity at app-grade reliability, audio backgrounding has caveats, no IAP).
  - App-store discoverability lost — a meaningful acquisition channel for Navo's audience.
  - Subscriptions can't use Apple/Google IAP — web-billing only — which has UX friction and revenue-share implications.
- **Evidence** — Recent Apple WWDC updates have not closed the gap meaningfully.

---

## Comparison

| Criterion | Flutter | React Native | KMP+CMP | Native | Capacitor | PWA |
|---|---|---|---|---|---|---|
| C1 UI consistency | Strongest | Medium | Medium | per-platform by design | Strong (web) | Strong (web) |
| C2 RTL / Persian | Strong with custom font | Adequate (per-lib check) | Good (native text) | Best | Strong (web) | Strong (web) |
| C3 Audio maturity | Strong (just_audio) | Adequate (rntp) | Native-level, less abstracted | Best | Weak in WebView | Limited on iOS |
| C4 Native integration | Strong (channels) | Strong (bridged) | Native | Native | Strong (Capacitor plugins) | Weak (web APIs only) |
| C5 Ecosystem | Strong | Strongest | Growing | N/A | Decent | Standard web |
| C6 Hot reload DX | Best | Strong (Fast Refresh) | Slow (KMP iOS) | Slow (iOS especially) | Web-fast | Web-fast |
| C7 App size / perf | 15–40 MB; native-grade | 25–60 MB; near-native | 20–40 MB; native-grade | Best | 30–80 MB; WebView-bound | N/A |
| C8 Talent pool | Strong, Dart niche | Strongest (TS/React) | Growing | Strongest (per platform) | Decent | Strongest |
| C9 Framework risk | Single vendor (Google) | Mature (Meta + open governance) | JetBrains-backed | Lowest | Ionic Inc + community | None |
| C10 Code share % | ~95% across mobile | ~90% across mobile | ~70–90% (Compose) or 30–60% (logic only) | 0–40% with KMP shared layer | ~95% | ~100% with website |

---

## Disqualified

- **Capacitor / Ionic** — disqualified for a production content+audio app. **Gate:** audio backgrounding inside WebView is too fragile for Navo's core TTS playback feature.
- **PWA-only at MVP** — disqualified. **Gate:** iOS push/IAP/audio limitations make a competitive launch infeasible in 2026; revisit only if Apple closes the PWA-on-iOS gap.
- **Native (Kotlin + Swift, no shared UI)** — not formally disqualified but ruled out by founder-resource constraint: two codebases is the wrong multiplier for a one-engineer foundation. Reopens only if a specific native-only capability becomes core, or if engineering capacity grows substantially.

---

## Findings and tradeoffs

1. The real choice is between **Flutter** (current proposed direction) and **React Native (with Expo)**. Both can deliver Navo's product on time. The decision pivots on whether the project weights *UI consistency + a single-language stack* (favours Flutter) or *TypeScript-everywhere + ecosystem breadth* (favours React Native).
2. **Kotlin Multiplatform + Compose Multiplatform** is a credible third option but the iOS-side tooling (build times, iOS-native UI fidelity) is less mature than Flutter's iOS story as of 2026. It becomes more attractive in 2027+ if the JetBrains roadmap delivers.
3. **Native (Kotlin + Swift)** is the wrong call for a one-founder build — two codebases is a multi-month tax on every feature. Revisit only if a specific native feature (e.g., on-device ML inference, ARKit/ARCore) becomes core to Navo.
4. RTL/Persian rendering is a real concern but solvable in any of the three viable options with care: bundled Persian font; per-screen `Directionality` (Flutter) / `I18nManager` (RN); testing on Android system fonts with Persian numerals; verifying bidi in mixed-script content.
5. **Audio is the canary feature.** A framework's audio maturity is a strong signal for "is this stack production-ready for content apps?" Flutter's `just_audio` + `audio_service` ecosystem is the most battle-tested for the Navo use case. RN's `react-native-track-player` is fine but has more community-support variability and more historic-bug churn.
6. **Code-sharing argument with backend/website** (RN's main TypeScript advantage) is often overstated in practice. Even on RN, the app rarely shares much real logic with a backend — types and small utilities at best — which is not enough on its own to overturn the framework decision.
7. **Hiring is not the binding constraint at founder phase** — but Dart's niche status would matter at a Series-A team-size. The ADR should weigh this consciously rather than treat it as default.

---

## Recommendation candidates

| Candidate | Confidence | Wins when |
|---|---|---|
| **Flutter** | High | Audio maturity and UI consistency are weighted highest; founder is willing to learn Dart (or already has Dart experience); team stays small (1–3 engineers) for the next 12–18 months |
| **React Native (Expo, New Architecture)** | Medium | TypeScript-everywhere code share is genuinely valued (e.g., backend lands on TS); future team-growth plan prefers React engineers; founder is already deep in React |
| **Kotlin Multiplatform + Compose Multiplatform** | Low (revisit 2027) | Project pivots to need maximum native performance and wants to converge with a Kotlin backend; willing to accept iOS-side rough edges; not the right call for MVP in 2026 |

The currently proposed direction (Flutter) is well-supported by the evidence. The research does not produce a reason to overturn it, but the ADR should record *why* it was chosen rather than treat it as default.

---

## Risks and unknowns

- **Dart-as-niche-language hiring risk** — manageable today (Flutter community is large) but worth flagging in the ADR.
- **iOS App Store policy drift** — Apple's posture on the "reader app" exception and EU DMA third-party billing alternatives is actively evolving in 2026; subscription flow choices may force framework-irrelevant work either way (see `navo-backend/docs/research/05-identity-monetisation-notifications.md` when filed).
- **Persian-script font coverage on iOS system fonts** — must ship a bundled Persian font; not a framework-level risk but worth recording.
- **Flutter Web ambition** — if the project later wants Flutter Web for an internal admin or auth surface, it is possible; if it wants Flutter Web for the public site, the SEO ceiling is too low (the public site stays on Next.js/Astro per `navo-website`).
- **Background audio on iOS** is reliable across Flutter and RN but only when the library is configured correctly; this is integration cost, not a framework comparison axis.
- **App size baseline drift** — Flutter releases occasionally bump baseline binary size; budget for re-measurement at each major upgrade.

---

## Implications for boundaries

- **app ↔ backend** — The framework choice does not strongly constrain the backend API style. REST/OpenAPI works equally well from Flutter and RN. GraphQL has slightly better tooling on RN (Apollo Client) than Flutter (`graphql_flutter` is functional but smaller community). If the backend lands on tRPC, RN gets the bigger benefit (TS code share); Flutter would consume tRPC over its HTTP shape only. See [02-app-backend-integration.md](02-app-backend-integration.md) for the consumer-side detail.
- **app ↔ website** — No direct shared code today on either Flutter or RN. Deep-link contracts between app and website (universal links / app links) are framework-agnostic.
- **Push, IAP, deep-link contracts** — framework-agnostic at the protocol level; ADR-able once the backend's push and subscription architecture is locked.

---

## ADR candidates / future foundation decisions

- **ADR — App framework: Flutter (proposed) vs React Native vs KMP+CMP**, citing this research and (when filed) the audio research.
- **ADR — State management / routing / dependency injection inside the chosen framework** — separate decision, opens after the framework ADR.
- **ADR — Localisation and RTL conventions for the app** — bundled fonts, locale-fallback chain, language-switch UI, persistence model. Cross-references the website's i18n ADR for consistency.
- **Future research — On-device storage / offline cache strategy** (Hive / Isar / drift / sqflite for Flutter; AsyncStorage / WatermelonDB / op-sqlite for RN) — opens after the framework ADR.

---

## Open follow-ups

- Confirm the founder is comfortable with Dart's smaller hiring pool before the ADR closes on Flutter.
- Confirm whether any Flutter-Web / desktop ambition is intended for `navo-app` (per [docs/07-technical-direction.md](../07-technical-direction.md) "possible Flutter Web / desktop later if justified") — affects state-management choice indirectly.
- Confirm the audio-maturity weighting; if audio is *not* a launch feature, the recommendation set widens to include RN at higher confidence.

---

## References

- Flutter release notes and roadmap — https://flutter.dev/release
- React Native New Architecture status — https://reactnative.dev/architecture/landing-page
- Expo SDK release log — https://expo.dev/changelog
- JetBrains Compose Multiplatform — https://www.jetbrains.com/lp/compose-multiplatform/
- `just_audio` Flutter package — https://pub.dev/packages/just_audio
- `react-native-track-player` — https://rntp.dev/
- Apple "Reader App" exception, App Review Guidelines 3.1 — referenced as policy authority; verify current state when ADR is opened
- EU Digital Markets Act, iOS alternative billing (2024–2026) — referenced as policy authority; verify current state when ADR is opened
