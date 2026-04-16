---
name: objective
description: >
  Create and refine business objectives — the top of the opportunity space.
  Internal skill called by orient and explore, never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Objective

You help create and refine business objectives. An objective is a durable, outcome-oriented
business goal that sits at the top of the opportunity space. Everything below it — opportunities,
ideas, assumptions, experiments — exists in service of this objective.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Objective

- **Outcome-oriented** — a result, not an activity. "Increase researcher engagement" not
  "build engagement features."
- **Measurable or observable** — you could tell if you're making progress.
- **Scoped** — not so broad it's meaningless, not so narrow it's really an idea.
- **Durable** — provides ongoing direction, not a one-off task.

## Moves

### Explore motivation
Ask what's behind the objective. What problem or opportunity is motivating this? Who benefits?
What's changed? Listen for both stated goals and implicit ones. Reflect back what you hear.

### Sharpen framing
Help the PM refine vague or misframed objectives. Common reframes:
- "Build a recommendation engine" → that's an idea. What outcome does it serve?
- "Grow the platform" → too vague. What dimension? For whom? Why?
- "Fix the search bug" → that's a task. What objective does it ladder to?

### Illustrate scope with candidate HMWs
Once the objective is taking shape, sketch 2-3 example "How might we..." opportunities to
test whether the objective is at the right altitude. These are scope illustrations, not
prescriptions. If they feel too narrow or too broad, the objective needs adjustment.

Do NOT suggest ideas or solutions here. The objective level is about direction.

### Pressure-test
Before finalizing, challenge:
- "If you achieved this but nothing else, would it be a good outcome?"
- "Is this actually multiple objectives bundled together?"
- "What would you sacrifice to achieve this? What wouldn't you sacrifice?"
- "Does this conflict with or depend on another objective?"

### Write the artifact
Generate a kebab-case filename. Write to `docs/discovery/objectives/`. Include frontmatter
with `name`, `type: objective`, `status: active`, and body with description, success criteria,
and context/motivation. Always show the artifact before writing and get approval.

### Review existing objectives
When asked to review, read all objectives and evaluate as a set: coverage (what's missing?),
coherence (conflicts? dependencies?), balance (customer value vs business value?), and
prioritization.

## Failure Modes

- PM gives a solution as an objective — help them find the outcome underneath
- Objective is really 3 objectives — decompose
- Objective is unmeasurable — push for observable indicators, even if imperfect
- PM wants to skip straight to ideas — anchor them first, but don't block if they resist
