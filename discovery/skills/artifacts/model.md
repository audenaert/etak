# The Product Opportunity Space

The discovery system maintains a **persistent, evolving map** of your
product's opportunity space — what you know about customer needs, the
solutions you're considering, the assumptions behind them, and what
you've learned from testing those assumptions.

This map lives in `docs/discovery/` as markdown artifacts with typed
links between them. It is your team's institutional memory for product
discovery.

## Why This Exists

Product teams lose knowledge constantly. Insights from user interviews
fade. The reasoning behind decisions evaporates. Ideas get revisited
without remembering why they were shelved. Assumptions go untested
because no one remembers they were assumptions.

The opportunity space captures this knowledge in a structured,
navigable form that persists across conversations, sprints, and team
changes. Its value compounds over time:

- **Invalidated assumptions** save you from building the wrong thing
- **Validated assumptions** give you confidence to commit
- **The graph itself** becomes a map of what you know and don't know — making gaps visible

## The Structure

Five-node core hierarchy plus two peripheral types.

```
Objective          Why — the business outcome you're pursuing
  └─ Opportunity   Who/What — customer needs framed as "How might we..."
       └─ Idea     How — proposed solutions that address opportunities
            └─ Assumption   What must be true for the idea to work
                 └─ Experiment   How we test whether it's true
```

Peripheral types extend the graph without being part of the spine:

- **Critique** — a single round of structured examination targeting an idea or opportunity. Findings can graduate into assumptions.
- **Memo** — a sustained analytical document (framework, survey, literature review, structured argument) that informs the opportunity space without being a gap to fill or a solution to build. Optionally `supports` an opportunity or objective; may be foundational and link to nothing.

## How to Work With It

Not a waterfall. You can enter anywhere:

- **Top-down**: start with an objective, explore opportunities, brainstorm ideas
- **Bottom-up**: start with an idea, trace upward to find what opportunity it serves and what objective it ladders to
- **Middle-out**: start with a customer need you've observed, connect it up to objectives and down to ideas

The natural rhythm is: **discover** what's possible (navigate, brainstorm,
prioritize) → **define** it in the graph (create and refine artifacts) →
**validate** your thinking (surface assumptions, critique, design
experiments) → back to discover with what you've learned.

## File Naming

Use kebab-case derived from the node name:

- `increase-researcher-engagement.md`
- `hmw-help-researchers-discover-related-works.md` (opportunity filenames are prefixed with `hmw-`)

## Typed Links

Links reference other nodes by filename (without extension):

| From         | Link field          | To                                                    |
|--------------|---------------------|-------------------------------------------------------|
| Opportunity  | `supports`          | Objective(s)                                          |
| Idea         | `addresses`         | Opportunity(ies)                                      |
| Assumption   | `assumed_by`        | Idea(s)                                               |
| Experiment   | `tests`             | Assumption(s)                                         |
| Experiment   | `result_informs`    | Idea(s), Opportunity(ies)                             |
| Critique     | `about_idea`        | Idea (set exactly one of about_idea or about_opportunity) |
| Critique     | `about_opportunity` | Opportunity (set exactly one of about_idea or about_opportunity) |
| Memo         | `supports`          | Objective(s), Opportunity(ies) — optional             |

## Status Lifecycles

| Type        | Statuses                                                        |
|-------------|-----------------------------------------------------------------|
| Objective   | active, paused, achieved, abandoned                             |
| Opportunity | active, paused, resolved, abandoned                             |
| Idea        | draft, exploring, validated, ready_for_build, building, shipped |
| Assumption  | untested, validated, invalidated                                |
| Experiment  | planned, running, complete                                      |
| Critique    | planned, running, complete                                      |
| Memo        | draft, active, archived                                         |

## Traceability to Development

Discovery nodes do not point forward into development. When a validated
idea is scoped into implementation, the traceability link is maintained
from the **development side** via `from_discovery` on the project or
story. This keeps the audit trail owned by the engineering team —
whose changes must trace back to the change request that initiated
them — and often satisfies compliance requirements for production-system
traceability.
