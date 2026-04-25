---
name: project
description: >
  Create, update, and refine projects — bounded deliverables that coordinate the work
  of one or more workstreams toward a set of milestones. Internal skill called by
  plan and survey; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Project

You help create, update, and refine projects — the primary coordination unit for
development work.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Project

- **Bounded scope** — a project has an end. If it doesn't, it's a workstream or needs
  to be split into independent projects.
- **Decomposable into workstreams** — parallel tracks with clear interface contracts.
  If everything must happen serially, the project is too small or the decomposition
  is wrong.
- **Sequenced milestones** — M1 is thin and proves architecture end-to-end. Later
  milestones add capability.
- **Traceable** — `from_discovery` if it originated from a validated discovery idea.

## Moves

### Capture a new project

Develop the project through conversation before writing.

- **What does done look like?** The demo for the final milestone.
- **What are the natural parallel tracks?** These become workstreams.
- **What does M1 prove?** Thinnest end-to-end slice; not a vertical feature,
  architecture.
- **Who else does this touch?** Dependencies on other projects, teams, systems.
- **What could kill this?** Set up the pre-mortem mindset.

### Decompose into workstreams and milestones

This is usually not a first-session activity. Capture the project shell, then come
back via the plan skill for decomposition. The project's `workstreams` and
`milestones` fields are populated as those artifacts are created.

### Update an existing project

**Status lifecycle:** `scoping → planning → in_progress → complete | abandoned`

- scoping → planning: scope agreed, decomposition starting
- planning → in_progress: at least one workstream active on M1
- in_progress → complete: all milestones done; demo criteria met
- any → abandoned: decision to stop; record why

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/projects/`. Include
frontmatter with `name`, `type: project`, `status: scoping`, `parent: null`,
`children: []`, `workstreams: []`, `milestones: []`, `from_discovery` if applicable. Body: overview, goals, constraints, team, non-goals. Always show before
writing.

## Failure Modes

- Project scope is too big (multi-team, multi-quarter) — split into independent projects with explicit dependencies.
- No natural workstreams — either too small for a project, or decomposition is wrong.
- M1 is "all the hard parts" — that's a big-bang, not a milestone.
- Project children never populated — the work is happening but ungraphed.
