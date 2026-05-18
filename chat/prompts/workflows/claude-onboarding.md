# Claude: Cold-start onboarding

## For the user

- **What this is:** the prompt sent to Claude Code at the start of a fresh session to orient it before any planning or execution.
- **When to use:** at session start, before asking Claude to plan, execute, audit, or close anything.
- **Target AI:** Claude Code.
- **Input:** none — Claude reads the project on its own from `CLAUDE.md` and the docs.
- **Output:** a brief orientation report covering project purpose, current milestone / work-package status, design-track status (if relevant), and the immediate next step (or that direction is awaited).

--- COPY FROM BELOW ---

You're joining cold — no prior context. Orient yourself via CLAUDE.md and the context/ layer (the docs define what to read and in what order).

# Session mode
SESSION_MODE = design-oriented | development-oriented

Apply the design / development reading rules below according to the selected mode.

If this session is design-oriented, follow the design-mode router in CLAUDE.md and use the latest `app.design-status.md` when available for current design-track status. Read `context/design/design-overview.md` as the design entry point.

If this session is development-oriented, follow the default development reading path and skip `app.design-status.md` unless the task explicitly depends on design-track state.

Do not propose or plan any work yet — just confirm you have the full picture:
- what the project is
- what the current milestone and work-package status is
- whether a design-track status / next design action is recorded, if relevant to the session mode
- what the immediate next step is, or whether it is awaiting direction

Flag anything ambiguous, inconsistent, or out of date.

## Required structured fields (orientation report — top of report)

Your orientation report MUST begin with four discrete fields. Downstream bridges parse these to route correctly; fuzzy prose breaks the routing.

- `Project state:` exactly one of:
  - `milestone-pending` — no milestone is currently `open` (a `placeholder` milestone may exist, or the next milestone's folder may not exist yet)
  - `milestone-open-no-wp` — a milestone is `open`; no active WP
  - `wp-drafting` — a WP task file exists with `Status: planned` and `Frozen:` blank (planning conversation in progress)
  - `wp-frozen` — a WP task file is frozen (`Frozen:` date set); execution not yet started
  - `wp-executing` — a WP task file is `Status: active`; execution in progress
  - `wp-closing` — a WP's readiness report is being authored
  - `milestone-closing` — final WP of a milestone is closed; milestone-close PR / tag / Release pending
  - `unknown` — state cannot be determined from the repo. Do NOT guess. Set `unknown` and surface what is ambiguous.
- `Active milestone:` the milestone slug (e.g. `00-bootstrap-foundation`) if exactly one milestone is `open`; `none` if zero milestones are `open`. If multiple are `open` (unusual), flag this in the report — do not pick one.
- `Next work package:` the WP slug if defined; `undefined` if `Project state` is `milestone-open-no-wp` but no next WP has been picked; `n/a` for states where the field does not apply (`milestone-pending`, `milestone-closing`, `unknown`).
- `Milestone-pending sub-state:` REQUIRED when `Project state` is `milestone-pending`; otherwise `n/a`. Exactly one of:
  - `not-created` — the next milestone's folder does NOT exist yet at `context/tasks/<NN>-<slug>/`. Typical when a previous milestone closed and the next has not been scaffolded.
  - `roadmap-pending` — the milestone folder exists with the README's `Purpose` still in template-scaffold form, OR the `Work packages` table still contains template example rows. Status is `placeholder`.
  - `opening-ready` — the milestone README has real content (`Purpose` filled, `Roadmap shape` recorded, `Work packages` table populated with real candidates). Status is still `placeholder`.
  - `unknown` — sub-state cannot be determined.

Optional (include only if you naturally surfaced it during orientation; otherwise omit and let the consuming bridge ask Claude to verify):
- `Scope baseline:` one of `scaffold` (current `docs/08-scope.md` or `docs/08-<phase>-scope.md` still in template form), `authored` (real content), or `unknown`.

Report back once oriented.
