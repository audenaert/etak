# Discovery Node Schemas

All discovery artifacts live in `docs/discovery/` within the current project, organized by level:

```
docs/discovery/
  objectives/
  opportunities/
  ideas/
  assumptions/
  experiments/
  critiques/
```

## File Naming

Use kebab-case derived from the node name:
- `increase-researcher-engagement.md`
- `hmw-help-researchers-discover-related-works.md`

## Typed Links

Links reference other nodes by filename (without extension):

| From         | Link field      | To                |
|--------------|-----------------|-------------------|
| Opportunity  | `supports`      | Objective(s)      |
| Idea         | `addresses`     | Opportunity(ies)  |
| Assumption   | `assumed_by`    | Idea(s)           |
| Experiment   | `tests`         | Assumption(s)     |
| Experiment   | `result_informs`| Idea(s), Opportunity(ies) |
| Critique     | `target`        | Any node          |

Discovery nodes do not point forward into development. When a validated idea is
scoped into implementation, the traceability link is maintained from the
**development side** via `from_discovery` on the initiative, project, or story.
This keeps the audit trail owned by the party responsible for it — the engineering
team whose changes must trace back to the change request that initiated them —
and often satisfies compliance requirements for production-system traceability.

## Objective

```yaml
---
name: "Human-readable name"
type: objective
status: active  # active | paused | achieved | abandoned
---
```

Body: Description, context, success criteria, how we'd measure it.

## Opportunity

Opportunities are framed as "How might we..." (HMW) statements — open-ended questions that invite solution thinking without embedding a specific approach.

```yaml
---
name: "HMW help researchers discover works outside their existing knowledge?"
type: opportunity
status: active  # active | paused | resolved | abandoned
supports:
  - objective-filename-without-ext
---
```

Body: Description, evidence, who experiences this, how we learned about it.

## Idea

```yaml
---
name: "Human-readable name"
type: idea
status: draft  # draft | exploring | validated | ready_for_build | building | shipped
addresses:
  - opportunity-filename-without-ext
---
```

Body: Description, how it works, why we think it would help.

## Assumption

```yaml
---
name: "Human-readable name"
type: assumption
status: untested  # untested | validated | invalidated
importance: high  # high | medium | low — if wrong, does the idea fail?
evidence: low     # high | medium | low — how much do we actually know?
assumed_by:
  - idea-filename-without-ext
---
```

Body: Description, why this matters, what evidence exists (or doesn't).

## Experiment

```yaml
---
name: "Human-readable name"
type: experiment
status: planned  # planned | running | complete
tests:
  - assumption-filename-without-ext
method: user_interview | prototype_test | fake_door | concierge_mvp | data_analysis | ab_test | survey
success_criteria: "Specific, measurable criteria"
duration: "Estimated duration"
effort: low  # low | medium | high
result: null  # validated | invalidated | inconclusive
learnings: ""
interpretation_guide: |
  How to read results against success criteria.
  Edge cases and ambiguities to watch for.
action_plan:
  if_validated: "What we'll do next"
  if_invalidated: "What we'll reconsider or pivot to"
  if_inconclusive: "Follow-up experiment or additional data needed"
---
```

Body: Detailed experiment plan, participant criteria, protocol, materials needed.

## Critique

Critique artifacts capture the back-and-forth of an adversarial stress-test session so it can be referenced later.

```yaml
---
name: "Critique of AI-powered related works suggestions"
type: critique
target: idea-ai-powered-related-works-suggestions
personas_used:
  - "Tenured professor resistant to AI"
  - "PhD student on a deadline"
  - "Resource-constrained builder"
frameworks_used:
  - "Pre-mortem"
date: "2026-03-31"
artifacts_created:
  - assumption-researchers-trust-ai-suggestions
---
```

Body: Full critique session — rounds with persona/framework label, findings by severity (fatal/serious/minor), themes covered, strengths confirmed, synthesis and recommendations.

## Status Values Summary

| Type        | Statuses                                                    |
|-------------|-------------------------------------------------------------|
| Objective   | active, paused, achieved, abandoned                         |
| Opportunity | active, paused, resolved, abandoned                         |
| Idea        | draft, exploring, validated, ready_for_build, building, shipped |
| Assumption  | untested, validated, invalidated                            |
| Experiment  | planned, running, complete                                  |
| Critique    | (no status — critiques are point-in-time records)           |
