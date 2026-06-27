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
Prescribe WHAT and WHY (current state, locked decisions, next action), not HOW. Assume a capable executor that will read the linked source docs and the auto-loaded CLAUDE.md files; prefer pointers to source-of-truth files over inlining their content or restating methodology.
</objective>

<execution>

Step 1 — Read memory index and active files
If a memory index exists (e.g. MEMORY.md at `~/.claude/projects/*/memory/MEMORY.md`), read it.
Read only project or reference memory files that were explicitly mentioned or worked on this session. Derive the list from the conversation — do not read all files.
Skip any behavioral feedback files (e.g. `feedback_*.md`) — behavioral rules belong in CLAUDE.md files which auto-load in the next session. Including them in the handoff is redundant. ALSO exclude any rule saved to a project CLAUDE.md THIS session — it auto-loads in-project. Before writing the prompt, list everything written to any CLAUDE.md this session and exclude all of it; restating it creates a redundant second source of truth that drifts.

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

## Read first (source of truth)
[Pointer list: the session's source-of-truth files to read before acting — key state docs, specs, build logs touched this session. Note that global + project CLAUDE.md auto-load. Trust these files over the recap below; the recap is an index, not a substitute.]

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
[Apply the cut-test FIRST: include a bullet ONLY if a fresh session that reads the auto-loaded CLAUDE.md files (global + project) AND the linked Read-first docs would still miss it. Exclude anything saved to any CLAUDE.md this session and all global/project rules (they auto-load). If everything is covered there, write "None — see CLAUDE.md + the Read-first docs." Otherwise 1-3 bullets of genuinely session-only nuance not yet written down (current mindset/priority, a rule nuance that arose mid-session).]
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
