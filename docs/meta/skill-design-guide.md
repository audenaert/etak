# Etak Skill & Agent Design Guide

This guide is for anyone authoring skills or agents in the Etak marketplace. It covers the harder question: **what makes a skill good?**

## Why this guide exists

A skill file is not documentation. It's a prompt — it gets loaded into Claude's context every time the skill is invoked, and every line shapes behavior. A skill that reads well to humans but over-instructs the model will produce worse output than a skill that's sparer but better-targeted.

The guidelines below come from auditing [`etak-discovery`](../../discovery/) and [`etak-develop`](../../develop/) against Claude Code best practices and Etak's philosophy. They take strong positions on purpose: a style guide that won't rule anything out doesn't help anyone ship good work.

## Etak's philosophy in five lines

Skills and agents embody a specific stance:

- **Receptive** — hold space for pre-formal thinking; don't force structure too early
- **Generative** — propose, notice, surface; don't wait to be asked
- **Grounded** — read the material before speaking about it (code for development, graph for discovery)
- **Confirming, not committing** — show artifacts before writing; the user owns the commit
- **Honest about uncertainty** — flag what's guessed vs. what's grounded; invalidation is success

If a skill or agent drifts from this posture — e.g., becomes a silent automator or a generic chatbot — it's drifted from what Etak is for.

## Plugin anatomy at a glance

```
<your-plugin>/
├── .claude-plugin/plugin.json       # manifest (see plugin-schema.json)
├── README.md                        # overview + relationship to other etak plugins
├── skills/
│   ├── <skill>/
│   │   ├── SKILL.md                 # the prompt (frontmatter + body)
│   │   └── references/              # on-demand supporting docs
│   └── _internal/
│       ├── core/                    # shared foundation (schemas, guidelines)
│       └── <artifact>/              # per-artifact writing skills
└── agents/
    └── <agent>.md                   # single-file agent (frontmatter + body)
```

Conventions worth internalizing:

- Plugin name in the manifest carries the `etak-` prefix; directory name omits it.
- User-facing skills live at the top of `skills/`. Internal skills (artifact writers, shared foundation) live under `skills/_internal/` and set `user-invocable: false`.
- A skill's `references/` directory is loaded on demand — put long-form guidance there, not in `SKILL.md`.

## Anatomy of a skill

```yaml
---
name: <kebab-case>
description: >
  What this skill does, framed as when to reach for it.
when_to_use: >
  "trigger phrase", "another trigger phrase", "a third one"
model: sonnet                       # haiku | sonnet | opus
effort: high                        # low | medium | high | xhigh | max
context: fork                       # only for autonomous, summarizing work
user-invocable: false               # set on internal skills only
allowed-tools: Read, Write, Edit, Glob, Grep
---

# <Skill Name>

<One-paragraph purpose statement — what this skill helps with and for whom.>

## Your Stance

<How the model shows up when this skill runs. Receptive, generative,
adversarial, rigorous-but-pragmatic. Load-bearing — see G2.>

## <The working content>

<Operational rules, the moves the skill runs, references to deeper guides.>

## What <Skill> Does NOT Do

<Boundaries. What belongs to a sibling skill. See G4.>

## Transitions

<Specific next-skill handoffs with triggering user behavior. See G5.>
```

## Anatomy of an agent

Agents are single markdown files in `agents/`. The frontmatter is the same shape as a skill plus `context: fork` (agents almost always fork) and typically an expanded `allowed-tools` list. The body is longer because agents run autonomously and hold themselves to a process the user isn't watching — but every process step still has to earn its line (see G12).

## The Guidelines

### G1. A skill file is a prompt, not documentation

Every line in `SKILL.md` consumes token budget and shapes behavior. If a line neither changes what Claude does nor prevents drift in an edge case, it's narrative tax — move it to `docs/` or the plugin README.

**How to test a line:** can you point at a concrete behavior the line produces or prevents? If no, cut it.

