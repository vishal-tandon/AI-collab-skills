---
name: log
description: Chronicle the current session as a human-to-machine interaction log. Writes to two places: the active project's data/session-log.md (project interactions) and ~/claude-projects/session-log.md (global log across all projects and system-level changes). Use at end of session, or after /wrap runs it automatically.
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
---

# /log — Session Chronicle

Write a narrative record of this session as a human-to-machine interaction log. Not a technical summary — a story of who brought what, who proposed what, what changed, and why.

Writes to two logs. Both are required. Do not skip either.

---

## Step 1 — Identify the active project

Scan the conversation for project references: file paths touched, CLAUDE.md files read, project folder names mentioned. Identify which project this session belongs to.

If multiple projects were touched, pick the primary one (most file activity).

If no project is identifiable, skip Step 2 and Step 3 and go straight to Step 4.

---

## Step 2 — Write the project-level entry

Target: `{project-root}/data/session-log.md`

If the file does not exist, create it with this header:
```markdown
# Session Log — {Project Name}

Chronological record of sessions. Each entry captures the human-to-machine interaction — what was brought, asked, proposed, decided, and built.

---
```

Write a narrative entry:
```markdown
## {YYYY-MM-DD} — Session {N} — {short title}

{2-4 paragraphs in plain narrative prose.}
```

**What to capture at project level:**
- What the user came in with (musing, question, problem, half-formed idea) specific to this project
- What was asked and how Claude responded — first attempt vs what it took to land
- Where the user pushed back or redirected, and what that surfaced
- What was built, decided, or confirmed within the project
- What was left open in this project

---

## Step 3 — Append project log and confirm

Append the entry to `{project-root}/data/session-log.md`.

**Session number:** Count existing `## ` entries in the file to determine N. If file is new, N = 1.

---

## Step 4 — Write the global-level entry

Target: `~/claude-projects/session-log.md`

If the file does not exist, create it with this header:
```markdown
# Global Session Log

Chronological record of all sessions across projects. Captures what was worked on, what projects were touched, and any system-level changes — skills built, workflows changed, process decisions made.

---
```

Write a narrative entry:
```markdown
## {YYYY-MM-DD} — Session {N} — {short title}

{2-3 paragraphs in plain narrative prose.}
```

**What to capture at global level:**
- Which projects were touched and what happened in each (brief — detail lives in project log)
- Any system-level changes: skills built or modified, workflows redesigned, memory changes
- Cross-project decisions or patterns that emerged
- Meta-level shifts in how you and Claude are working together

**What to omit:**
- Project-specific detail already in the project log — summarise, don't repeat
- File paths and technical specifics

**Attribution rule (both logs):**
- Name the user when they initiated, questioned, pushed back, decided, or redirected
- Name Claude when proposing, building, recommending, or getting it wrong
- Show the transfer of thinking — where ideas came from the user, where Claude shaped them, where the user corrected course

**Tone (both logs):** Factual, neutral, clear. Not a celebration. Not a debrief. A record. Prose only — no bullet lists.

**Global session number:** Count existing `## ` entries in global log to determine N.

---

## Step 5 — Append global log and confirm

Append the entry to the global session log.

Output:
```
Logged.
- {project-root}/data/session-log.md — Session {N} appended
- ~/claude-projects/session-log.md — Session {N} appended
```

Then stop. No summary, no reflection, no additional output.
