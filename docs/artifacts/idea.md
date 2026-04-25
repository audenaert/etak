# Idea

**Role:** proposed solution that addresses one or more opportunities. The primary unit of product output, but always in dialogue with the problem above it.

## Why it exists

An idea is your answer to the *how might we* question an opportunity poses. It describes what you would build or do, and why you believe it would help. Concrete enough to evaluate. Not so detailed it becomes a spec.

Ideas are the primary output of discovery. They start rough, get examined, get tested, get validated or invalidated, and either move into development or get shelved. The idea's journey through those states is the journey discovery exists to support.

Ideas matter as much for what they prevent as for what they produce. An idea captured in the graph, with assumptions surfaced and tested, prevents a specific failure mode: the team building something that sounded plausible in a meeting and turned out to rest on a belief nobody checked. Every invalidated assumption on an idea is two weeks of saved engineering effort.

## When to create one

- When brainstorming solutions for an opportunity.
- When someone proposes a feature — capture it, then connect it.
- When bottom-up thinking produces a solution before the problem is articulated. Capture the idea first, then trace upward to the opportunity it addresses. This is a valid entry path.

## Schema

```yaml
---
name: "AI-powered related works suggestions"
type: idea
status: draft  # draft | exploring | validated | ready_for_build | building | shipped
addresses:
  - hmw-help-researchers-discover-works
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Descriptive title of the proposed solution. |
| `type` | yes | Always `idea`. |
| `status` | yes | Current lifecycle state. |
| `addresses` | yes | Slugs of opportunities this idea serves. |

Ideas do not carry forward links into development. When the idea moves into implementation, the development-side artifact (project or story in etak-develop) records a `from_discovery` link pointing back to this idea. Traceability is the engineering team's responsibility — this matches how compliance audit trails work in practice, where changes to production systems must trace back to the change request that initiated them.

## Status lifecycle

```
draft  ──▶  exploring  ──▶  validated  ──▶  ready_for_build  ──▶  building  ──▶  shipped
```

- **draft** — captured but not yet developed. Ideas are cheap; capture them freely.
- **exploring** — worth investigating. Assumptions are being surfaced and tested.
- **validated** — key assumptions tested. Evidence supports the approach.
- **ready_for_build** — enough detail to scope for development. Handoff-ready.
- **building** — implementation has started.
- **shipped** — live and in use.

Statuses advance when earned, not when decided. An idea is not `validated` because the PM said so; it's validated because the assumptions under it have been tested and the results support proceeding.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `addresses` | Opportunity(ies) |
| Incoming | Assumptions link via `assumed_by` | this idea |

An idea can address multiple opportunities. A feature to help researchers bookmark papers might address both "help researchers return to interesting work" and "help researchers organize their reading." The graph carries both connections.

## Example

```markdown
---
name: "AI-powered related works suggestions"
type: idea
status: exploring
addresses:
  - hmw-help-researchers-discover-works
---

## Description

When a researcher is viewing a work, show a sidebar panel of related works they
haven't visited, using embedding similarity and citation graph proximity.

## Why This Could Work

The corpus already has rich metadata and citation relationships. Embedding similarity
can surface thematic connections that pure citation links miss. In-context suggestions
(shown while reading) likely perform better than standalone recommendation feeds
because they land when intent is highest.

## Open Questions

- How do we handle cold start — works with thin metadata?
- Should suggestions be personalized or universal?
- What's the right number of suggestions to show without overwhelming?
```

## Useful actions

### Create

Top-down: from [explore](../skills/explore.md) within the context of an opportunity. Bottom-up: "I have an idea for X" and Etak will capture the idea and help trace upward.

### Surface assumptions

```
What are we assuming about this idea?
```

[Sound](../skills/sound.md) produces the beliefs the idea rests on. This is typically the next move after capturing an idea.

### Critique

```
Critique this idea.
```

[Critique](../skills/critique.md) stress-tests the idea from external perspectives. Complementary to sound — sound looks inward, critique looks from outside.

### Advance status

Status moves with evidence. When all critical assumptions are validated, the idea advances to `validated`. When the idea has enough detail to hand to engineering, it advances to `ready_for_build`. These transitions are usually prompted by [orient](../skills/orient.md) or [prioritize](../skills/prioritize.md).

### Compare against siblings

If an opportunity has multiple ideas under it, [prioritize](../skills/prioritize.md) can rank them by value, cost, and confidence. This is the move that precedes a roadmap decision.

### Decompose

Large ideas should be split into smaller, independently-valuable ideas. "An AI assistant for the whole product" is not an idea — it's a program. Split until each idea answers a single question an opportunity poses.

## Tips

- **Ideas are cheap.** Capture them even if half-baked. `draft` exists for a reason.
- **Bottom-up entry is valid.** If a colleague has an idea before the opportunity is defined, capture the idea and trace upward. Often the tracing reveals the real opportunity.
- **Connect to opportunities.** Every idea should address at least one opportunity. If no opportunity exists, create a lightweight one first.
- **Don't over-specify.** The idea level answers "what and why," not "how." Technical details belong in specs or design docs in etak-develop, which trace back to this idea via `from_discovery` — not the other way around.
- **Surface assumptions early.** An idea with no assumptions surfaced is an untested bet. Every idea should have at least three or four assumptions before it leaves `draft`.

## Related

- [Opportunity](opportunity.md) — the customer need the idea addresses.
- [Assumption](assumption.md) — testable beliefs the idea rests on.
- [Experiment](experiment.md) — tests for those assumptions.
- [Sound skill](../skills/sound.md) — surface what the idea is betting on.
- [Critique skill](../skills/critique.md) — stress-test the idea.
- [Artifacts overview](README.md)
