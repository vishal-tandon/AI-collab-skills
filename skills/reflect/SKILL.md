---
name: reflect
description: Run a reflection on work done in this session. Invoke when the user types /reflect, "reflect on this", "reflect on the last task", "what did we learn", "what went well", "let's reflect", or after a task or feature is completed. Two modes: full session (default) or last task only. Analyse the conversation autonomously — no questions to the user. Surface what worked, what didn't, what would have been faster, and how the user can prompt more effectively. Always show proposed memory saves before writing them.
---

# /reflect — Reflection

Two modes. Detect which one from the prompt:

| Trigger | Mode |
|---|---|
| `/reflect` or "reflect on this session" or "reflect on our work" | **Full session** — scan entire conversation |
| "reflect on the last task" or "reflect on that" or "reflect on [specific thing]" | **Last task** — scope to the most recent distinct task only |

---

## Scoping the last task

When in **last task** mode: scan backwards from the most recent message to find where the task started. A new task begins when the user shifts topic — "now let's...", "ok next...", "can you help me with...", or starts a clearly unrelated thread. Everything before that boundary is out of scope.

State the boundary at the top of the output:
`> Scoped to: [brief description, e.g. "building the /reflect skill"]`

---

## What to analyse

Scan every prompt the user sent and every response Claude gave within the scope.

**1. Prompt patterns**
- What types of prompts did the user send? (exploratory, directive, corrective, clarifying)
- What was the real intent — did Claude pick it up immediately or require iterations?
- Where did the user have to re-explain, rephrase, or correct?

**2. Iteration forensics**
- Every time the user sent a follow-up to fix or redirect: what caused it?
- Root cause: wrong assumption, missing context, scope creep, Claude over-engineered, Claude under-read intent, Claude asked unnecessary questions, Claude missed a stated preference
- How many turns were spent recovering from each misstep?

**3. What Claude got wrong**
- Concrete failures: wrong format, wrong scope, wrong tone, wrong depth, ignored constraints
- Recurring pattern? (e.g. always over-explains, always misses visual preferences, always asks questions already answered)
- Stored memory misapplication: did Claude apply a saved rule too literally, ignoring context where nuance was needed? If a correction was needed because Claude was too binary with a stored rule, flag it and propose a nuance update to that memory.

**4. Efficiency opportunities**
- What could have been resolved in one turn that took three?
- What context, if the user had provided upfront, would have eliminated back-and-forth?
- What did Claude do that was unnecessary — over-researching, over-scaffolding, unrequested features, verbose responses?
- What could Claude have inferred from memory or context but didn't?
- Did this session resume from a handoff? If so, were any docs or outputs already drafted in a previous session that Claude regenerated unnecessarily?

**5. What works with you vs what doesn't**
- Methods that worked: approaches that got fast approval or good flow
- Methods that didn't: patterns that caused friction, rework, or pushback
- The user's decision style as observed in this scope

