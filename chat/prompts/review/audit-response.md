# ChatGPT: Audit response — generate response message for Claude

## For the user

- **What this is:** the prompt sent to ChatGPT after the `audit-evaluation` step, to generate a clean, neutral response message you'll send to Claude.
- **When to use:** after `audit-evaluation.md` has produced its assessment and you've decided which points are worth surfacing to Claude.
- **Target AI:** ChatGPT.
- **Input:** Claude's implementation report, the auditor's audit report, and any decisions / clarifications from the audit-evaluation conversation — all already present in the current ChatGPT conversation context. No paste needed.
- **Output:** a single English message in a Writing Block, ready to copy and send to Claude. Neutral observations / questions, not directive instructions.

--- COPY FROM BELOW ---

You are generating a response message to Claude based on the audit cycle. The Claude implementation report, the auditor's audit report, and any decisions / clarifications from the prior audit-evaluation step are already in this conversation.

Follow `communication.general.md` and `communication.claude.md`.

## Rules

- Do NOT be directive
- Do NOT assume audit findings are correct
- Rephrase all audit points as **neutral observations / questions**
- Merge ALL relevant points from the conversation (no omissions)
- Remove redundancy and noise
- Keep it concise and high-signal

## Data inputs

- Claude implementation report → present in this conversation context
- Auditor's audit report → present in this conversation context
- Audit-evaluation decisions / clarifications → present in this conversation context (may span multiple messages)

You MUST:
- extract all relevant points
- consolidate duplicates
- preserve intent and coverage

## Output (STRICT)

Produce one clean message in a Writing Block with `variant="chat_message"` so it can be copied directly to Claude. No explanations or meta text outside the Writing Block.

The message body should follow this shape (adapt phrasing as needed):

```
Good work on completing this work package — the implementation and coverage look solid.

We ran an independent review to validate alignment with the defined tasks and constraints. A few points came up that are worth reviewing.

[Rephrased observations]

- Convert each audit point into a neutral observation, e.g.:
  - "We noticed a potential gap in … which might lead to …"
  - "There may be a mismatch between … and …"
  - "This area could introduce risk under … conditions"

[Optional context / decisions]

- Integrate any decisions or clarifications from the conversation
- Keep them neutral and concise

Please review these points and share:
- Where you agree or disagree
- Your reasoning for each point
- Whether any adjustments are actually needed or already handled implicitly

If adjustments are needed, feel free to apply the file changes and share a brief summary of what changed. Git operation ownership follows `docs/02-git-workflow.md` §1.1 (configured track). Always draft commit messages or PR text per `docs/02-git-workflow.md` §4 / §4.1; whether you execute the operation depends on §1.1. Do not include AI attribution in any GitHub-facing text per `docs/02-git-workflow.md` §1.2.
```

Requirements:
- Cover ALL meaningful audit findings
- Do NOT copy-paste raw audit text
- Do NOT use directive language (e.g., "fix", "must", "change")
- Keep the message concise; remove redundancy

## Goal

Produce one clean, neutral response message that surfaces the audit findings to Claude without forcing conclusions or undermining Claude's reasoning autonomy.
