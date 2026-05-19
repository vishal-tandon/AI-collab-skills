# Setup Guide

## What you need

These skills run inside **Claude Code** — Anthropic's AI coding assistant. Claude Code comes in three forms, all of which support skills:

| Environment | Notes |
|---|---|
| **VS Code extension** | Install the Claude Code extension from the VS Code marketplace |
| **Terminal (CLI)** | Run `claude` in your terminal |
| **Desktop app** | Available at [claude.ai/code](https://claude.ai/code) |

All three use the same `~/.claude/` config folder. Skills installed once work across all environments.

**Not supported:** Claude.ai web (claude.ai) is a different product and does not support skills.

---

## Install

```bash
git clone https://github.com/vishal-tandon/AI-collab-skills.git
cp -r AI-collab-skills/skills/* ~/.claude/skills/
```

Restart Claude Code. All four skills are immediately available.

**Verify the folder structure looks like this:**
```
~/.claude/skills/
├── reflect/
│   └── SKILL.md
├── handoff/
│   └── SKILL.md
├── log/
│   └── SKILL.md
└── wrap/
    └── SKILL.md
```

If skills aren't being detected, the most common cause is a missing subfolder — each skill needs its own folder, not a flat `SKILL.md` directly inside `skills/`.

---

## First session

Do some work in Claude normally. Then at the end of the session, type:

```
/reflect
```

Claude will analyse how the session went — where it misread your intent, what caused rework, what patterns it noticed. It will propose memory saves: learnings written to files Claude reads at the start of future sessions.

Read what it surfaces. Approve the saves that are right. Correct the ones that aren't. That's one cycle of the loop.

---

## Recommended daily workflow

```
Work in Claude as normal
↓
/wrap at end of session
```

`/wrap` runs `/reflect` → `/log` in sequence, then asks if you want `/handoff`. One command covers all three practices cleanly.

---

## What each skill does

**`/reflect`**
Analyses how you and Claude worked together — not what you built, but how. Surfaces patterns in your communication, where Claude misread intent, what would have been faster. Proposes learnings to write to memory. Run after a specific task or at session end.

**`/handoff`**
Generates a copy-pasteable starter prompt that carries full context into a new conversation: decisions locked, state current, next steps clear. Run when your context window is getting full or before you close a long session.

**`/log`**
Writes a narrative chronicle of the session — what you brought, what Claude built, who proposed what, where course was corrected. Writes to a project-level log and a global log across sessions. Run at end of session.

**`/wrap`**
Chains reflect → log in sequence, then asks if you want handoff. The recommended end-of-session habit.

---

## Memory

When `/reflect` proposes saves, it's writing learnings to files Claude reads at session start. Where those files go depends on your setup:

- If you use Claude Code's auto-memory system, saves go to `~/.claude/memory/` automatically
- If not, `/reflect` will present proposed saves as text — copy what's useful into your project's `CLAUDE.md` file manually

Either way works. The auto-memory system just makes it seamless.

---

## Customising skills

Each skill is a plain markdown file. Open it, edit it, save it. Changes take effect immediately — no restart needed.

Start with `/reflect` — add analysis dimensions that matter for how you work. The skill gets more useful the more precisely it describes what to look for.
