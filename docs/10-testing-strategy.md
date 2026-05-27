# Navo App — Testing strategy (living doctrine)

**Status:** Scaffold. Doctrine is filled in over time as the testing posture firms up. Pre-`01-foundation`, established topics derive from existing hard rules; runtime-touching topics are pending or not applicable until a runtime exists.

> Living testing-strategy doctrine for Navo App. Records current expectations about what gets tested, at what layer, and with what kind of evidence. References binding sources (ADRs, working rules, scope baselines) rather than replacing them. Evolves through planning and review workflows; does not bind execution by itself — binding power lives in [`01-working-rules.md`](01-working-rules.md), [`08-scope.md`](08-scope.md), and [`decisions/`](decisions/).

This document is the **living testing-strategy counterpart** to:

- [`01-working-rules.md`](01-working-rules.md) §5 (provider/adapter discipline) and §7 (review discipline) — binding rules restated here as testing invariants.
- [`07-technical-direction.md`](07-technical-direction.md) "Architecture direction" — the backend HTTP adapter, push/identity/payment provider adapters, and the audio-playback seam are primary testing surfaces.
- [`decisions/`](decisions/) — binding ADRs.
- [`08-scope.md`](08-scope.md) — frozen phase baseline; per-phase acceptance lives in the scope's acceptance section.
- [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) — readiness-report template; per-work-package test evidence captured at closure.

Doctrine here **describes**; ADRs, working rules, and scope baselines **bind**. Doctrine updates flow through the workflow in §4 below — see [`09-architecture.md`](09-architecture.md) §4 for the canonical evolution rules.

---

## 1. Purpose

This document maintains a live, descriptive index of Navo App's testing strategy: what must be tested (and at what layer), what kinds of evidence are produced, and which testing topics remain open. It is the place an AI session or human reviewer goes to ask "what's the current testing posture?" without reconstructing it from working rules and scope.

The doctrine does not introduce new rules. It restates and indexes what the binding sources commit the project to.

---

## 2. Relationship to other documents

| Document | Role | Relationship to doctrine |
|---|---|---|
| [`01-working-rules.md`](01-working-rules.md) §5, §7 | Provider/adapter and review discipline | Binding rules. Doctrine restates them as testable expectations. |
| [`07-technical-direction.md`](07-technical-direction.md) | Bootstrap-time technical direction | The backend HTTP adapter surface, the push/identity/payment provider adapter seams, and the audio-playback engine set primary contract- and seam-validation testing surfaces. |
| [`decisions/`](decisions/) | ADRs (binding) | Testing-relevant ADRs (e.g., framework choice, state-management choice, push approach, subscription approach) are sources for topics here. |
| [`08-scope.md`](08-scope.md) | Frozen phase baseline | Per-phase test acceptance lives in scope's acceptance section; doctrine is the cross-phase view. |
| [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) | Readiness-report template | Per-work-package test evidence (smoke logs, byte-identity checks, walkthroughs) is captured per closure. |

---

## 3. Testing topics

Each topic carries a `Status` (`established` / `pending` / `not applicable`) and a `Source` pointer.

| Topic | Status | Source |
|---|---|---|
| Provider/adapter mock seam discipline (business logic exercised against both live and mock adapters) | established | [`01-working-rules.md`](01-working-rules.md) §5 |
| Per-work-package readiness-report evidence | established | [`01-working-rules.md`](01-working-rules.md) §7 · [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) |
| Independent audit pass for non-trivial work packages (encouraged) | established | [`01-working-rules.md`](01-working-rules.md) §7 |
| Readiness reports as frozen audit evidence | established | [`01-working-rules.md`](01-working-rules.md) §7 |
| Test framework selection (Flutter `flutter_test`, `integration_test`, Dart `test`, or equivalents depending on framework decision) | pending — depends on framework choice in `01-foundation` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Widget / unit test posture (state-management library testing pattern, navigation testing pattern) | pending — depends on state management / architecture ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Integration / end-to-end test posture (real-device vs emulator, golden tests for UI, screen-size matrix) | pending — depends on app technical ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Backend HTTP adapter mock seam (contract testing against `navo-backend` APIs) | pending — cross-repo API-contract ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Push provider mock seam (FCM / APNs) and deep-link payload contract verification | pending — notification ADR + cross-repo deep-link contract with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Identity provider mock seam (Apple / Google sign-in) | pending — cross-repo auth ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Subscription / entitlement sandbox verification (app-store sandbox flows and backend entitlement sync) | pending — cross-repo monetisation ADR with `navo-backend` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" · [`../chat/resources/app.project-state.md`](../chat/resources/app.project-state.md) "Open Questions" |
| Audio playback verification (background-audio, queue, caching/offline, interruption/handoff behaviour) | pending — audio approach research | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Localisation / RTL testing (English + Persian; later Arabic / Turkish) | pending — localisation ADR | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Cost-bounded testing for paid-provider stages (if app testing incurs paid usage — e.g., translation/audio probe runs) | pending — depends on provider mix and spend controls | [`01-working-rules.md`](01-working-rules.md) §6 |
| Pre-runtime topics (test runner config, CI test stage, coverage policy, device farm) | not applicable — no runtime exists | — |

> Topics are seeded from the hard rules in [`01-working-rules.md`](01-working-rules.md) and the architecture direction. New topics enter via planning conversations or audit responses per §4.

---

## 4. Doctrine evolution rules

Testing-strategy doctrine moves through the same discipline as architecture doctrine. The canonical evolution rules are in [`09-architecture.md`](09-architecture.md) §4. Summary:

1. **Proposing a change.** Inside a planning conversation, audit response, or work-package closure. Silent drift is not permitted.
2. **Accepting a change.** Product-owner approval, then a `docs:` commit.
3. **Binding precedence.** Working rules, ADRs, and scope baselines bind; doctrine describes. The binding source wins on conflict.
4. **Superseding.** Superseded rows stay with a status pointer to the replacement.
5. **Coverage check.** Audit runs and work-package closures validate testing-doctrine touchpoints; deviations and incidental violations are routed for product-owner decision.