**6. Collaboration dynamics — the broader lens**
Go beyond session bugs. Ask: what does this session reveal about *how the user works* at a structural level?
- What does the user always do after Claude delivers X? (e.g. asks for controls, asks for a variant, asks to embed somewhere)
- What does the user never do? (e.g. never gives full spec upfront, never asks for explanations, never wants options they didn't ask for)
- What should Claude pre-empt next time this type of task comes up?
- What is the user's role in the loop — curator/director, spec-writer, or something else?
- What delivery model fits best for this type of work?
- If a new collaborator joined and asked "how does this user like to work?", what would you tell them from this session alone?

This is the most important section. Surface workflow patterns, not just one-off fixes.

**6b. Repeatable patterns synthesis**
Cross-reference "what went well" and "what didn't work" to identify patterns that appear across both directions:
- Things that worked in this session that also appear in existing memory as confirmed working methods — was the method applied correctly? Does it need reinforcement?
- Things that failed in this session that also appear in memory as known failure modes — was a saved rule ignored, misapplied, or missing nuance?
- For each pattern found: Is it already saved? If yes, was it applied correctly this session? If not, does it need a new save?
- Explicitly hunt for confirmed working methods that lack a memory entry — not just corrections, but positive patterns that reliably produce good outcomes and should be locked in so future sessions don't drift.

This step exists to correct the bias toward saving only failures. Equally valuable: "this approach reliably works, save it so we don't regress."

**7. Agent opportunities**
Scan for tasks that were repetitive, multi-step, or well-defined enough to be delegated:
- Was any task in this session a strong candidate for an existing agent or workflow?
- Was an existing agent used? Did it perform well — or did Claude end up doing the work inline instead?
- Were there repeatable task patterns that don't yet have an agent but should? (e.g. "every time we do X, Claude does Y and Z in sequence")
- What would the agent need to know, do, and produce to handle this task reliably?

If a new agent is warranted: name it, describe its job in one sentence, and list what it reads/writes.

**8. Source fidelity** *(content sessions only)*
When content was derived from a source document (article, musing, brief), check that derived pieces haven't introduced framing the source doesn't support:
- Does the most extractable/quotable line in the derived post accurately represent the source thesis?
- Did any reframe during iteration shift the core claim?
- Would a reader who only saw the derived post walk away with the right understanding of the source argument?

Flag any drift as a specific correction, not a general note.

**9. Pre-build prerequisites** *(visual/technical sessions)*
Before any implementation task, check — did Claude confirm the necessary constraints before writing the first line?
- Platform constraints: target dimensions, format, safe zones
- Role of the artifact: what job does it do?
- Format confirmation: was the format explicitly agreed before build started?

Flag any build that started without these as a preventable efficiency loss.

---

## Output format

```
## Reflect — [brief description of scope]
> Scoped to: [last task description]   <- only in last-task mode

### What went well
[2-5 bullets. Specific moments, not generalities.]

### What didn't work
[2-5 bullets. Name the failure, cause, and cost in turns/rework.]

### What would have been faster
[Actionable tips — some for Claude, some for the user. Direct.]

### Methods that work with you
[Patterns that reliably got fast approval or good flow. Specific.]

### Methods that don't work with you
[Patterns that caused friction. Honest.]

### Collaboration dynamics
[The broader lens — not session bugs, but structural patterns about how the user works.
- What the user always does after Claude delivers X
- What Claude should pre-empt next time this type of task appears
- What the delivery model should look like for this kind of work
- What a new collaborator would need to know about how the user operates
Be specific. This section should generate the most durable memory saves.]

### Repeatable patterns
[Cross-section synthesis. For each pattern:]
- **[Pattern name]** — Seen in: [what went well / what didn't]. Already saved? [yes/no]. Action: [save / update / already covered].

### Agent opportunities
[Tasks from this session that could be delegated to an existing or new agent.
- Existing agent fit: was there a task an agent should have handled but didn't?
- New agent candidate: repeatable task pattern with a clear input/output?
- If new: name, one-sentence job description, what it reads/writes.
Skip if nothing warrants it — "Agent opportunities: none this session."]

### Proposed memory saves
[For each proposed save, use the routing format below. Flag whether it comes from a failure or a confirmed working method.]

**[→ ~/.claude/CLAUDE.md | → project/CLAUDE.md | → memory: filename.md]**
> Section: [section name to add/update, for CLAUDE.md saves]
> Source: [failure correction / confirmed working method]
> Rule or content: [exact text to embed or save]

Save these? You can approve all, skip any, or edit before I write.

### Skill improvement?
[After every reflect run, check: did this session surface a new analysis dimension not covered by this skill's categories?

Ask:
- Was there a type of failure this session that didn't fit neatly into any existing category?
- Is there a recurring pattern in how the user gives feedback that should be tracked as a named category?
- Would a future reflect run catch more if it explicitly looked for X?

If yes: propose a concrete addition to this skill. Name the new category, describe what to look for, give an example from this session.
If no: output "Skill improvement: none this session."

This section is how the reflect skill improves itself over time.]
```

---

## Memory routing — where each save goes

Before proposing any save, classify using this decision tree:

```
Is this a behavioral rule (how Claude should act)?
  ├─ Yes, applies in every project → embed in ~/.claude/CLAUDE.md
  ├─ Yes, applies only in this project → embed in project CLAUDE.md
  └─ No, it's reference data → save as memory file
```

**The core mistake to avoid:** saving behavioral rules to memory files. Memory files get loaded on demand — rules buried there don't get applied. Rules in CLAUDE.md files auto-load every session and are always active.

### Route 1: `~/.claude/CLAUDE.md` (global, always active)

Use for:
- Communication style, output format, decision patterns
- How the user gives feedback, how they work
- Universal workflow rules that apply across every project

How to embed: open `~/.claude/CLAUDE.md`, find the relevant section, add inline. One sentence + When to apply. Read the file first to avoid duplicates.

### Route 2: Project `CLAUDE.md` (auto-loaded when in that directory)

Use for:
- Domain-specific rules for this project (e.g. design system rules, copy patterns)
- Project-specific workflow or tool rules

How to embed: open the project's CLAUDE.md, add to the relevant section.

### Route 3: Memory file (reference data only)

Use only for: project state, historical context, long reference material, agent definitions.

**Save as global memory** when:
- Reference data that applies across projects
- Complex reference too long to embed in CLAUDE.md inline

**Save as project memory** when:
- Project decisions, history, or status

**Don't save** when:
- Already captured (update instead)
- One-off that won't recur
- Behavioral rule that belongs in CLAUDE.md

---

## Tone

Sharp colleague doing an honest debrief — not a consultant, not a therapist. No filler. Direct. Only useful if honest about what went wrong and what should change. Don't soften failures.

---

## After approval

Once the user confirms which saves to write:
1. For CLAUDE.md saves: open the file, find the right section, embed inline
2. For memory file saves: write the file, update MEMORY.md index if one exists
3. Update existing entries rather than creating duplicates
4. Confirm: "Saved [n] rules. [list of destinations]"
