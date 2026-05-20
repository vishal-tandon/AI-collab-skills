---
name: handoff
description: Generates a handoff prompt to continue the current conversation in a fresh context window. Reads active memory, project state, and session outcomes. Outputs a single copy-pasteable prompt. Use when context window is running low or before ending a long session.
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

Step 1 — Read memory index and active files
If a memory index exists (e.g. MEMORY.md at `~/.claude/projects/*/memory/MEMORY.md`), read it.
Read only project or reference memory files that were explicitly mentioned or worked on this session. Derive the list from the conversation — do not read all files.
Skip any behavioral feedback files (e.g. `feedback_*.md`) — behavioral rules belong in CLAUDE.md files which auto-load in the next session. Including them in the handoff is redundant.

Step 2 — Identify session outcomes
Scan the conversation for:
- Decisions made and locked (do not re-open in new session)
- Files created or modified
- Tasks completed
- Tasks pending / next steps
- Questions left unanswered

Step 3 — Read active project
For any project touched this session, read its CLAUDE.md if it exists.

Step 4 — Build the handoff prompt

Structure:
```
# Handoff Prompt — [date]

## Who I am
[2-3 sentences: role, background, current focus.]

## What we accomplished this session
[Bulleted list — specific, not vague. Include file paths for anything created/modified.]

## Decisions locked — do not revisit
[Each decision on one line. What was decided and why, in one sentence.]

## Active projects and their state
[One block per active project: name, stack, status, next step.]

## Next steps (in priority order)
[Numbered list. Specific enough that a fresh session can start executing immediately.]

## Key context for working with me
[3-5 bullets covering non-obvious context from THIS session only — things not already covered by ~/.claude/CLAUDE.md or the project CLAUDE.md, which auto-load in the next session. Do not repeat global rules. Focus on: session-specific decisions, nuance that came up, constraints that shaped the approach.]
```

Step 5 — Output
Print the handoff prompt inside a single markdown code block so it can be copied cleanly.
Follow with: "Copy this into your next session to continue without re-explaining context."

</execution>

<constraints>
- Do not include full file contents — reference file paths instead
- Do not repeat rules already captured in CLAUDE.md files — they auto-load
- Target length: 400-600 words in the output prompt (brief flag: 200-300 words)
- Every locked decision must appear — omitting one risks re-litigating it in the new session
- Next steps must be specific enough to act on immediately
</constraints>
