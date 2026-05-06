# Spike

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create and update spikes — time-boxed investigations that reduce
uncertainty and unblock decisions.

## Schema

```yaml
---
name: "Evaluate OAuth libraries for multi-provider support"
type: spike
status: planned  # planned | in_progress | complete
time_box: "2 days"
question: "Which OAuth library best supports Google + GitHub + SAML with minimal custom code?"
decision_criteria: "Supports all 3 providers, <500 lines of glue code, active maintenance"
outcome: null  # finding | proof_of_concept | adr
result: null
---
```

Body: investigation plan, what to look at, how to evaluate. After completion,
add a Results section with what was found and the recommendation.

## What Makes a Good Spike

- **Question-driven** — a single, concrete question to answer.
- **Time-boxed** — the box is sacred. If you blow through it, you've learned
  something too: the problem is harder than you thought.
- **Decision criteria stated upfront** — how will you know the answer is good
  enough?
- **Outcome-typed** — at the end, you have a *finding*, a *proof of concept*, or
  an *ADR*. Not a bigger question.

## Moves

### Capture a new spike

- **What's the question?** Force it into one sentence.
- **Why does it matter?** What decision does this unblock?
- **What are you NOT answering?** Prevent scope creep.
- **How long?** Days, not weeks. If you need weeks, it's probably a project.
- **What does a good answer look like?** Decision criteria make the finding
  useful.

### Update a running spike

**Status lifecycle:** `planned → in_progress → complete`

- planned → in_progress: investigation started
- in_progress → complete: time-box hit or answer found

When closing the spike, fill in:
- **outcome**: `finding` | `proof_of_concept` | `adr`
- **result**: the actual answer, concise

If the outcome is `adr`, create the ADR artifact and link it. If `proof_of_concept`,
note where the code lives (branch, PR, or gist). If `finding`, write the finding
into the spike body so it doesn't evaporate.

### Time-box breached

Don't quietly extend. Either:
- **Accept partial answer** — write up what you know, ship a provisional finding
- **Consciously extend** — record why; note new deadline
- **Abandon** — you've learned the problem is different than expected; spike
  failed its premise. That's valuable. Record it.

### Write the artifact

Generate a kebab-case filename and write to the canonical path in the artifacts registry. Include
frontmatter with `name`, `type: spike`, `status: planned`, `time_box`, `question`,
`decision_criteria`, `outcome: null`, `result: null`. Body: investigation plan,
what to look at, how to evaluate. Always show before writing.

## Failure Modes

- Spike with no decision criteria — investigation without a terminal condition.
- Spike as procrastination — "we need to spike this" when the answer is knowable
  from the code or from experience.
- Spike past time-box without acknowledgment — silent scope growth.
- Spike that produces a bigger question rather than an answer — you didn't scope
  the question tightly enough. Capture the finding, scope the next spike.