**Do:**
> You help PMs discover what their ideas are resting on — probing beneath a plausible-looking idea to find the beliefs it depends on.

**Don't:**
> The name comes from nautical practice — sounding the water to learn what's beneath the surface before committing to a course…

The nautical etymology is a great line for the README. It adds nothing to the model's behavior.

**Exception:** short operational metaphors that show up in the model's voice are fine. Long philosophical setups are not.

### G2. Every user-facing skill takes a clear stance

A `Your Stance` block near the top defines posture. The stance tells the model how to *show up* — and without one, skills regress to generic helpfulness.

**Test for stance strength:** given only the stance text, can you describe how this skill handles an ambiguous input *differently* from a sibling skill? If the stances of two sibling skills are substitutable, the distinction isn't real yet.

Examples from discovery: `orient` is receptive, `explore` is generative, `sound` is exploratory, `critique` is adversarial-but-constructive, `prioritize` is evaluative, `experiment` is rigorous-but-pragmatic. You can't swap them.

### G3. Operational rules over prescriptive checklists

The model does better with rules it can apply to novel cases than with numbered procedures it stops reading past step 4.

**Rule (good):** "Read the code before writing prose."
**Procedure (usually bad):** "Step 1: run `git log`. Step 2: grep for X. Step 3: read file Y."

**Numbered process is appropriate when:**
- The skill or agent runs autonomously for a long time
- Multiple distinct phases produce distinct artifacts
- The order is genuinely load-bearing (pre-work before dispatch; ground-before-propose; test-before-implement)

**Numbered process is harmful when:**
- It micromanages Claude's tool use
- It prescribes steps the model would take anyway
- It suppresses judgment for variations that matter

The `developer` agent's rules block is a model of this:

> - Tests pass, linters clean, ACs covered before submitting
> - Commits tell a clear story — one logical change each
> - When an AC is ambiguous and you made a judgment call, say so in the self-review
> - When you couldn't fully satisfy an AC, explain why — trust depends on truth-telling

Four load-bearing principles, applied across every decision the agent makes in an autonomous run. Compare to a 15-line numbered procedure that would say less.

### G4. Say what the skill is NOT for

"What X does NOT do" sections cost lines but earn them back by preventing sprawl. They are how a multi-skill routing system stays clean. Every user-facing skill needs one.

**Do:**
> ## What Sound Does NOT Do
>
> Sound does not judge the idea. It does not say "this is good" or "this is bad." It reveals the structure underneath so the PM can make that judgment themselves. That's the difference between sounding and critiquing.

### G5. Name the transitions

Skills explicitly name where to go when the conversation shifts. This is mixed-initiative in miniature: the AI proposes the next move, the user decides.

Transitions should be **specific**, not generic:

**Do:**
> - Assumptions need ranking → **prioritize** ("You have eight assumptions. Want to figure out which ones to test first?")

**Don't:**
> You may want to consider another skill next.

### G6. `model` and `effort` are deliberate and consistent within a plugin

A plugin that specifies `model: sonnet` on some skills and inherits on others is saying "these choices weren't considered." That's a smell. Either specify everywhere, or inherit everywhere, or have a stated rule for when to specify.

