---
name: initiative
description: >
  Create, update, and refine initiatives — multi-project strategic containers.
  Internal skill called by plan and survey; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Initiative

You help create, update, and refine initiatives — multi-project strategic containers
that give a set of related projects a shared "why."

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Initiative

- **Strategic, not tactical** — an initiative is worth multiple quarters of work across
  multiple teams. If it fits inside one project, it's a project.
- **Outcome-framed** — describes the change in the business or the product, not the
  features that will produce it.
- **Connected** — ideally traces upward to a discovery objective via `from_discovery`,
  or downward to at least one project.
- **Honest about scope** — includes constraints, non-goals, and success criteria.

## Moves

### Capture a new initiative

Engage with the strategic context first, structure second.

- **What changes if this succeeds?** Outcome language, not output language.
- **Why now?** External pressure, internal maturity, strategic bet.
- **Who is served?** The customer or the business — be specific.
- **Constraints?** Timeline, team, budget, dependencies.
- **Non-goals?** What this initiative explicitly doesn't include.

If a discovery idea initiated this, confirm the `from_discovery` link.

### Update an existing initiative

**Status lifecycle:** `proposed → active ⇄ paused → complete | abandoned`

- proposed → active: first project is planned and resourced
- active → paused: explicit decision to pause; note why
- active → complete: success criteria met
- any → abandoned: explicit decision to stop; note why (important for learning)

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/initiatives/`. Include
frontmatter with `name`, `type: initiative`, `status: proposed`, `parent: null`,
`children: []`, `from_discovery` if applicable. Body: strategic context, success
criteria, timeline constraints, non-goals. Always show before writing.

## Failure Modes

- Initiative is really a project (fits in one team, one quarter) — reframe as a
  project under an existing or new initiative.
- Initiative is a wish ("transform the platform") without success criteria — push
  for measurable outcomes.
- Initiative with no children after a while — it's a floating goal with no work; flag.
- Abandoned without explanation — always capture why. Future-you will ask.
