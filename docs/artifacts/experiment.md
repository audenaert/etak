# Experiment

**Role:** structured test of one or more assumptions. The mechanism that turns discovery from opinion-driven to evidence-driven.

## Why it exists

Most teams "run experiments." Most of what they call experiments are feature releases with a post-hoc metric check. An experiment in Etak's sense has five parts, and without all five it is something else.

1. A clear assumption being tested.
2. An appropriate method.
3. Specific success criteria defined in advance.
4. An interpretation guide for edge cases.
5. Pre-committed actions for each possible outcome.

The discipline is not academic. It is the difference between learning something and performing confidence. Without pre-committed criteria, the results meeting becomes a negotiation. Without pre-committed actions, the results produce nothing but a presentation. The structure makes the learning stick.

Experiments are also how you compound. Every experiment result, whether validated, invalidated, or inconclusive, updates the graph. The next decision has better evidence than the last one. That compounding is the whole game.

## When to create one

- **A high-importance, low-evidence assumption surfaces.** The canonical trigger.
- **Before significant investment.** If you're about to scope a multi-sprint build, test the riskiest assumption first. The cost of the experiment is always less than the cost of the build.
- **A stakeholder disagreement is blocked on evidence.** When two senior people disagree about whether users want X, an experiment is faster than a debate.
- **An assumption is shared across ideas.** One experiment can de-risk several lines of thinking. High leverage.

## Schema

```yaml
---
name: "Prototype test: related works panel"
type: experiment
status: planned  # planned | running | complete
tests:
  - researchers-will-engage-with-ai-suggestions
method: prototype_test
success_criteria: "At least 3 of 5 participants click on a suggested work within 30 seconds"
duration: "1 week"
effort: low  # low | medium | high
result: null  # validated | invalidated | inconclusive
learnings: ""
interpretation_guide: |
  3/5 = validated. 2/5 = inconclusive. 1/5 or 0/5 = invalidated.
  Watch for clicks out of politeness rather than genuine interest.
action_plan:
  if_validated: "Advance idea to ready_for_build"
  if_invalidated: "Explore alternative delivery mechanisms"
  if_inconclusive: "Run fake-door test at larger scale"
result_informs: []
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Descriptive title. |
| `type` | yes | Always `experiment`. |
| `status` | yes | Current lifecycle state. |
| `tests` | yes | Slugs of assumptions being tested. |
| `method` | yes | Experiment method (see below). |
| `success_criteria` | yes | Specific, measurable criteria defined in advance. |
| `duration` | yes | Estimated time to run. |
| `effort` | yes | Resource investment: low, medium, high. |
| `result` | no | Outcome after completion. |
| `learnings` | no | Key findings and unexpected observations. |
| `interpretation_guide` | no | How to read results; edge cases to watch. |
| `action_plan` | yes | Pre-committed actions for each outcome. |
| `result_informs` | no | Slugs of ideas or opportunities changed by results. |

## Method types

| Method | Best for | Evidence quality |
|--------|----------|------------------|
| `user_interview` | Desirability, qualitative texture | Moderate |
| `prototype_test` | Usability, specific interactions | High |
| `fake_door` | Desirability at scale | High |
| `concierge_mvp` | Viability, end-to-end feasibility | High |
| `data_analysis` | Feasibility, some viability | Varies |
| `ab_test` | Causal assumptions with traffic | High |
| `survey` | Desirability at scale (limitations apply) | Moderate |

## Status lifecycle

```
planned  ─────▶  running  ─────▶  complete
```

- **planned** — designed but not started.
- **running** — actively collecting data.
- **complete** — results recorded and propagated.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `tests` | Assumption(s) |
| Outgoing | `result_informs` | Ideas, opportunities |

When an experiment completes, its results propagate: tested assumptions update to validated or invalidated; parent ideas may advance or need rethinking.

## Example

```markdown
---
name: "Prototype test: related works panel"
type: experiment
status: complete
tests:
  - researchers-will-engage-with-ai-suggestions
method: prototype_test
success_criteria: "At least 3 of 5 participants click on a suggested work within 30 seconds"
duration: "1 week"
effort: medium
result: validated
learnings: |
  4 of 5 participants clicked within the window. Participants valued the citation
  context shown beside each suggestion. One participant wanted to know WHY a given
  work had been suggested — opens a follow-up opportunity around suggestion rationale.
interpretation_guide: |
  3/5 = validated. 2/5 = inconclusive. 1/5 or 0/5 = invalidated.
  Watch for participants clicking out of politeness rather than genuine interest.
action_plan:
  if_validated: "Advance idea to ready_for_build, scope for next sprint"
  if_invalidated: "Explore alternative delivery (curated lists, social signals)"
  if_inconclusive: "Run larger fake-door test with 50+ users"
---

## Experiment Plan

### Participants
5 active researchers using the platform at least weekly.

### Protocol
1. Show participants their current reading view.
2. Reveal the related works sidebar with 5 AI-generated suggestions.
3. Observe behavior for 2 minutes without prompting.
4. Ask follow-up questions about relevance and trust.

## Results

4 of 5 participants clicked on at least one suggestion within 30 seconds. The
participant who didn't said they wanted to finish reading first but would check
later. All participants said citation context made suggestions feel trustworthy.
```

## Useful actions

### Design

```
Design an experiment for this assumption.
```

[Experiment skill](../skills/experiment.md) walks through the five parts. Refuses vague success criteria. Drafts action plans for you to confirm.

### Start

When you begin running, update status to `running`. Etak will notice if it runs significantly past its estimated duration.

### Record results

```
Record results for [this experiment].
```

Enter what happened. The tool interprets against pre-committed criteria and pushes back on post-hoc reinterpretation.

### Propagate

```
Propagate the results.
```

The most important action. Updates the tested assumptions, traces impact on parent ideas, checks for shared assumptions across other ideas, and recommends next moves. Stale experiments (complete but not propagated) show up as the first priority in [orient](../skills/orient.md).

### Review the interpretation guide

If results come in ambiguous, re-read the guide. If the guide doesn't cover the case, update it and note the update. The next experiment will benefit.

## Tips

- **Pre-commit on actions.** Decide what you'll do with each outcome before results arrive. This prevents post-hoc rationalization.
- **Smallest viable experiment.** Enough evidence to make a decision, not perfect certainty.
- **Record results promptly.** The most common failure mode is running experiments and never closing the loop.
- **Propagate outcomes.** Update assumptions, review parent ideas, check for shared beliefs. This is what makes the graph a living system.
- **Inconclusive is valid.** But it should lead to a follow-up action, not a shrug.
- **Learnings beyond the headline matter.** Capture unexpected observations — they often become the next opportunity.

## Related

- [Assumption](assumption.md) — the belief the experiment tests.
- [Idea](idea.md) — may advance or pivot based on results.
- [Experiment skill](../skills/experiment.md) — design, run, record, propagate.
- [Designing experiments tutorial](../tutorials/designing-experiments.md) — runnable walkthrough.
- [Artifacts overview](README.md)
