# Workstream

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create and update workstreams — parallel delivery tracks within a project,
defined by boundaries, contracts, and integration points.

## Schema

```yaml
---
name: "Backend auth services"
type: workstream
project: project-oauth2-migration
owner: null
status: active  # active | blocked | complete
interface_contracts:
  - "POST /auth/token — returns JWT, consumed by frontend workstream"
integration_points:
  - milestone: milestone-m1-google-oauth-login
    description: "Backend must serve /auth/google endpoint before frontend can integrate"
---
```

Body: scope boundary, what's included, what's excluded, key decisions.

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

Generate a kebab-case filename and write to the canonical path in the artifacts registry. Include
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
