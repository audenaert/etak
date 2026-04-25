---
name: workstream
description: >
  Create and update workstreams — parallel delivery tracks within a project, each
  owning interface contracts with its neighbors. Internal skill called by plan;
  never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Workstream

You help create and update workstreams — parallel delivery tracks within a project,
defined by boundaries, contracts, and integration points.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Workstream

- **Natural boundary** — workstreams reflect a real seam in the system
  (frontend/backend, ingestion/serving, data/app). Contrived workstreams make
  coordination worse, not better.
- **One owner** — a person or small team. If nobody owns it, it's not a
  workstream.
- **Interface contracts** — explicit APIs, data shapes, or protocols between
  workstreams. "We'll figure it out" is where parallel work goes to die.
- **Integration points** — specific milestones where workstreams converge and
  integrate. Not every milestone; the ones that matter.

## Moves

### Capture a new workstream

- **What's the boundary?** Draw it concretely — what's in, what's out.
- **What does it depend on?** Other workstreams, external systems, prior
  milestones.
- **What depends on it?** Who consumes its output.
- **What's the interface contract?** Minimum: the API or data contract at the
  boundary. Write it as testable text, not "we'll coordinate."
- **Who owns it?** Can be deferred, but ideally named.

### Update interface contracts

Contracts evolve. When they change:
- Note the change and the reason
- Make sure the consuming workstream is aware
- If the change is breaking, treat it like an ADR-level decision

### Update status

**Status lifecycle:** `active ⇄ blocked → complete`

- active → blocked: something external is holding the stream up; name it
- blocked → active: unblocked; note resolution
- active → complete: all owned epics/stories done

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/workstreams/`. Include
frontmatter with `name`, `type: workstream`, `project`, `owner`, `status: active`,
`interface_contracts` (list of human-readable contract statements),
`integration_points` (list of `{milestone, description}`). Body: scope, inclusions,
exclusions, key decisions. Always show before writing.

## Failure Modes

- One-workstream project — that's just a project; don't add the layer.
- Workstreams that are really skill groups ("backend team," "frontend team")
  rather than work seams — rethink.
- Workstreams with no interface contracts — integration chaos incoming.
- Blocked without a named blocker — "blocked" as status without a reason is noise.
