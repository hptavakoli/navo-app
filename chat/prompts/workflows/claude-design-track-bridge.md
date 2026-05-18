# ChatGPT: Design-track bridge — generate design-track prompt for Claude

## For the user

- **What this is:** the prompt sent to ChatGPT after Claude has reported orientation in a design-oriented session. ChatGPT validates design-track alignment, then decides whether to generate a Claude prompt and (if appropriate) drafts it.
- **When to use:** in design-oriented sessions, between Claude's orientation report and the next design-track action.
- **Target AI:** ChatGPT.
- **Input:** Claude's orientation / status report — paste it into the marker block in the prompt body.
- **Output:** alignment check + next-step assessment (Farsi, brief), and (only if appropriate) a final English design-track prompt for Claude in a Writing Block.

--- COPY FROM BELOW (after pasting Claude's report into the marker block) ---

I have onboarded Claude Code in a new design-oriented session and Claude has access to the project code and documentation. Claude's orientation / status report is in the marker block below.

Your goal: **validate Claude's design-track alignment**, then decide whether a clean next prompt for Claude should be generated.

Use the project resources, especially:
- `app.project-state.md` → general project state
- latest `app.design-status.md` when available → current design-track status / next-action candidates
- `communication.general.md`
- `communication.claude.md`
- `playbooks.workflows.md`

If latest `app.design-status.md` appears stale compared with the user's supplied context, flag possible staleness instead of silently inferring.

<<<CLAUDE_ORIENTATION_REPORT>>>
[Paste Claude's orientation / status report here, replacing this line.]
<<<END_CLAUDE_ORIENTATION_REPORT>>>

If the marker block is empty, do NOT proceed. Stop and tell the user the report is missing.

## What to do

1. **Check alignment (concise):**
   - Does Claude's understanding match the current general project state and latest design-track status?
   - Does Claude correctly preserve the design / development boundary?
   - Does Claude avoid treating design docs as implementation authority?
   - Does Claude avoid starting visual design / design-system handoff unless explicitly authorised?
   - Flag only real mismatches, stale assumptions, or ambiguities.

2. **Assess the next design-track step:**
   - If the next action is clear and already authorised by the user, generate the prompt for Claude.
   - If multiple reasonable options exist, briefly explain them and do **not** force a prompt unless the user clearly asked for one.
   - If Claude suggests a next action, validate it against latest `app.design-status.md` and project resources; do not accept it blindly.
   - If the right action is only a status / sanity check / closure step, keep the generated prompt limited to that.

3. **Generate a Claude prompt only when appropriate:**
   - The prompt must match the selected design-track action.
   - The prompt must be in English.
   - The prompt must be concise, non-directive, scope-safe, and design-mode only.

## Requirements for the generated prompt

Follow:
- `communication.general.md`
- `communication.claude.md`

The prompt MUST preserve these boundaries:
- Design docs are product / design working knowledge, not implementation authority.
- Do not start development planning, task-file authoring, code work, or implementation.
- Do not start design-system handoff, visual generation, layout / component work, or design-system generation unless explicitly requested.
- Do not edit `app.design-status.md` from ChatGPT-side workflows; status updates should happen through Claude / repo workflow and then be replaced in ChatGPT resources.
- If a patch is requested, it must be explicitly scoped to design documentation and must not broaden silently.
- If the action is audit-only, Claude must not edit files.

## Common design-track prompt types

Use the smallest appropriate type:

- **Audit / alignment report only** — checking a brief, canon section, surface map, or status consistency. Must explicitly forbid patches / file edits.
- **Targeted design-doc patch authorization** — only after an audit was reviewed and the user authorised a patch. Must state exact file(s), scope, and review decisions.
- **Sanity / cluster alignment check** — checking several recently edited docs for contradictions or drift. Must avoid cosmetic churn.
- **Inventory / status report** — when current state is unclear or a cold-start / closure check is needed. Must not edit files unless separately authorised.
- **Design-track closure / synchronization check** — near session end to verify docs, tracker, and working tree state. Must not start the next design action.
- **Pre-flight / canonical decision framing** — for unresolved design-system or design-canon decisions. Must frame options and implications before asking for implementation or visual work.
- **Design handoff input preparation** — only when explicitly authorised. Must be based on reviewed design-track status and pre-flight readiness.

## Output format (STRICT)

### 1) Alignment Check (Farsi, very brief)
- 2–4 bullets max
- Only mismatches / notes / confirmations that matter
- If aligned, say it is aligned

### 2) Next-Step Assessment (Farsi, brief)
- State whether the next action is clear or whether multiple options exist
- If multiple options exist, list only the realistic options and why
- If no prompt should be generated yet, say so clearly

### 3) Prompt for Claude (English, only if appropriate)
- Provide only one final prompt
- Follow `communication.claude.md` rules
- Place it inside a Writing Block with `variant="chat_message"` so it can be easily copied
- If no prompt is appropriate yet, omit this section or state that no Claude prompt should be generated yet

## Prompt shape (guide)

When generating a prompt for Claude, adapt to the action type, but generally include:
- Acknowledge Claude's orientation / previous report
- State the selected design-track action
- Define scope boundaries clearly
- State whether files may or may not be edited
- Preserve design / development separation
- Ask Claude to reason from repo docs, not chat memory
- Require a structured report at the end
- Ask Claude not to start the next action unless explicitly told

## Do NOT

- Do NOT generate implementation prompts
- Do NOT generate development planning prompts
- Do NOT ask for task-file drafts
- Do NOT treat design documents as implementation authority
- Do NOT start design-system handoff or visual generation unless explicitly authorised
- Do NOT force a next prompt when the next design action is ambiguous
- Do NOT include multiple prompt variants
- Do NOT include long explanations
- Do NOT restate the whole project context
- Do NOT duplicate detailed design-track status from `app.design-status.md`

## Goal

Produce a minimal, clear, design-track-coordination prompt the user can copy directly into Claude — or surface a brief next-step assessment if no prompt is appropriate yet.
