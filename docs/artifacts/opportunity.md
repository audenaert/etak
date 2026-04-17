# Opportunity

**Role:** customer need, pain point, or desire framed as a *how might we* question. Sits between objective and idea.

## Why it exists

Opportunities describe what customers struggle with, without specifying a solution. The *how might we* framing is deliberate: it keeps the opportunity generative. One opportunity should invite multiple possible solutions.

> "HMW help researchers discover works across disciplines?"

That single framing could lead to AI suggestions, curated collections, visualization tools, reading lists, social signals, or something nobody has thought of yet. The moment you say "add a sidebar with recommendations," you've crossed into idea territory and collapsed the space of options.

Opportunities are the reason Etak is not just a backlog. A backlog is a list of solutions. An opportunity space is a map of problems worth solving, each of which could be addressed many ways. Keeping those separate is the difference between PM and project management.

## When to create one

- When you identify a specific customer need, struggle, or desire.
- When decomposing an objective into addressable problem areas.
- When someone proposes a feature — capture the underlying need as an opportunity, and the proposed feature as an idea under it.
- When brainstorming surfaces a distinct problem worth tracking.

## Schema

```yaml
---
name: "HMW help researchers discover works outside their existing knowledge?"
type: opportunity
status: active  # active | paused | resolved | abandoned
supports:
  - increase-researcher-engagement
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | *How might we* question capturing the customer need. |
| `type` | yes | Always `opportunity`. |
| `status` | yes | Current lifecycle state. |
| `supports` | yes | Slugs of objectives this opportunity serves. |

## Status lifecycle

```
active  ─────▶  paused  ─────▶  active    (resume)
active  ─────▶  resolved                   (need addressed by shipped ideas)
active  ─────▶  abandoned                  (not worth pursuing)
paused  ─────▶  abandoned
```

- **active** — being explored. Ideas are being generated and tested.
- **paused** — temporarily deprioritized.
- **resolved** — the customer need has been addressed by a shipped idea.
- **abandoned** — no longer relevant or worth pursuing.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `supports` | Objective(s) |
| Incoming | Ideas link via `addresses` | this opportunity |

Opportunities can decompose into sub-opportunities. A broad HMW ("*how might we* help researchers discover more of the corpus?") may split into narrower HMWs (about finding related works, seeing coverage gaps, tracing influence patterns). Sub-opportunities link to their parent via `supports`, just like connections to objectives.

## Example

```markdown
---
name: "HMW help researchers discover works outside their existing knowledge?"
type: opportunity
status: active
supports:
  - increase-researcher-engagement
---

## Description

Researchers tend to start navigation from works and authors they already know. The
current interface supports this well but doesn't help them find works beyond their
existing mental map.

## Evidence

- User interviews (3 researchers): all reported staying within familiar territory
- Usage data: 80% of navigation follows citation links from known starting points
- Strength of evidence: moderate

## Current Workarounds

Researchers ask colleagues for recommendations, browse conference proceedings manually,
or maintain personal reading lists in spreadsheets.

## Impact

High for power users who would engage more deeply if they could see the broader landscape.
Moderate for casual users who may not know what they're missing.
```

## Useful actions

### Create

Through [explore](../skills/explore.md) or by describing the need directly. "I want to capture an opportunity about researchers missing relevant papers."

### Decompose

Ask [explore](../skills/explore.md) to split a broad opportunity into narrower sub-opportunities. Useful when you realize an opportunity contains multiple distinct problems masquerading as one.

### Brainstorm ideas under it

```
Brainstorm ideas for [this opportunity].
```

[Explore](../skills/explore.md) generates candidates. Saved ideas link automatically via `addresses`.

### Critique it

```
Critique this opportunity.
```

[Critique](../skills/critique.md) examines whether the opportunity is framed well, whether the evidence holds up, whether there are personas being excluded.

### Update status

When an opportunity is addressed by a shipped idea, mark it `resolved`. When it becomes irrelevant, mark it `abandoned`. Ideas under a resolved or abandoned opportunity get flagged in the next [orient](../skills/orient.md) session.

## Tips

- **Always frame as *how might we*.** If a team member gives a flat statement, convert it. The framing is doing work.
- **Solution-agnostic.** *HMW add a sidebar with recommendations* is an idea in disguise.
- **Right breadth.** *HMW improve the platform* is too broad. *HMW add a button* is too narrow. Aim for 2–5 plausible solution approaches.
- **Decompose when broad.** A single opportunity with ten ideas under it probably needs splitting.
- **Cite evidence, even thin evidence.** One user interview, a support ticket pattern, a hunch from a sales conversation. Some evidence beats none.
- **Every opportunity should link to at least one objective.** An orphan opportunity is a signal to either add the objective or reconsider whether the opportunity is worth tracking.

## Related

- [Objective](objective.md) — the outcome this opportunity serves.
- [Idea](idea.md) — proposed solutions that address opportunities.
- [Explore skill](../skills/explore.md) — generate opportunities and ideas.
- [Critique skill](../skills/critique.md) — stress-test an opportunity's framing.
- [Artifacts overview](README.md)
