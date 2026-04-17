# Objective

**Role:** business outcome the team is responsible for. The top of the graph. Everything else ultimately traces back to an objective.

## Why it exists

An objective is what you promised your stakeholders. Not "we'll build X," but "we'll move Y." Objectives describe results, not activities. They are durable strategic goals: *increase researcher engagement* rather than *build engagement features*.

Without clear objectives, discovery drifts toward interesting-but-ungrounded exploration. Every opportunity, idea, assumption, and experiment should ultimately answer the question "which outcome does this serve?" If you can't answer that question, the work is either premature or off-strategy.

Objectives are also how you keep a team from the feature factory. A team measured on features shipped will ship features. A team measured on objective movement will sometimes ship fewer features — because some features turn out not to move the objective, and the discovery work revealed that before the build.

## When to create one

- At the start of a product initiative, to articulate what success looks like.
- When you realize work is happening without a clear strategic goal — a common diagnostic is "why are we doing this?" producing different answers from different team members.
- When multiple opportunities share a common business outcome and you want to link them.

## Schema

```yaml
---
name: "Help researchers discover relevant works beyond their existing knowledge"
type: objective
status: active  # active | paused | achieved | abandoned
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Outcome-oriented title. A phrase or short sentence, not a question. |
| `type` | yes | Always `objective`. |
| `status` | yes | Current lifecycle state. |

The body of the document holds description, success criteria, and motivation. See the example below.

## Status lifecycle

```
active  ─────▶  paused  ─────▶  active    (resume)
active  ─────▶  achieved                   (success)
active  ─────▶  abandoned                  (no longer relevant)
paused  ─────▶  abandoned
```

- **active** — currently pursued. Guides discovery decisions.
- **paused** — temporarily deprioritized. May resume.
- **achieved** — success criteria met.
- **abandoned** — superseded or no longer relevant.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Incoming | Opportunities link via `supports` | this objective |

Objectives sit at the top of the graph. They don't link upward to anything inside the graph. They may link externally to OKRs, strategy docs, or leadership commitments, but those connections live in the body rather than the schema.

## Example

```markdown
---
name: "Help researchers discover relevant works beyond their existing knowledge"
type: objective
status: active
---

## Description

Researchers currently navigate the corpus through known entry points — works and
authors they already know. We want to surface connections and works they wouldn't
find on their own, expanding the scope of their research without overwhelming them.

## Success Criteria

- Researchers report discovering works they hadn't previously encountered
- Usage of discovery-oriented features increases quarter over quarter
- Researchers follow suggested connections and engage with the resulting content

## Motivation

The corpus contains ~2,000 works with rich interconnections. Most researchers only
engage with a small slice. The value of the platform increases disproportionately
if we help them see the broader landscape.
```

## Useful actions

### Create

Through conversation. "I want to add a new objective about improving trial conversion." The [explore](../skills/explore.md) or [orient](../skills/orient.md) skills will handle it.

### Review

```
Show me all active objectives.
```

Lists everything that is currently on your team's plate. Useful for calibration and for leadership conversations.

### Decompose

Ask [explore](../skills/explore.md) to brainstorm opportunities under an objective. This is the natural second move after defining the objective.

### Update status

When an objective is achieved, paused, or abandoned, update the status. The graph then reflects the shift. Downstream artifacts (opportunities, ideas) tied to a paused or abandoned objective get flagged in the next [orient](../skills/orient.md) session so you can decide what to do with them.

### Split

If an objective contains "and" ("increase engagement *and* reduce churn"), consider splitting it. Two objectives are easier to prioritize against than one compound one. Etak will notice and prompt.

## Tips

- **Outcome, not activity.** *Reduce onboarding time* not *build onboarding wizard.*
- **Measurable or observable.** You should be able to tell whether you're making progress.
- **Not too broad.** *Grow the platform* is too vague. What dimension? For whom?
- **Not too narrow.** If it sounds like a feature, it's probably an idea, not an objective.
- **Illustrate with candidate opportunities.** If you can sketch two or three *how might we* questions under the objective, it's scoped correctly. If you can sketch twenty, it's too broad. If you can sketch none, it's too narrow.

## Related

- [Opportunity](opportunity.md) — decomposes objectives into customer needs.
- [Orient skill](../skills/orient.md) — presents objectives as part of the graph readout.
- [Explore skill](../skills/explore.md) — generate opportunities under an objective.
- [Artifacts overview](README.md)
