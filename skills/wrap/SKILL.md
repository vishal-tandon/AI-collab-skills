---
name: wrap
description: End-of-session skill. Chains reflect, log, and a git-state check in sequence. Use when wrapping up a session: captures learnings (reflect), chronicles the interaction (log), and reports uncommitted/unpushed repo state with an offer to checkpoint (git-state). After all three complete, asks if the user wants to run /handoff. Trigger on /wrap, "let's wrap", "wrap up the session", "end of session".
allowed-tools:
  - Read
  - Write
  - Bash
  - Glob
  - Grep
---

# /wrap — Session Wrap

Runs reflect, log, and a git-state check in sequence. Do not skip any step. Do not compress or merge them: each has a distinct job.

---

## Step 1 — Reflect

Use the Skill tool to invoke the `reflect` skill in full-session mode.

Follow reflect exactly: scan the conversation, output the full reflection, present proposed memory saves, wait for the user to approve or skip each one, then write the approved saves.

Do not proceed to Step 2 until reflect is complete and memory saves are resolved.

---

## Step 2 — Log

Use the Skill tool to invoke the `log` skill.

Follow log exactly: detect the active project, write the narrative session entry in the User/Claude format, append to `data/session-log.md`, confirm with "Logged. Session {N} appended."

Do not summarise or repeat reflect output here. Log is a separate, independent chronicle.

---

## Step 3 - Git state

For each repo touched this session, run `git status --short` and, where an upstream exists, `git log @{u}.. --oneline`. Report uncommitted and unpushed state for every repo touched, not just the most recently edited one.

Then offer scoped checkpoint commits: stage by explicit path only, never a blanket `git add`, commit as a resumable checkpoint. Never push or open a PR unless the user asks.

Do this because work often spans parallel sessions or continues in another context window: git debt accumulates silently unless it is surfaced here.

---

## Step 4 - Handoff prompt

After the git-state step completes, output exactly:

```
Wrap complete. Want to run /handoff to prepare the next session? (y/n)
```

If the user says yes: use the Skill tool to invoke the `handoff` skill.
If the user says no: stop.

---

## Constraints

- Never merge reflect and log output: they serve different purposes
- Never skip log because "reflect already covered it": they are not the same
- Never push or open a PR in the git-state step unless the user asks: a commit is a resumable checkpoint, not a finalize
- Never add commentary between steps: just execute and move to the next
