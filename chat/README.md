# chat/ — AI-Coordination Resources

This folder contains AI-coordination tooling for the multi-AI development workflow: resources uploaded to ChatGPT, and prompt templates pasted into Claude Code, ChatGPT, and (optionally) an audit tool such as Copilot.

## Not project authority

`chat/` is tooling, not project content. **Future Claude Code sessions must NOT treat anything in this folder as a source of truth for project state, scope, working rules, or planning unless explicitly asked.**

The live project authority remains:

- [`../CLAUDE.md`](../CLAUDE.md) — the dispatcher
- [`../docs/01-working-rules.md`](../docs/01-working-rules.md), [`../docs/02-git-workflow.md`](../docs/02-git-workflow.md), [`../docs/03-planning-model.md`](../docs/03-planning-model.md), [`../docs/04-runbook.md`](../docs/04-runbook.md) — operational discipline (always present)
- [`../docs/05-product-context.md`](../docs/05-product-context.md), [`../docs/06-feature-blueprint.md`](../docs/06-feature-blueprint.md), [`../docs/07-technical-direction.md`](../docs/07-technical-direction.md), [`../docs/08-scope.md`](../docs/08-scope.md) — project-shaping documents
- [`../docs/decisions/`](../docs/decisions/) — ADRs
- [`../docs/research/`](../docs/research/) — optional research and synthesis (when used)
- [`../context/tasks/`](../context/tasks/) and [`../context/sanity-checks/`](../context/sanity-checks/) — task files and readiness reports
- [`../context/design/`](../context/design/) — optional design-track working knowledge (when active)
- [`../context/product-source/`](../context/product-source/) — raw ideation; **not authority**
- [`../CHANGELOG.md`](../CHANGELOG.md)

If anything in `chat/` (especially `chat/resources/context.project-state.md`) conflicts with the live project documents above, the live documents win. The ChatGPT-side resources are point-in-time snapshots and may be behind reality.

---

## Structure

```
chat/
├── README.md                       (this file)
├── resources/                       ← uploaded to ChatGPT manually (5 files)
└── prompts/
    ├── bootstrap/                   ← one-time, for new projects from the template (removed at bootstrap closeout)
    ├── migration/                   ← one-time, for existing projects adopting the template (removed at migration closeout)
    ├── workflows/                   ← onboarding, bridges, project-state-update, customization, session-closure
    ├── execution/                   ← work-package closure
    └── review/                      ← audit cycle
```

### `resources/` — uploaded to ChatGPT

Five files manually uploaded into ChatGPT's project resource folder. ChatGPT auto-loads them at every onboarding.

- `context.project-state.md` — general project-state snapshot (Current Status, Next Action, Operational Constraints, Key Decisions, Progress Log)
- `communication.general.md` — communication philosophy, language rules, prompt-generation discipline
- `communication.claude.md` — Claude-specific behavior expectations
- `playbooks.workflows.md` — operational workflows (Planning, Implementation, Audit, Refinement, Work Package Close, Next Work Package, Design)
- `user-environment.md` — user-global / device-stable facts (OS, editor, package managers, language runtimes); **maintained from a canonical copy and synced across the user's projects**

The dot-prefix in filenames (e.g., `communication.claude.md`, not `communication-claude.md`) is intentional: ChatGPT shows resources as flat files without folder hierarchy, and the dot-naming preserves grouping in the ChatGPT-side UI.

**Maintenance loop:** edit here (source of truth) → upload or replace in ChatGPT's resource folder. Use `chat/prompts/workflows/claude-project-state-update.md` to ask Claude Code to apply incremental updates to `context.project-state.md`. `user-environment.md` is updated at the user's canonical copy and synced across all the user's projects — do not edit it from inside a single project's session unless the user explicitly asks.

When the design track is active, the user may also upload latest `design-status.md` from `context/design/` as a sixth resource — replaced (not edited) after each design-workflow update.

### `prompts/` — session prompt templates

Templates copied and pasted into chat sessions.

- `bootstrap/` — **one-time** prompts for spinning up a new project from this template:
  - `01-chatgpt-prepare-brief.md` — run in ChatGPT after discussing the project idea; produces a structured bootstrap brief.
  - `02-claude-customize.md` — run in Claude Code on the freshly-cloned new repo; consumes the brief and customizes the template.
  - **After bootstrap, both files (and the `bootstrap/` folder) are recommended for deletion** — the customization report names them as Category C (Bootstrap-only).
- `migration/` — **one-time** prompts for onboarding an existing project to the template's conventions (the brownfield counterpart of `bootstrap/`). See [`prompts/migration/README.md`](prompts/migration/README.md) for the full workflow and per-prompt details. After migration closeout, the entire `migration/` folder is deleted (along with `bootstrap/` if still present and the temporary `_migration/` snapshot at the target repo root).
- `workflows/` — cold-start onboarding (Claude / ChatGPT), session-mode bridges (planning / implementation / design-track), project-state update, session-closure sync.
- `execution/` — work-package closure.
- `review/` — audit run (auditor) → audit evaluation (ChatGPT) → audit response (ChatGPT).

Each template is recipient-aware: Claude-targeted prompts use command-style English; ChatGPT-targeted prompts include the Farsi-explanation and Writing Block conventions; auditor-targeted prompts use deterministic-validator framing.

