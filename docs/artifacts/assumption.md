# Assumption

**Role:** testable belief that must be true for an idea to work. The hinge on which discovery turns from opinion to evidence.

## Why it exists

Every idea rests on beliefs. The team believes users want this. Believes the mechanism will work. Believes the outcome will move the metric. Most of those beliefs never get examined. Most of them are probably right. A few are wrong, and the wrong ones are why good-looking ideas ship and fail.

An assumption is a belief, made explicit, stated so precisely that you could design an experiment to test it. Two dimensions matter:

- **Importance.** If this belief is wrong, how badly does the idea fail?
- **Evidence.** How much do we actually know?

The riskiest assumptions — high importance, low evidence — are the ones worth testing first. Cheap tests that settle risky beliefs are the highest-leverage work in product discovery.

The system works this way on purpose. Without assumption tracking, teams build ideas and hope they work. With it, teams identify the beliefs that make the idea risky, test them cheaply, and either gain confidence or pivot before investing in implementation.

An invalidated assumption is not a failure. It is the system working. It means the team learned something important for the cost of an experiment rather than the cost of a build.

## When to create one

- **After capturing an idea.** Surfacing assumptions should follow idea capture almost always.
- **When critique identifies a belief that needs testing.** A critique finding often becomes a new assumption.
- **When you realize you're making a bet without evidence.** The moment of realization is the moment to capture.
- **When an assumption is shared across multiple ideas.** High-leverage — one test informs several.

## Schema

```yaml
---
name: "Researchers will engage with AI-generated related works suggestions"
type: assumption
status: untested  # untested | validated | invalidated
importance: high  # high | medium | low
evidence: low     # high | medium | low
assumed_by:
  - ai-powered-related-works-suggestions
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Clear, testable belief stated as a specific claim. |
| `type` | yes | Always `assumption`. |
| `status` | yes | Current lifecycle state. |
| `importance` | yes | Impact if wrong: high, medium, low. |
| `evidence` | yes | Current evidence level: high, medium, low. |
| `assumed_by` | yes | Slugs of ideas this assumption underlies. |

## Status lifecycle

```
untested  ─────▶  validated     (experiment confirms)
untested  ─────▶  invalidated   (experiment disproves)
```

- **untested** — not yet tested by an experiment.
- **validated** — experiment results support the belief.
- **invalidated** — experiment results contradict the belief.

Note what's missing: there is no "partially validated" status. Results either meet the pre-committed success criteria or they don't. If they don't clearly meet them, the experiment result is inconclusive and a follow-up is needed. Assumption status stays `untested` until something settles it.

## Importance and evidence: calibration

**Importance.** If wrong, how bad?

- **High** — the idea cannot be saved.
- **Medium** — the idea still works but impact is significantly smaller.
- **Low** — the idea works; we adjust and move on.

**Evidence.** How much do we actually know?

- **High** — multiple independent sources, including recent behavioral data.
- **Medium** — one good study, a clear analog, strong indirect evidence.
- **Low** — hunches, one or two conversations, analyst intuition. No behavioral data.

The seductive lie is calling things medium. Most beliefs are low until someone checks. Be honest.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `assumed_by` | Idea(s) |
| Incoming | Experiments link via `tests` | this assumption |

An assumption can be held by multiple ideas. Shared assumptions are especially valuable to test — one experiment de-risks everything that rests on them.

## Example

```markdown
---
name: "Researchers will engage with AI-generated related works suggestions"
type: assumption
status: untested
importance: high
evidence: low
assumed_by:
  - ai-powered-related-works-suggestions
---

## Description

We believe researchers viewing a work will click on and read AI-suggested related
works at a meaningful rate (>15% click-through). Without this engagement, the feature
delivers no value regardless of suggestion quality.

## Current Evidence

- **For:** researchers in interviews expressed interest in discovering new works
- **Against:** the same researchers said they distrust AI recommendations
- **Unknown:** whether in-context suggestions (while reading) perform differently
  from standalone recommendation feeds

## If Wrong

The related works sidebar becomes noise users learn to ignore. We would need to
reconsider the delivery mechanism — perhaps curated collections or social signals
rather than AI suggestions.
```

## Useful actions

### Create

Typically through [sound](../skills/sound.md), which surfaces assumptions for an idea. Can also be created directly: "I want to add an assumption that X."

### Rate

Set importance and evidence honestly. The ratings drive what gets tested first.

### Prioritize

```
Prioritize assumptions.
```

[Prioritize](../skills/prioritize.md) ranks assumptions by risk (importance × inverse evidence). The top of the list is what to test first.

### Design an experiment

```
Design an experiment for this assumption.
```

[Experiment](../skills/experiment.md) produces a test. The pre-committed action plans say what happens when results come in.

### Update with new evidence

When an experiment completes, [experiment](../skills/experiment.md) propagates the result: assumption status advances to validated or invalidated, evidence level updates. Parent ideas and sibling ideas with shared assumptions get flagged for review.

### Promote or demote

If a conversation reveals that an assumption rated medium is actually critical — or that one rated high is less load-bearing than thought — update the rating. The graph reflects the current state of your understanding.

## Tips

- **Testable.** Every assumption should be stated so you could design an experiment. If you can't imagine one, the assumption is too vague.
- **High importance + low evidence = test first.** The core prioritization heuristic.
- **Shared assumptions are high-leverage.** One assumption underlying three ideas means one experiment de-risks all three.
- **Invalidation is valuable.** It saves you from building the wrong thing. Celebrate it.
- **Be honest about evidence.** "We think users want this" with no research is low evidence. Not medium.
- **Keep *If Wrong* concrete.** Spell out what happens to the parent idea if the belief fails. This is what importance is really rating.

## Related

- [Idea](idea.md) — the solution resting on this assumption.
- [Experiment](experiment.md) — the method used to test it.
- [Sound skill](../skills/sound.md) — surface assumptions from an idea.
- [Prioritize skill](../skills/prioritize.md) — rank by risk.
- [Experiment skill](../skills/experiment.md) — design tests.
- [Artifacts overview](README.md)
