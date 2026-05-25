# ChatGPT: Milestone-opening bridge — generate milestone-opening prompt for Claude

## For the user

- **What this is:** the prompt sent to ChatGPT after Claude has reported orientation that no milestone is currently `open`. ChatGPT validates alignment, reads the sub-route from the orientation report, then drafts a clean milestone-opening prompt you'll send to Claude.
- **When to use:** between Claude's orientation report and Claude's milestone-opening conversation, when the orientation report has `Project state: milestone-pending`.
- **Target AI:** ChatGPT.
- **Input:** Claude's orientation report — paste it into the marker block in the prompt body.
- **Output:** alignment check (Farsi, brief) plus a final English milestone-opening prompt for Claude delivered as a ChatGPT Canvas document (or equivalent editable single-document panel).

--- COPY FROM BELOW (after pasting Claude's orientation report into the marker block) ---

I have onboarded Claude Code in a new session and Claude has full access to the project code and documentation. Claude's orientation report is in the marker block below.

Your goal: **validate alignment**, **read the milestone-opening sub-route from the orientation report**, then **generate a clean milestone-opening prompt for Claude** that opens the next milestone safely and without premature status changes.

This template is for **development-mode milestone opening only**. Do not use it for design-mode work, brand / identity work, visual exploration, or design-documentation refinement.

<<<CLAUDE_ORIENTATION_REPORT>>>
[Paste Claude's orientation report here, replacing this line.]
<<<END_CLAUDE_ORIENTATION_REPORT>>>

If the marker block is empty, do NOT proceed. Stop and tell the user the orientation report is missing.

You do NOT have direct access to the repository. Everything you know about the repo state comes from Claude's orientation report and from the project resources uploaded to this session. If a needed fact is missing, ask for a clearer orientation from Claude — do not attempt repo inspection yourself.

## What to do

1. **State guard (BEFORE any alignment check):**
   - Read the orientation report's `Project state:` field.
   - If `Project state` is `milestone-pending`: continue to step 2.
   - If `Project state` is anything else (`milestone-open-no-wp`, `wp-drafting`, `wp-frozen`, `wp-executing`, `wp-closing`, `milestone-closing`, `unknown`): STOP. Tell the user which bridge applies (`claude-planning-bridge.md`, `claude-implementation-bridge.md`, `session-closure-sync.md`, or re-orient with a discrete `Project state`).
   - In every STOP case, do NOT generate any prompt for Claude.

2. **Read the sub-route directly from the orientation report's `Milestone-pending sub-state:` field:**
   - `not-created` → proceed to the `not-created` sub-route below.
   - `roadmap-pending` → proceed to the `roadmap-pending` sub-route below.
   - `opening-ready` → proceed to the `opening-ready` sub-route below.
   - `unknown` or missing → STOP. Tell the user to have Claude re-orient with a discrete sub-state value (or to specify the sub-route explicitly). Do NOT generate any prompt.

3. **Check alignment (concise):**
   - Does Claude's understanding match the project state (purpose, milestone status, next action)?
   - Flag only real mismatches or ambiguities (if any). Keep it short.

4. **Surface scope-baseline status (informational, not blocking) using a two-track approach:**
   - Read the orientation report's `Scope baseline:` field, if present.
   - If `scaffold`: include this note in the bridge's output AND in the generated prompt's preamble — *"Scope baseline at `docs/08-scope.md` is still in template-scaffold state. You may author it now in a separate conversation, or treat it as a candidate work package under this milestone. The bridge is not blocking on it."*
   - If `authored`: no note needed.
   - If the field is missing or `unknown`: include this instruction in the generated prompt — *"Before drafting, please check the current state of `docs/08-scope.md` (or its renamed-per-phase variant `docs/08-<phase>-scope.md`). If still in template-scaffold state, surface this in your report so the user can decide whether to author it separately or as a candidate WP under this milestone."*
   - You (ChatGPT) do NOT inspect repo files; either the orientation report carries the status, or Claude performs the check in the generated prompt.

5. **Generate the milestone-opening prompt for Claude** following the sub-route logic below.

## Requirements for the generated prompt

Follow the communication rules in project resources:
- `communication.general.md`
- `communication.claude.md`

Keep the prompt concise, non-directive (no assumptions of correctness), and scope-safe (no implementation).

Keep it development-mode only:
- do not treat design documents as implementation authority
- do not start visual / design-system work or design handoff

### Sub-route: `not-created`

Before generating the prompt, ChatGPT MUST ask the user to confirm the next milestone's `<NN>` and `<slug>`. The bridge does NOT auto-derive these — milestone numbering is append-only per `docs/03-planning-model.md` §8, and the slug is a project decision. ChatGPT MAY suggest values based on previously-closed milestones visible in the orientation report (e.g. *"Previous closed: 00-bootstrap-foundation; suggest `01-<slug>` as next — please confirm the number and slug"*), but does not decide unilaterally.

Once the user has confirmed concrete `<NN>` and `<slug>`, the generated prompt MUST:
- direct Claude to copy `context/tasks/_milestone-template/README.md` into the new sibling folder `context/tasks/<NN>-<slug>/README.md` using the user-confirmed values (no `<NN>-<slug>` placeholder leak in the generated prompt — the bridge substitutes the concrete values before generating)
- fill the placeholders in the new README (`Number`, `Slug`, `Folder`); leave `Status: placeholder`, no `Started:` date yet
- DO NOT flip status in this step; this sub-route only scaffolds the folder
- have Claude report back with the new folder path and recommend the next step (continue to `roadmap-pending` shape, or stop and let the user direct)

### Sub-route: `roadmap-pending`

The generated prompt MUST:
- direct Claude to fill the milestone README's `Purpose` (1–3 paragraphs drawing from the brief / current project state), `Roadmap shape (recorded YYYY-MM-DD)` section (with today's date), and `Work packages` table rows with candidate WPs at `Status: roadmap (not frozen)` per `docs/03-planning-model.md` §9
- DO NOT flip `Status` to `open` in this step; status stays `placeholder`
- have Claude report back with the updated README path, the candidate WP slugs proposed, and a recommendation for the next step (status flip → `opening-ready`, or further roadmap refinement)

### Sub-route: `opening-ready`

The generated prompt MUST:
- explicitly require user confirmation in the form: *"About to flip milestone `<NN-slug>` to `Status: open` with `Started: <today>`. Confirm `yes` to proceed."*
- only after explicit `yes`: flip the milestone README's `Status: placeholder` → `Status: open` and set `Started: YYYY-MM-DD` to today's date
- DO NOT modify any other section of the README in this step
- have Claude report back confirming the status flip and the next step (typically: ready for the first WP planning conversation, which uses `claude-planning-bridge.md`)

### Common constraints (all sub-routes)

The prompt MUST:
- be development-mode only (no design-mode work)
- forbid implementation, runtime code, dependencies, infrastructure
- include the scope-baseline note from step 4 of "What to do" above, when applicable
- defer Git operation ownership to `docs/02-git-workflow.md` §1.1; defer AI-attribution policy to `docs/02-git-workflow.md` §1.2
- require Claude's chat output to be a brief summary plus the path(s) of files created or updated — no full file content pasted inline

## Output format (STRICT)

### 1) Alignment Check (Farsi, very brief)
- 2–4 bullets max
- Only mismatches/notes (if any). If none, say it's aligned.
- If the state guard or sub-route determination failed, surface this prominently — do NOT proceed to §2.

### 2) Prompt for Claude (English, final only)
- Provide **only the final prompt** to send to Claude.
- Deliver it as a ChatGPT Canvas document (or equivalent editable single-document panel) so the user can copy it cleanly; the canvas body must contain only the prompt itself — no preamble, no commentary.

## Do NOT

- Do NOT generate any prompt if the state guard fails, the sub-state is `unknown` or missing, or the `not-created` sub-route's `<NN>-<slug>` is unconfirmed.
- Do NOT inspect repo files yourself — you do not have repo access; rely on the orientation report or have Claude perform the check in the generated prompt.
- Do NOT skip the user-confirmation requirement in the `opening-ready` sub-route.
- Do NOT flip milestone status in the same conversation as the roadmap-pending fill — these are separate explicit steps.
- Do NOT treat scope-baseline status as a blocker; surface it as a note only.
- Do NOT auto-derive the next milestone's `<NN>` or `<slug>`; require user confirmation.
- Do NOT include long explanations or restate the full project context.
- Do NOT mix milestone opening with WP planning — WP planning uses `claude-planning-bridge.md`.
- Do NOT include multiple prompt variants.

## Goal

Produce a minimal, production-ready milestone-opening prompt for the correct sub-route. The generated prompt opens the milestone safely, makes scope-baseline status visible without blocking, and never flips status without explicit confirmation.
