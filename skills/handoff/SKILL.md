---
name: handoff
description: Generates a handoff prompt to continue the current conversation in a fresh context window. Reads active project state and session outcomes. Outputs a single copy-pasteable prompt. Use when context window is running low or before ending a long session.
argument-hint: "[--brief] — shorter output for simple sessions"
allowed-tools:
  - Read
  - Bash
  - Glob
  - Grep
---

<objective>
Synthesise the current session's work, decisions, and pending tasks into a compact handoff prompt.
The output must bring a fresh Claude session up to speed in one injection — no back-and-forth required.
Optimise for: completeness of locked decisions, clarity of next steps, brevity everywhere else.
</objective>

<execution>

Step 1 — Read active context
Read any CLAUDE.md or memory index file for the current project if one exists.
Read any project files directly relevant to what was worked on this session.
Skip if no project files are accessible.

Step 2 — Identify session outcomes
Scan the conversation for:
- Decisions made and locked (do not re-open in new session)
- Files created or modified
- Tasks completed
- Tasks pending / next steps
- Questions left unanswered

Step 3 — Read active project
For any project touched this session, read its CLAUDE.md or equivalent if it exists.

Step 4 — Build the handoff prompt

Structure:
```
# Handoff Prompt — [date]

## Context
[2-3 sentences: what you're working on and why. Current focus, relevant background.]

## What we accomplished this session
[Bulleted list — specific, not vague. Include file paths for anything created/modified.]

## Decisions locked — do not revisit
[Each decision on one line. What was decided and why.]

## Active project state
[One block per active project: name, stack, status, next step.]

## Next steps (in priority order)
[Numbered list. Specific enough that a fresh session can start executing immediately.]

## Key context
[3-5 bullets: the non-obvious things a new session needs to not repeat mistakes. Working preferences, constraints, decisions that shaped the approach.]
```

Step 5 — Output
Print the handoff prompt inside a single markdown code block so it can be copied cleanly.
Follow with: "Copy this into your next session to continue without re-explaining context."

</execution>

<constraints>
- Do not include full file contents — reference file paths instead
- Target length: 400-600 words in the output prompt (brief flag: 200-300 words)
- Every locked decision must appear — omitting one risks re-litigating it in the new session
- Next steps must be specific enough to act on immediately
</constraints>
