---
name: reflect
description: Run a reflection on work done with Vishal. Invoke when the user types /reflect, "reflect on this", "reflect on the last task", "what did we learn", "what went well", "let's reflect", or after a task or feature is completed and the user wants to capture learnings. Two modes: full session (default) or last task only. Analyse the conversation autonomously — no questions to the user. Surface what worked, what didn't, what would have been faster, and how Vishal can prompt more effectively. Always show proposed memory saves before writing them.
---

# /reflect — Reflection

Two modes. Detect which one from the prompt:

| Trigger | Mode |
|---|---|
| `/reflect` or "reflect on this session" or "reflect on our work" | **Full session** — scan entire conversation |
| "reflect on the last task" or "reflect on that" or "reflect on [specific thing]" | **Last task** — scope to the most recent distinct task only |

---

## Scoping the last task

When in **last task** mode: scan backwards from the most recent message to find where the task started. A new task begins when Vishal shifts topic — "now let's...", "ok next...", "can you help me with...", or starts a clearly unrelated thread. Everything before that boundary is out of scope.

State the boundary at the top of the output:
`> Scoped to: [brief description, e.g. "building the /reflect skill"]`

---

## What to analyse

Scan every prompt Vishal sent and every response Claude gave within the scope.

**1. Prompt patterns**
- What types of prompts did Vishal send? (exploratory, directive, corrective, clarifying)
- What was the real intent — did Claude pick it up immediately or require iterations?
- Where did Vishal have to re-explain, rephrase, or correct?

**2. Iteration forensics**
- Every time Vishal sent a follow-up to fix or redirect: what caused it?
- Root cause: wrong assumption, missing context, scope creep, Claude over-engineered, Claude under-read intent, Claude asked unnecessary questions, Claude missed a stated preference
- How many turns were spent recovering from each misstep?

**3. What Claude got wrong**
- Concrete failures: wrong format, wrong scope, wrong tone, wrong depth, ignored constraints
- Recurring pattern? (e.g. always over-explains, always misses visual preferences, always asks questions already answered)
- Stored memory misapplication: did Claude apply a saved rule too literally, ignoring context where nuance was needed? If a correction was needed because Claude was too binary with a stored rule, flag it and propose a nuance update to that memory.

**4. Efficiency opportunities**
- What could have been resolved in one turn that took three?
- What context, if Vishal had provided upfront, would have eliminated back-and-forth?
- What did Claude do that was unnecessary — over-researching, over-scaffolding, unrequested features, verbose responses?
- What could Claude have inferred from memory or context but didn't?

**5. What works with Vishal vs what doesn't**
- Methods that worked: approaches that got fast approval or good flow
- Methods that didn't: patterns that caused friction, rework, or pushback
- Vishal's decision style as observed in this scope

