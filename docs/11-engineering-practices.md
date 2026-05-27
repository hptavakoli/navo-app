# Navo App — Engineering practices (living doctrine)

**Status:** Scaffold. Doctrine is filled in over time as engineering practices firm up. Pre-`01-foundation`, established topics derive from existing hard rules, Git workflow, and planning discipline; toolchain-touching topics are pending until a runtime exists.

> Living engineering-practices doctrine for Navo App. Records current expectations about how work is done — change discipline, dependency management, secrets handling, environment isolation, review cadence, Git posture. References binding sources rather than replacing them. Evolves through planning and review workflows; does not bind execution by itself — binding power lives in [`01-working-rules.md`](01-working-rules.md), [`02-git-workflow.md`](02-git-workflow.md), and [`03-planning-model.md`](03-planning-model.md).

This document is the **living engineering-practices counterpart** to:

- [`01-working-rules.md`](01-working-rules.md) — operational rules (binding).
- [`02-git-workflow.md`](02-git-workflow.md) — Git workflow (binding).
- [`03-planning-model.md`](03-planning-model.md) — planning discipline (binding).
- [`decisions/`](decisions/) — binding ADRs.
- [`08-scope.md`](08-scope.md) — frozen phase baseline.

Doctrine here **describes**; the binding sources above **bind**. Doctrine updates flow through the workflow in §4 below — see [`09-architecture.md`](09-architecture.md) §4 for the canonical evolution rules.

---

## 1. Purpose

This document maintains a live, descriptive index of how Navo App does engineering: which practices are settled, which are pending decisions, and which are explicitly not yet applicable. It is the place an AI session or human reviewer goes to ask "what does this project's day-to-day engineering look like?" without reconstructing it from working rules and Git workflow.

The doctrine does not introduce new rules. It restates and indexes what the binding sources commit the project to.

---

## 2. Relationship to other documents

| Document | Role | Relationship to doctrine |
|---|---|---|
| [`01-working-rules.md`](01-working-rules.md) | Operational rules (binding) | Doctrine restates rules as practices and points back. The working rules win on conflict. |
| [`02-git-workflow.md`](02-git-workflow.md) | Git workflow (binding) | Doctrine restates the Git operating posture (Conventional Commits, logical-unit commits, no AI attribution, approval-gated operations). |
| [`03-planning-model.md`](03-planning-model.md) | Planning discipline (binding) | Doctrine restates the planning cadence and the frozen-task-file precondition for implementation. |
| [`decisions/`](decisions/) | ADRs (binding) | Engineering-touching ADRs (CI, build, signing, store-deploy, dev-environment, tooling) are sources for topics here. |
| [`08-scope.md`](08-scope.md) | Frozen phase baseline | Per-phase practice overlays live in scope; doctrine is the cross-phase view. |

---

## 3. Engineering-practices topics

Each topic carries a `Status` (`established` / `pending` / `not applicable`) and a `Source` pointer.

| Topic | Status | Source |
|---|---|---|
| Documentation-first (technical concerns land in a doc before code) | established | [`01-working-rules.md`](01-working-rules.md) §2, §3 |
| Small, reviewable changes; no large unreviewable batches | established | [`01-working-rules.md`](01-working-rules.md) §2 |
| No silent scope expansion | established | [`01-working-rules.md`](01-working-rules.md) §2 |
| No runtime, dependencies, or package-manager files without explicit approval | established | [`01-working-rules.md`](01-working-rules.md) §2 |
| ADR lifecycle (Proposed → Accepted → Superseded) | established | [`01-working-rules.md`](01-working-rules.md) §3 · [`03-planning-model.md`](03-planning-model.md) §14 |
| Conventional Commits; logical-unit commits; no commit-per-task | established | [`02-git-workflow.md`](02-git-workflow.md) §4, §5 |
| Approval-gated Git operation ownership | established | [`02-git-workflow.md`](02-git-workflow.md) §1.1 |
| No AI attribution in GitHub-facing output | established | [`02-git-workflow.md`](02-git-workflow.md) §1.2 |
| Secrets in env vars only; `.env` gitignored before first commit | established | [`01-working-rules.md`](01-working-rules.md) §6 |
| Cost discipline (EUR 10/day soft guideline; provider-dashboard hard cap) | established | [`01-working-rules.md`](01-working-rules.md) §6 |
| Daily spend monitoring when paid providers are in use | established | [`01-working-rules.md`](01-working-rules.md) §6 |
| Isolated local environments (`.venv/<purpose-slug>/` or language equivalent — for the app side, this also covers per-target build/tool isolation) | established | [`01-working-rules.md`](01-working-rules.md) §8 |
| Per-work-package readiness reports as closure evidence | established | [`01-working-rules.md`](01-working-rules.md) §7 · [`../context/sanity-checks/README.md`](../context/sanity-checks/README.md) |
| Independent audit pass for non-trivial WPs (encouraged) | established | [`01-working-rules.md`](01-working-rules.md) §7 |
| Frozen task file required before implementation begins | established | [`01-working-rules.md`](01-working-rules.md) §2 · [`03-planning-model.md`](03-planning-model.md) §9 |
| Design and implementation modes do not mix | established | [`01-working-rules.md`](01-working-rules.md) §9 |
| Research immutability + versioned syntheses (when research track is active) | established | [`01-working-rules.md`](01-working-rules.md) §3 · [`research/README.md`](research/README.md) |
| CI / build / lint / format toolchain (Flutter analyzer, formatter, build pipeline) | pending — `01-foundation` ADR(s) | — |
| Pre-commit hook configuration | pending — `01-foundation` ADR(s) | — |
| Language / runtime version pinning conventions (Flutter SDK and Dart pin policy) | pending — depends on stack choices in `01-foundation` | [`07-technical-direction.md`](07-technical-direction.md) "Open questions" |
| Dependency pinning policy specifics (`pubspec.lock` shape, sticky-parameter pins) | pending — depends on stack choices | [`01-working-rules.md`](01-working-rules.md) §4 |
| Code signing posture (iOS provisioning profiles, Android signing keys, secrets storage for signing material) | pending — `01-foundation` signing/CI ADR | — |
| Store-deploy pipeline (App Store Connect / Google Play Console; release-channel discipline; beta/staged-rollout posture) | pending — store-deploy ADR | — |
| Pre-runtime topics (test runner config, code-coverage policy, release-automation hooks, device-farm posture) | not applicable — no runtime exists | — |

> Topics are seeded from binding rules already in place. New topics enter via planning conversations or audit responses per §4.

---

## 4. Doctrine evolution rules

Engineering-practices doctrine moves through the same discipline as architecture doctrine. The canonical evolution rules are in [`09-architecture.md`](09-architecture.md) §4. Summary:

1. **Proposing a change.** Inside a planning conversation, audit response, or work-package closure. Silent drift is not permitted.
2. **Accepting a change.** Product-owner approval, then a `docs:` commit.
3. **Binding precedence.** Working rules, Git workflow, ADRs, and scope baselines bind; doctrine describes. The binding source wins on conflict.
4. **Superseding.** Superseded rows stay with a status pointer to the replacement.
5. **Coverage check.** Audit runs and work-package closures validate engineering-doctrine touchpoints; deviations and incidental violations are routed for product-owner decision.
