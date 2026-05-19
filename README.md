# AI-collab-skills

Three Claude Code skills for building a compounding AI collaboration system. Each session builds on the last — your AI gets better at working with you the more you use it.

---

## Skills

| Command | Name | What it does |
|---------|------|--------------|
| `/reflect` | Reflect | Analyses how you worked together — not what you built, but how. Surfaces patterns, writes learnings to memory. |
| `/new-chat` | Handoff | Generates a copy-pasteable starter prompt that carries full context to a new conversation. |
| `/log` | Log | Writes a narrative chronicle of the session — what was brought, what was built, who proposed what. |
| `/wrap` | Wrap | Runs Reflect → Log in sequence, then asks if you want Handoff. One command at end of session. |

---

## Install

Requires [Claude Code](https://claude.ai/code).

```bash
git clone https://github.com/vishal-tandon/AI-collab-skills.git
cp -r AI-collab-skills/skills/* ~/.claude/skills/
```

Restart Claude Code. All four commands are live.

---

## Use

```
/reflect          — run after a task or at end of session
/new-chat         — run when context window is full, before closing
/log              — run to chronicle what happened this session
/wrap             — runs all three in sequence (recommended daily habit)
```

---

## How it compounds

Running these three practices across sessions builds something that no single session could:

- **Reflect** surfaces patterns in how you work. Those patterns become rules encoded into your project files.
- **Handoff** carries those rules and current state into every new session — no context lost, no momentum broken.
- **Log** builds an archive of every interaction — interrogatable, case-study-ready.

After 10 sessions: the AI knows your communication patterns, your decision style, what causes rework.

After 50 sessions: the logs become data. Find patterns across months. Build case studies from recorded sessions. Track how your collaboration evolves.

The practices compound because they feed each other — better constraints → better outputs → better reflections → better constraints.

---

## Customise

These are starting points. Edit the SKILL.md files to match how you work:

- Add analysis dimensions to `/reflect` that matter for your domain
- Adjust the handoff template in `/new-chat` to match your project structure
- Extend the log format in `/log` to capture what you want tracked

The system compounds because you keep improving it.

---

MIT License. Fork, extend, build beyond.