**6. Collaboration dynamics — the broader lens**
Go beyond session bugs. Ask: what does this session reveal about *how Vishal works* at a structural level?
- What does Vishal always do after Claude delivers X? (e.g. asks for controls, asks for a variant, asks to embed somewhere)
- What does Vishal never do? (e.g. never gives full spec upfront, never asks for explanations, never wants options he didn't ask for)
- What should Claude pre-empt next time this type of task comes up?
- What is Vishal's role in the loop — curator/director who tunes visually, not spec-writer?
- What delivery model fits best — sandbox with controls → final values → clean embed?
- If a new collaborator joined and asked "how does Vishal like to work?", what would you tell them from this session alone?

This is the most important section. Surface workflow patterns, not just one-off fixes.

**6b. Repeatable patterns synthesis**
Cross-reference "what went well" and "what didn't work" to identify patterns that appear across both directions:
- Things that worked in this session that also appear in existing memory as confirmed working methods — was the method applied correctly? Does it need reinforcement?
- Things that failed in this session that also appear in memory as known failure modes — was a saved rule ignored, misapplied, or missing nuance?
- For each pattern found: Is it already saved in memory? If yes, was it applied correctly this session? If not, does it need a new save?
- Explicitly hunt for confirmed working methods that lack a memory entry — not just corrections, but positive patterns that reliably produce good outcomes with Vishal and should be locked in so future sessions don't drift

This step exists to correct the bias toward saving only failures. Equally valuable: "this approach reliably works, save it so we don't regress."

**7. Agent opportunities**
Scan for tasks that were repetitive, multi-step, or well-defined enough to be delegated:
- Was any task in this session a strong candidate for an existing agent (e.g. career-writer, systems-architect, content agents)?
- Was an existing agent used? Did it perform well — or did Claude end up doing the work inline instead?
- Were there repeatable task patterns that don't yet have an agent but should? (e.g. "every time we do X, Claude does Y and Z in sequence")
- What would the agent need to know, do, and produce to handle this task reliably?

If a new agent is warranted: name it, describe its job in one sentence, and list what it reads/writes. Surface as a proposed memory save to `master_agent.md` or a new `role_*.md` file.

**8. Source fidelity** *(content sessions only)*
When content was derived from a source document (article, musing, brief), check that derived pieces haven't introduced framing the source doesn't support:
- Does the most extractable/quotable line in the derived post accurately represent the source thesis?
- Did any reframe during iteration shift the core claim — from paradigm-level insight to personal-level win, or vice versa?
- Would a reader who only saw the derived post walk away with the right understanding of the source argument?

Flag any drift as a specific correction, not a general note.

**9. Pre-build prerequisites** *(visual/technical sessions)*
Before any implementation task, check — did Claude confirm the necessary constraints before writing the first line?
- Platform constraints: target dimensions, aspect ratio, file format, safe zones
- Role of the artifact: what job does it do (supplement, summarise, prove, attract)?
- Format confirmation: was the format explicitly agreed before build started?

Flag any build that started without these as a preventable efficiency loss. Ask: "What did Claude know before writing the first line?" If the answer is "not enough," surface it here with the turn cost.

**10. Session health check** *(long or multi-domain sessions)*
When the session created substantial new systems (multiple CLAUDE.md updates, new skill files, new validators, new architecture), check:
- Was /wrap suggested proactively, or did Vishal have to ask?
- Did compaction occur before the session was captured — what knowledge is at risk?
- When should /wrap have been triggered, and what would have been preserved?

Flag any session where Vishal had to ask for /wrap as a missed proactive trigger.

---

## Output format

```
## Reflect — [brief description of scope]
> Scoped to: [last task description]   ← only in last-task mode

### What went well
[2–5 bullets. Specific moments, not generalities.]

### What didn't work
[2–5 bullets. Name the failure, cause, and cost in turns/rework.]

### What would have been faster
[Actionable tips — some for Claude, some for Vishal. Direct.]

### Methods that work with you
[Patterns that reliably got fast approval or good flow. Specific.]

### Methods that don't work with you
[Patterns that caused friction. Honest.]

### Collaboration dynamics
[The broader lens — not session bugs, but structural patterns about how Vishal works.
- What Vishal always does after Claude delivers X
- What Claude should pre-empt next time this type of task appears
- What the delivery model should look like for this kind of work
- What a new collaborator would need to know about how Vishal operates
Be specific. This section should generate the most durable memory saves.]

### Repeatable patterns
[Cross-section synthesis. For each pattern:]
- **[Pattern name]** — Seen in: [what went well / what didn't]. Already in memory? [yes/no]. Action: [save / update / already covered].

### Proposed memory saves
[For each proposed save, flag whether it comes from a failure or a confirmed working method:]
**[GLOBAL / PROJECT: project-name / TASK-SPECIFIC (not saving)]**
> File: feedback_xyz.md or project_xyz.md
> Type: feedback / project / user
> Source: [failure correction / confirmed working method]
> Content preview:
> [Exact content that would be written — title, body, Why, How to apply]

Save these to memory? You can approve all, skip any, or edit before I write.

### Skill opportunities — Part 1: New skills

Scan for each type. If yes: name the skill, one-line job, trigger, inputs, outputs.

| Type | Question |
|------|----------|
| **Process** | Was there a repeatable sequence of steps done manually that needs enforcing? (Signal: Claude or Vishal had to remember what came next, or steps were done out of order.) |
| **Output template** | Was there a fixed-input → fixed-output generation task done by hand? (Signal: same output shape produced multiple times, or will recur every time event X occurs.) |
| **Quality gate** | Was there a check that needed to happen before proceeding but was skipped, forgotten, or inconsistently applied? (Signal: Vishal caught something Claude should have caught first.) |
| **Workflow automation** | Was there a sequence of tool or platform operations always done together? (Signal: Claude ran 3+ steps manually that always fire in the same order.) |
| **Transformation** | Was there a content or artifact conversion from one format to another that will recur? (Signal: took X, produced Y in a different format.) |
| **Tool setup** | Was there a tool requiring configuration or invocation that will come up again? (Signal: Claude had to figure out how to use something that will recur.) |
| **Reference/lookup** | Was there a consistent question answered from a known source that will recur? (Signal: Claude searched or read the same type of source for the same type of question.) |
| **Agent coordination** | Was there orchestration of multiple steps or agents done manually that could be sequenced reliably? (Signal: Claude acted as orchestrator; a skill could replace it.) |

Self-editing reflect: only propose a new analysis category when the same uncovered dimension appears across 3+ sessions.
If nothing qualifies for a type: skip it.

### Skill opportunities — Part 2: Skill convergence

For every skill invoked this session, or any pattern done manually that matches an existing skill:

| Skill | Turns to approval | Root cause of iterations |
|-------|------------------|--------------------------|
| [skill name or "manual: pattern name"] | [N turns] | [what caused each correction] |

For any skill requiring more than 1 turn:
- What assumption did the skill make that was wrong?
- What rule, constraint, or worked example — if added to the skill definition — would have produced approved output on turn 1?
- Propose the exact addition: quote the line to add, name the section it goes in.

Skills approved in 1 turn: note them. That is the target state.

Convergence signal: same skill, 3+ sessions, consistently 1 turn = skill is mature.
Regression signal: skill that was 1 turn last session is now 3+ turns = something changed, audit it.]
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

**Behavioral rules go into CLAUDE.md files. NOT memory files. Rules in memory files don't get read.**

### Route 1: `~/.claude/CLAUDE.md` (global, always active)
- Communication style, output format, decision patterns
- How Vishal gives feedback, how he works
- Universal gates (key detail arrives last, scope before copy, etc.)
- Workflow patterns (session end, branching, GitHub setup)

How to embed: open `~/.claude/CLAUDE.md`, find the relevant section, add inline. One sentence + How to apply. Read file first to avoid duplicates.

### Route 2: Project `CLAUDE.md` (auto-loaded when in that dir)
- `content-master/CLAUDE.md` — carousel build, copy patterns, visual system
- `career/CLAUDE.md` — CV writing rules, career material tone
- Any project with domain/tool-specific rules

How to embed: open project CLAUDE.md, add to relevant section. Check section exists before creating new.

### Route 3: Memory file (reference data only)
Only for: project state, historical context, career data, agent definitions, reference material too long for CLAUDE.md.

**Save as global memory** (`~/.claude/projects/-home-vishal/memory/`) when:
- Reference data applying across projects
- Complex reference with many sub-rules that would bloat CLAUDE.md

**Save as project memory** when:
- Project state, decisions, history

**Don't save** when:
- Already captured (update instead)
- One-off that won't recur
- Behavioral rule that belongs in CLAUDE.md

### Proposed save format

```
**[→ ~/.claude/CLAUDE.md | → content-master/CLAUDE.md | → memory: filename.md]**
> Section: [section name to add/update]
> Source: [failure correction / confirmed working method]
> Rule: [exact text to embed]
```

---

## Tone

Sharp colleague doing an honest debrief — not a consultant, not a therapist. No filler. Direct. Only useful if honest about what went wrong and what should change. Don't soften failures.

---

## After approval

Once Vishal confirms which saves to write:
1. For CLAUDE.md saves: open the file, find the right section, embed inline
2. For memory file saves: write the file, update MEMORY.md index
3. Update existing entries rather than creating duplicates
4. Confirm: "Saved [n] rules. [list of destinations]"