---

## One-time entry workflows

Two disposable paths bring a project into the template's conventions:

- **Bootstrap** — for a brand-new project created from this template. Run [`prompts/bootstrap/01-chatgpt-prepare-brief.md`](prompts/bootstrap/01-chatgpt-prepare-brief.md) in ChatGPT, then [`prompts/bootstrap/02-claude-customize.md`](prompts/bootstrap/02-claude-customize.md) in Claude Code. On completion, bootstrap-only and migration-only prompts are removed.
- **Migration** — for an existing project adopting the template's conventions. The user manually copies the template into `_migration/` at the target repo root (locally excluded via `.git/info/exclude`, not tracked `.gitignore`), then runs the migration prompts in this sequence:
  1. [`prompts/migration/chatgpt-migration-onboarding.md`](prompts/migration/chatgpt-migration-onboarding.md) (ChatGPT-target) — orient and produce the migration brief (once, at the start).
  2. [`prompts/migration/claude-migration-onboarding.md`](prompts/migration/claude-migration-onboarding.md) (Claude-target) — orient at the start of every Claude-target session during migration.
  3. [`prompts/migration/01-claude-target-snapshot-intake.md`](prompts/migration/01-claude-target-snapshot-intake.md) (Claude-target) — verify the snapshot setup.
  4. [`prompts/migration/02-claude-target-inventory.md`](prompts/migration/02-claude-target-inventory.md) (Claude-target) — inventory target vs snapshot.
  5. [`prompts/migration/03-chatgpt-reconcile.md`](prompts/migration/03-chatgpt-reconcile.md) (ChatGPT-target) — draft the adoption plan.
  6. [`prompts/migration/04-claude-target-adopt.md`](prompts/migration/04-claude-target-adopt.md) (Claude-target) — execute adoption, one approved pass per session.
  7. [`prompts/migration/05-chatgpt-migration-closeout.md`](prompts/migration/05-chatgpt-migration-closeout.md) (ChatGPT-target) — draft the closeout directive.

  See [`prompts/migration/README.md`](prompts/migration/README.md) for prerequisites, hard rules, and per-prompt details. On completion, `_migration/` is deleted and bootstrap-only and migration-only prompts are removed.

Both paths are one-time, temporary entry workflows — not permanent modes. Bootstrap typically completes in a single Claude-target session; migration may span multiple sessions (one per approved adoption pass) within a bounded window. After either completes, the project continues through the normal long-lived development / design workflow.

If you are unsure which path applies: a brand-new repo with no real content uses bootstrap; a repo with existing docs, code, history, or decisions uses migration. Each entry workflow includes a defensive check that aborts if invoked against the wrong kind of repo.

---

## Bridge routing (which prompt to use)

After Claude's cold-start onboarding produces an orientation report with `Project state: <value>`, use the matching bridge or in-session continuation:

| Project state | Action / Bridge |
|---|---|
| `milestone-pending` | [`prompts/workflows/claude-milestone-opening-bridge.md`](prompts/workflows/claude-milestone-opening-bridge.md) — opens the next milestone (sub-routes for `not-created` / `roadmap-pending` / `opening-ready`, read from the orientation report's `Milestone-pending sub-state:` field) |
| `milestone-open-no-wp` | [`prompts/workflows/claude-planning-bridge.md`](prompts/workflows/claude-planning-bridge.md) — drafts the next WP's task file (sub-routes `wp-draft` / `wp-select-then-draft`, determined by whether `Next work package:` is concrete or `undefined`) |
| `wp-drafting` | Continue in-session — no bridge needed; finish drafting the task file and freeze it |
| `wp-frozen` | [`prompts/workflows/claude-implementation-bridge.md`](prompts/workflows/claude-implementation-bridge.md) — executes the frozen WP |
| `wp-executing` | Continue in-session if the session is already executing; use [`prompts/workflows/claude-implementation-bridge.md`](prompts/workflows/claude-implementation-bridge.md) if a previous session left work mid-execution and you're picking up cold |
| `wp-closing` | [`prompts/workflows/session-closure-sync.md`](prompts/workflows/session-closure-sync.md) — synchronizes project-state docs at closure |
| `milestone-closing` | [`prompts/workflows/session-closure-sync.md`](prompts/workflows/session-closure-sync.md) — same; the final WP's closure triggers the milestone close |
| `unknown` | Have Claude re-orient with a discrete `Project state` value; no bridge fires |

Each bridge validates `Project state` and aborts (with explicit redirect) if the state does not match its expected value. ChatGPT works from Claude's orientation report plus the resources uploaded to its session; it does not inspect the repo directly. When a needed fact is missing from the orientation report, the bridge either asks Claude to re-orient or instructs the generated prompt to have Claude perform the check.

---

## Conventions

- Filenames: kebab-case for prompt templates (`work-package-close.md`); dot-prefixed for ChatGPT resource files (intentional, as explained above).
- No emojis.
- No fixed section-number references in cross-doc citations — refer to sections by name; section numbers may shift.
- All files tracked in Git; standard project Git rules apply per [`../docs/02-git-workflow.md`](../docs/02-git-workflow.md), including the Operation ownership rule in §1.1 (Claude always drafts Git output; whether Claude also executes Git writes depends on the project's configured track).
