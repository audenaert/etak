# Project

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create, update, and refine projects — the primary coordination unit for
development work.

## Schema

```yaml
---
name: "OAuth2 migration"
type: project
status: scoping  # scoping | planning | in_progress | complete | abandoned
parent: null
children:
  - epic-oauth2-provider-integration
  - epic-token-management
workstreams:
  - workstream-backend-auth
  - workstream-frontend-auth
milestones:
  - milestone-m1-google-oauth-login
  - milestone-m2-multi-provider
from_discovery: idea-modern-auth-flow
---
```

Body: project overview, goals, constraints, team. Adjacent artifacts (specs,
ADRs, epics, workstreams, milestones) live at their own canonical paths and
link via slugs in frontmatter — projects are a single file, not a directory.

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

Generate a kebab-case filename and write to the canonical path in the artifacts registry. Include
frontmatter with `name`, `type: project`, `status: scoping`, `parent: null`,
`children: []`, `workstreams: []`, `milestones: []`, `from_discovery` if applicable. Body: overview, goals, constraints, team, non-goals. Always show before
writing.

## Failure Modes

- Project scope is too big (multi-team, multi-quarter) — split into independent projects with explicit dependencies.
- No natural workstreams — either too small for a project, or decomposition is wrong.
- M1 is "all the hard parts" — that's a big-bang, not a milestone.
- Project children never populated — the work is happening but ungraphed.
