# AI-collab-skills

Claude skills for improving AI collaboration and memory usage over time.

## What this is

AI sessions are stateless by default. Each conversation starts fresh. Patterns don't carry. You do the same calibration work every time.

This repo is a growing collection of Claude skills designed to change that — practices for building AI collaboration that compounds session over session, not just improves within one.

## What's here

| Skill | What it does |
|---|---|
| `reflect` | Analyses how you worked together at session end. Surfaces patterns, failures, and collaboration signals. Proposes memory saves — routed correctly to CLAUDE.md or memory files. |
| `handoff` | Generates a copy-pasteable starter prompt before closing a long session. Carries decisions, context, and file references into the next conversation. |
| `log` | Chronicles each session as a narrative record. Builds an archive of how you work, what you've built, and how the collaboration is evolving. |
| `wrap` | End-of-session shortcut. Runs reflect → log in sequence, then asks if you want a handoff. One command to close a session cleanly. |

## How to use

These are Claude Code skills. Works with the VS Code extension, terminal CLI, and desktop app — all share the same `~/.claude/` config folder. Not supported on claude.ai web.

To install:

1. Clone this repo
2. Copy the skill folders into `~/.claude/skills/`
3. Invoke during sessions with `/reflect`, `/handoff`, `/log`, `/wrap`

New to Claude Code skills? See [SETUP.md](SETUP.md) for a full walkthrough.

Each skill has a `SKILL.md` with full instructions.

## How memory works

These skills use two types of storage. Using them correctly is the difference between patterns that compound and patterns that accumulate and never get applied.

### Behavioral rules → CLAUDE.md files

Rules about how Claude should behave belong in CLAUDE.md files:

- `~/.claude/CLAUDE.md` — global rules, auto-loaded in every session
- `{project}/CLAUDE.md` — project-specific rules, auto-loaded when working in that directory

CLAUDE.md files auto-load every session without being asked. Rules placed here are always active.

### Reference data → memory files

Long reference material, project state, career context, agent definitions — anything you want to look up rather than always apply — goes in memory files (e.g. `~/.claude/projects/*/memory/`).

Memory files are loaded on demand when referenced.

### The mistake to avoid

Saving behavioral rules to memory files. Rules buried in memory files don't get applied — they're not loaded unless Claude is explicitly asked to read them. After several sessions, you end up with a growing library of rules that Claude never sees.

`/reflect` handles this routing automatically. When it proposes memory saves, it classifies each one — behavioral rule (→ CLAUDE.md) or reference data (→ memory file) — so the right content ends up in the right place.

## How this grows

New skills get added as I build and test them. The scope stays focused: practices that make AI collaboration sharper, more continuous, and more intentional.

If you've built something that fits, open a PR. If you've improved one of these skills for your own workflow, share it. The goal is a library that evolves with how people actually work with AI.

## Contributing

- Fork the repo
- Add or improve a skill in its own folder: `skills/your-skill-name/SKILL.md`
- Open a PR with a short description of what it does and why it works

No gatekeeping. If it makes collaboration better, it belongs here.

## License

MIT

## Built by

Vishal Tandon — Exploring how AI changes the way we design, build and test product ideas.

[LinkedIn](https://www.linkedin.com/in/vishaltandon/) · [Substack](https://substack.com/@vishxdesign) · [Medium](https://medium.com/@vishx.design) · [Instagram](https://www.instagram.com/vishx.design/)
