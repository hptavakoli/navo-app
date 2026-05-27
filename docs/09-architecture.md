# Navo App — Architecture (living doctrine)

**Status:** Scaffold. Doctrine is filled in over time as architecture commitments emerge from planning conversations, ADRs, and executed work. Pre-`01-foundation`, established topics derive from existing hard rules and direction; most stack-touching topics are pending.

> Living architecture doctrine for Navo App. Records current architectural principles, seams, and invariants as they emerge. References binding sources (ADRs, working rules, scope baselines) rather than replacing them. Evolves through planning and review workflows; does not bind execution by itself — binding power lives in [`decisions/`](decisions/), [`01-working-rules.md`](01-working-rules.md), and [`08-scope.md`](08-scope.md).

This document is the **living architecture counterpart** to:

- [`07-technical-direction.md`](07-technical-direction.md) — bootstrap-time stack and initial architecture direction; preserved as the as-of-bootstrap snapshot.
- [`decisions/`](decisions/) — binding ADRs (authoritative).
- [`01-working-rules.md`](01-working-rules.md) §10 — project-specific hard rules (currently a placeholder until foundation-level rules land); architecture-touching rules bind regardless of doctrine.
- [`08-scope.md`](08-scope.md) — frozen phase baseline.

Doctrine here **describes**; ADRs, working rules, and scope baselines **bind**. Doctrine updates flow through the workflow in §4 below — never silent drift.

---

## 1. Purpose

This document maintains a live, descriptive index of Navo App's architecture: the seams that must hold, the invariants the system relies on, and the open architectural questions. It is the place an AI session or human reviewer goes to ask "what is the current architecture's shape?" without reconstructing it from ADRs.

The doctrine does not introduce new rules. It indexes what the binding sources already commit the project to.

---

## 2. Relationship to other documents

| Document | Role | Relationship to doctrine |
|---|---|---|
| [`07-technical-direction.md`](07-technical-direction.md) | Bootstrap-time technical direction | Doctrine is the **living counterpart** — `07` is the as-of-bootstrap snapshot; this document tracks how architecture evolves. |
| [`decisions/`](decisions/) | ADRs (binding) | Doctrine **references ADRs as sources**. Where doctrine summarizes a commitment, it points back to the ADR that binds it. Doctrine does NOT replace ADRs. |
| [`01-working-rules.md`](01-working-rules.md) §5, §10 | Provider/adapter discipline; project-specific hard rules | Architecture-touching hard rules bind regardless of doctrine. Doctrine restates them as architectural invariants. §10 is currently a customization placeholder for Navo App; foundation-level rules will land via ADR. |
| [`08-scope.md`](08-scope.md) | Frozen phase baseline | Per-phase commitments live in scope; this doctrine is cross-phase. |
| [`03-planning-model.md`](03-planning-model.md) §13 | Discovery map | The discovery map points here for the current living architecture answer and at `07` for the bootstrap-time answer. |

---

## 3. Architecture topics

Each topic carries a `Status` (`established` / `pending` / `not applicable`) and a `Source` pointer. Topics are added as planning conversations open them and are marked `superseded` (not deleted) when later doctrine or ADRs replace them.

| Topic | Status | Source |
|---|---|---|
| Thin-client posture over `navo-backend` APIs (no backend product logic local to the app — user/content/search/notification/subscription business rules live in the backend) | established | [`07-technical-direction.md`](07-technical-direction.md) "Architecture direction" · [`../README.md`](../README.md) "What this is" |
| Public SEO / AEO surface owned by `navo-website` (the app does not render server-side, sitemap, or open-graph for public discovery) | established | [`07-technical-direction.md`](07-technical-direction.md) "Architecture direction" |
| External content/data creation pipeline (Hermes-style — source monitoring, scraping, extraction, summarisation, translation, audio/image generation) out of scope; consumed via `navo-backend` APIs | established | [`07-technical-direction.md`](07-technical-direction.md) "Architecture direction" · [`../README.md`](../README.md) "What this is" |
| Adapter shape for external services (backend HTTP API, push providers, identity providers, payment/subscription providers, audio playback engine, analytics) — business logic does not import provider SDKs directly | established | [`01-working-rules.md`](01-working-rules.md) §5 · [`07-technical-direction.md`](07-technical-direction.md) "Architecture direction" |
| App framework decision (Flutter for Android + iOS first vs native Kotlin/Swift vs React Native vs other) | pending — `01-foundation` ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| App surface strategy (Flutter Web / desktop in roadmap, or deferred in favour of `navo-website`) | pending — product/technical ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| State management, architecture, routing/navigation, dependency injection, testing approach | pending — `01-foundation` app technical ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Localisation strategy (English + Persian first, RTL UI, later Arabic / Turkish / other expansion; terminology handling) | pending — `01-foundation` localisation ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Auth integration (Apple / Google sign-in on-device, identity mapping to backend user profile) | pending — cross-repo ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Backend API contract surface (feed, content, audio, search, bookmark, notification, subscription consumption) | pending — cross-repo ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Audio player approach (background-audio engine, queue behaviour, offline/cache strategy, platform constraints) | pending — audio UX / technical research | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Push notification approach (FCM Android + APNs iOS, token registration with backend, notification preferences, deep-link payload contract) | pending — notification ADR + cross-repo deep-link contract with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Subscription integration (app-store-native flow vs RevenueCat) and backend entitlement sync | pending — cross-repo monetisation ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| MVP app scope for first implementation milestone after foundation | pending — `01-foundation` milestone-scoping decision | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Pre-runtime topics (CI shape, build / signing / store-deploy pipeline, observability stack) | not applicable — no runtime exists | — |

> Topics are seeded from the hard rules in [`01-working-rules.md`](01-working-rules.md) and the architecture direction in [`07-technical-direction.md`](07-technical-direction.md). New topics enter via planning conversations or audit responses per §4.

---

## 4. Doctrine evolution rules

Architecture doctrine moves through the same discipline as any other authoritative document. The rules below are the canonical version; [`10-testing-strategy.md`](10-testing-strategy.md) §4 and [`11-engineering-practices.md`](11-engineering-practices.md) §4 refer back here.

1. **Proposing a doctrine change.** A doctrine change is proposed inside a planning conversation, an audit response, or a work-package closure. Silent drift — editing doctrine outside one of these channels — is not permitted.
2. **Accepting a doctrine change.** The product owner explicitly approves the change. It is then applied to this document in a `docs:` commit per [`02-git-workflow.md` §4](02-git-workflow.md).
3. **Binding precedence.** If a doctrine description appears to conflict with an ADR, working rule, or scope baseline, the binding source wins; the doctrine entry is updated to match. Doctrine never overrides ADRs or hard rules.
4. **Superseding.** A topic is `superseded` when a later ADR or doctrine update replaces it. Superseded rows stay in this document with a status pointer to the replacement; they are not deleted.
5. **Coverage check.** Audit runs and work-package closures validate doctrine touchpoints and surface deviations (touchpoint mismatch) or incidental violations (work conflicts with current doctrine but did not declare the touchpoint). Findings are routed through the work-package closure flow at [`../chat/prompts/execution/work-package-close.md`](../chat/prompts/execution/work-package-close.md) and the audit prompts under [`../chat/prompts/review/`](../chat/prompts/review/) for product-owner decision: preserve current doctrine, update doctrine, or refine the work.

Doctrine is descriptive; binding lives in ADRs, working rules, and scope baselines. The discipline here keeps doctrine *honest* — the document either matches the binding sources or is updated through one of the channels above.