**Default recommendations (observed patterns in Anthropic's own skills):**

| Work | Model | Effort |
|------|-------|--------|
| Pure exploration (search, read-only listing) | Haiku or Sonnet | low–medium |
| Listening / reflecting / routing | Sonnet | medium |
| Drafting artifacts, grounded analysis | Sonnet | high |
| Multi-round critique, architectural reasoning | Opus | xhigh |
| Autonomous long-running implementation | Sonnet or Opus | high |

**Don't use Opus reflexively.** It costs more and runs slower. Reserve it for work where a weaker model will produce visibly worse output (multi-persona critique, architectural design).

### G7. `context: fork` only for autonomous, summarizing work

Fork when the agent will produce a lot of intermediate output (tool calls, search results, file reads) and return a consolidated summary. The main conversation stays clean; the user reviews the result.

**Don't fork for interactive skills.** The whole point is that the user and the model are in conversation in the shared context. Forking severs that.

Practical rule: agents almost always fork; skills almost never do.

### G8. Tool allowlists are narrow by default

Start from the minimum:

- Read-only skills: `Read, Glob, Grep`
- Artifact-writing skills: `Read, Write, Edit, Glob, Grep`
- Skills that genuinely invoke the shell: add `Bash` — and consider a one-line comment in the body explaining *why* if it isn't obvious
- Agents that dispatch sub-agents: add `Agent`
- Research agents: add `WebSearch, WebFetch`

**Broad allowlists are a smell.** If a skill ships with `Read, Write, Edit, Glob, Grep, Bash` but never runs Bash, that signals tool permissions weren't considered as part of the skill's identity. Tighten.

**The forbidden combination:** `Write, Edit` on an agent whose body explicitly says "you do not modify the graph." That's a contradiction the installer can't catch but a careful reviewer should.

### G9. The core skill is a table of contents

`_internal/core/SKILL.md` should be navigation — what references exist, what artifact skills are available, how composition works. The substantive content lives in `references/`. If the core skill grows past ~40 lines, extract more into references.

Loading three big references on every skill invocation is wasteful. The core skill should let the model pull the specific reference it needs — `concept.md` for the model, `schemas.md` for writing artifacts, `guidelines.md` for interaction posture.

### G10. Ground in the material before proposing

The single most important rule in Etak. Discovery grounds in the graph (read existing objectives, opportunities, ideas *before* brainstorming). Develop grounds in the codebase (grep for the subsystems, read the tests, check adjacent patterns *before* writing a spec).

This is reflection-in-action made concrete. It has to be **explicit**, not implied — if the skill can propose without reading, it will.

Examples of explicit grounding rules in-body:

- `spec/SKILL.md` has a dedicated "The Ground-in-Codebase Rule" section
- `brainstorm.md` opens with "Know what's already there. Before generating, read the parent artifact and its existing children."
- `feasibility.md` says "Read the codebase before writing the assessment. Without it, every dimension below is a guess."

### G11. Show artifacts before writing

Every artifact-writing skill shows proposed content before committing. Every `Write` / `Edit` is confirmed unless the user has explicitly authorized more autonomy.

This is how the mixed-initiative posture stays honest: **the AI drafts, the human commits.** Skills that write silently burn trust.

### G12. Agents prescribe more process than skills, but every step earns its line

An agent's body is longer than a skill's because the agent runs autonomously and has to hold itself to a process the human isn't watching. But process steps that don't change behavior — "then run git status" — are over-instruction. Replace them with principles the agent can apply.

**Rule:** if step N says "do X," ask: would the agent skip X without this line? If not, delete.

Watch especially for:
- Specific shell commands the model would already run (`tsc --noEmit`, `git fetch`, etc.)
- Steps that repeat what an adjacent step already said
- "Handle problems" sections that enumerate every conceivable edge case instead of stating a principle

### G13. Extract shared rules; keep differentiated stances local

If three sibling skills all say "push back on vague framing," hoist it into `core/references/guidelines.md`. If three sibling skills all have **different** stances, keep the stances in the sibling skills — don't flatten.

Shared *rules* go to core. Differentiated *voice* stays local.

**Also applies to references:** if a reference's opening sentence is a verbatim echo of its SKILL.md's opening sentence, the reference hasn't started yet. Rewrite the opening to describe what *this reference* adds.

### G14. Name the failure modes

Every skill ends with a "Failure Modes" or "Anti-Patterns" section. This teaches the model what to flag in the user's behavior and what to self-correct on its own. It's where "push back" behavior gets its specific shape.

**Do (from `plan`):**
> - **Planning without reading the codebase.** The inside-out lens is missing. Feasibility, effort, and sequencing all get disconnected from reality.
> - **Workstreams that are org chart, not system.** Coordination cost goes up, not down.
> - **M1 is big-bang.** "All the hard parts working" is a wish, not a milestone.

Each failure mode names a specific pattern *and* why it's bad. Generic warnings ("be careful not to over-engineer") don't teach.

### G15. Cross-plugin dispatch is explicit and graceful

When an agent depends on another plugin ("`tech-lead` dispatches to `etak-deliver`'s `quality-engineer`"), the dependency is stated and the fallback when that plugin isn't installed is stated too.

**The plugin must work usefully on its own and get more useful when companions are installed.** Every skill that expects another plugin should say what happens if it's absent:

> Until specialist agents are available, note in the plan where specialist review would be warranted and what risk is accepted by deferring.

## Authoring checklist

Before opening a PR, walk through this list for each skill or agent you're adding or modifying:

### Frontmatter
- [ ] `name` matches the directory
- [ ] `description` starts with what it does, framed as when to reach for it
- [ ] `when_to_use` has concrete trigger phrases
- [ ] `model` and `effort` are specified and defensible (see G6)
- [ ] `context: fork` present iff the work is autonomous + summarizing (see G7)
- [ ] `allowed-tools` is the narrowest set that lets the work happen (see G8)
- [ ] `user-invocable: false` on internal skills

### Body
- [ ] Opening paragraph is operational, not narrative (G1)
- [ ] `Your Stance` section is non-substitutable with sibling skills (G2)
- [ ] Process uses rules over numbered procedures, or the numbering is load-bearing (G3)
- [ ] "What X does NOT do" boundary section exists (G4)
- [ ] Transitions are specific handoffs with trigger behaviors (G5)
- [ ] Ground-in-material is explicit if the skill produces artifacts (G10)
- [ ] Artifact-writing skills show before writing (G11)
- [ ] Failure modes / anti-patterns section exists (G14)
- [ ] Cross-plugin dispatch (if any) states both dependency and fallback (G15)

### References
- [ ] Reference opening does not echo the SKILL.md opening verbatim (G13)
- [ ] Loading each reference is worth the tokens it costs (G1, G9)

### README
- [ ] `Relationship to other etak plugins` section names upstream / sibling / downstream plugins and what flows between them (G15)

## Common mistakes

### The narrative-heavy skill
The opening reads like a product page — why the feature exists, the problem it solves, history of the approach. Move it all to the README. The skill itself should start by telling the model what to do.

### The everything-goes allowlist
Every new skill ships with `Read, Write, Edit, Glob, Grep, Bash`. When asked, the author says "just in case." This is the smell G8 names. If the work doesn't need a tool, remove it.

### The checklist masquerading as a process
Ten numbered steps, each saying "now do the thing you'd do anyway." The model stops reading around step 4 and runs on general disposition. Collapse to the 3-4 steps that are genuinely load-bearing; convert the rest into principles in a `Guidelines` block.

### The phantom transition
"When this comes up, consider another skill." Which skill? For which trigger? A transition that doesn't name a target and a user cue isn't a transition — it's a gesture. Rewrite as "X → **skill-name** ('quoted trigger line')".

### The silent writer
A skill writes files without previewing. The user ends up with artifacts they didn't consent to. This breaks the mixed-initiative contract. Always confirm, always show.

### The weak stance
"You are a helpful assistant who thinks carefully." The model was going to do that anyway. A stance has to give the skill something to push *against* — an adversarial posture, a receptive posture, a narrow focus. "Helpful" is not a stance.

---

## Further reading

- [`docs/plugin-design-review.md`](../plugin-design-review.md) — the audit that produced these guidelines
- [`docs/context/foundations.md`](../context/foundations.md) — the research foundations Etak is built on
- [`docs/context/about-the-name.md`](../context/about-the-name.md) — why "Etak" and what it implies
- [`plugin-schema.json`](../../plugin-schema.json) — the manifest schema

When in doubt, read the existing plugins. `etak-discovery` and `etak-develop` are the reference implementations; any pattern you see there that *isn't* documented here is either an established convention or a bug worth flagging in a PR.
