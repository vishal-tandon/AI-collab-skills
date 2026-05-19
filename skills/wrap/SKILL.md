---
name: wrap
description: End-of-session skill. Chains reflect then log in sequence. Use when wrapping up a session. Trigger on /wrap, "let's wrap", "wrap up the session", "end of session".
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
---

# /wrap — Session Wrap

Runs two skills in sequence. Do not skip either step. Do not compress or merge them — each has a distinct job.

---

## Step 1 — Reflect

Use the Skill tool to invoke the `reflect` skill in full-session mode.

Follow reflect exactly: scan the conversation, output the full reflection, present proposed memory saves, wait for the user to approve or skip each one, then write the approved saves.

Do not proceed to Step 2 until reflect is complete and memory saves are resolved.

---

## Step 2 — Log

Use the Skill tool to invoke the `log` skill.

Follow log exactly: detect the active project, write the narrative session entry, append to `data/session-log.md`, confirm with "Logged. Session {N} appended."

Do not summarise or repeat reflect output here. Log is a separate, independent chronicle.

---

## Step 3 — Handoff prompt

After log confirms, output exactly:

```
Wrap complete. Want to run /handoff to prepare the next session? (y/n)
```

If yes: use the Skill tool to invoke the `handoff` skill.
If no: stop.

---

## Constraints

- Never merge reflect and log output — they serve different purposes
- Never skip log because "reflect already covered it" — they are not the same
- Never add commentary between steps — just execute and move to the next
