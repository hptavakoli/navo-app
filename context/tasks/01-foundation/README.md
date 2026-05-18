# Milestone: Foundation

> **Number:** `01`
> **Slug:** `foundation`
> **Folder:** `01-foundation/`
> **Status:** placeholder
> **Started:** — (set when milestone moves to `open`)
> **Target close:** `v0.1.0` (informational; versions and milestones are decoupled per [`../../../docs/03-planning-model.md` §15](../../../docs/03-planning-model.md))

## Purpose

This milestone establishes the app foundation before implementation-heavy work begins. It validates or revises the proposed Flutter direction, clarifies app responsibility boundaries against `navo-backend` and `navo-website`, defines first UX/product surfaces, identifies backend API dependencies, and produces the ADRs and research needed before runtime development can start.

Foundation precedes the first implementation milestone for app scaffolding, onboarding, feed, audio, and core navigation decisions. It follows the bootstrap context that initialised this repository from the foundation template; it precedes a future implementation milestone whose slug and scope will be decided here.

Work in this milestone is **documentation-first**. No package manager files, runtime structure, or application code lands during Foundation — all such decisions become ADRs and research outputs first, per [`../../../docs/01-working-rules.md` §2](../../../docs/01-working-rules.md).

## Relationship to versions

Versions and milestones are decoupled per [`../../../docs/03-planning-model.md` §15](../../../docs/03-planning-model.md). `v0.1.0` is the first planned tag for this repository; the milestone close is *marked* by a tagged version, not *caused* by tagging. Whether `v0.1.0` lands at the close of Foundation, partway through, or at the open of a successor milestone is a planning decision recorded here when the milestone moves to `open`.

## Work packages

| # | Slug | Status | Closes at | Task file | Readiness report |
|---|---|---|---|---|---|
| — | — | — | — | — | — |

No work packages are opened yet. Foundation is currently `placeholder` — the first task of the planning conversation is to identify and outline the work packages that will execute against the milestone intent above. Likely candidates (per the brief's open questions, to be confirmed at planning time):

- App framework validation (Flutter vs native Android/iOS, React Native, other alternatives) — foundation ADR.
- App surface strategy (Flutter Web / desktop in roadmap, or deferred in favour of `navo-website` for web) — product/technical ADR.
- State management and architecture (state library, routing/navigation, dependency injection, testing approach) — app technical ADR.
- Localisation strategy (English/Persian first; RTL UI; later Arabic/Turkish/other expansion; terminology handling) — localisation ADR.
- Auth integration (Apple/Google sign-in; identity mapping to backend user profiles) — app-backend auth ADR (cross-repo with `navo-backend`).
- API contract dependency (feed, content, audio, search, bookmark, notification, subscription) — backend/app contract planning; ADRs in each affected repo.
- Audio player approach (background audio, queue behaviour, caching/offline, platform constraints) — audio UX/technical research.
- Push notification approach (FCM/APNs, token handling, preferences, deep-links) — notification ADR.
- Subscription integration (RevenueCat vs app-store-native; sync with backend entitlements) — monetisation/subscription ADR (cross-repo).
- MVP app scope for the next milestone — milestone-scoping decision.

Order, granularity, and parallelism are decided at planning time. Candidates above are not frozen tasks; they are inputs to the planning conversation that will produce the actual numbered work packages.

## Roadmap shape

To be authored when the milestone moves from `placeholder` to `open`. At that point each work package gets a 2–4 sentence description here, plus a note on the Git operating mode for the milestone (the default per-WP short-lived-branch + PR model in [`../../../docs/02-git-workflow.md` §2/§3/§6/§12](../../../docs/02-git-workflow.md), or the conditional milestone-branch policy at [`../../../docs/02-git-workflow.md` §14](../../../docs/02-git-workflow.md)).

## Notes

- This is a **multi-repo workspace milestone**. Several Foundation decisions have cross-repo implications and should be landed as ADRs in each affected repo: auth integration (with `navo-backend`), API contract (with `navo-backend`), subscription/entitlement integration (with `navo-backend`), notification deep-link contract (with `navo-backend`). Other Foundation decisions are repo-local: Flutter framework choice, state management library, routing, RTL/localisation handling, audio player architecture.
- **Flutter is proposed, not final.** See [`../../../docs/07-technical-direction.md`](../../../docs/07-technical-direction.md). The first task of Foundation is to validate or revise the proposed framework direction; no runtime work begins until that validation lands as an ADR.
- This README follows the milestone-README template at [`../../../docs/03-planning-model.md` §12](../../../docs/03-planning-model.md). It remains the source of truth for current status, dependencies, and parallelism notes.
