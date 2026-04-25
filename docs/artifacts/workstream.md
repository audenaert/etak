# Workstream

**Role:** parallel delivery track within a project, defined by a boundary and its interface contracts. Cuts across the project hierarchy.

## Why it exists

Workstreams model where a project divides along a seam in the system. Backend and frontend. Ingestion and serving. Data pipeline and application. When a project is worth decomposing, the decomposition is usually along these seams, not across them.

A workstream makes the seam explicit. It names what's inside, what's outside, and what contracts the workstream owes its neighbors. "The backend-auth workstream exposes `POST /auth/token` returning a JWT; the frontend-auth workstream consumes it." That sentence is the kind of thing a workstream carries. Without it, parallel work drifts, and the two sides discover at integration time that they made incompatible assumptions.

Workstreams are orthogonal to epics and stories. An epic sits inside one workstream. A story sits inside one workstream. The workstream holds the boundary; the containment hierarchy holds the work. Both matter.

## When to create one

- When decomposing a project along a real system seam.
- When two teams or two engineers will work in parallel and need explicit interface contracts.
- When a set of epics or stories naturally belongs together because they share a boundary.

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
  - "GET /auth/session — returns current user from session cookie"
integration_points:
  - milestone: milestone-m1-google-oauth-login
    description: "Backend serves /auth/google before frontend integrates"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short name for the delivery track. |
| `type` | yes | Always `workstream`. |
| `project` | yes | Slug of the containing project. |
| `owner` | no | Person or team responsible. Can be deferred. |
| `status` | yes | Current lifecycle state. |
| `interface_contracts` | yes | Human-readable contract statements with neighboring workstreams. |
| `integration_points` | yes | List of `{milestone, description}` for milestones where workstreams converge. |

## Status lifecycle

```
active  ⇄  blocked  ─────▶  complete
```

- **active** — work is proceeding.
- **blocked** — something external is holding the stream up. Name the blocker.
- **complete** — all owned epics and stories are done.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `project` | [Project](project.md) |
| Outgoing | `integration_points[].milestone` | [Milestone](milestone.md) |
| Incoming | Epics and stories link via `workstream` | this workstream |
| Incoming | Milestones reference via `workstream_deliverables` | this workstream |

## Example

```markdown
---
name: "Backend auth services"
type: workstream
project: project-oauth2-migration
owner: "backend-team"
status: active
interface_contracts:
  - "POST /auth/token — exchanges OAuth code for a JWT, consumed by frontend workstream"
  - "GET /auth/session — returns the authenticated user from the session cookie, consumed by frontend and by downstream services"
  - "Session cookie format is unchanged from the current session model"
integration_points:
  - milestone: milestone-m1-google-oauth-login
    description: "Backend must serve /auth/google/callback and issue a session before frontend integrates the login button"
  - milestone: milestone-m2-multi-provider
    description: "Provider abstraction stable; frontend can dispatch to any provider uniformly"
---

## Scope

All server-side auth: OAuth callback handlers, provider abstraction, token
exchange, session issuance, the session verification middleware.

## In

- `src/auth/*` (routes, providers, session factory)
- The session middleware consumed by every authenticated route
- The database schema for users and identities

## Out

- The login button and provider selection UI (frontend workstream)
- Authorization policy (belongs to the permissions workstream in a separate project)

## Key Decisions

- Use `passport.js` for provider abstraction (see ADR-001)
- Keep the existing session cookie format; the OAuth work changes issuance, not
  the public session contract
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when decomposing a project. The plan skill walks boundary, dependencies, dependents, and interface contracts.

### Update interface contracts

Contracts evolve. When they change, note the change and the reason. If the change is breaking, treat it like an ADR-level decision and capture it in [an ADR](adr.md). Make sure the consuming workstream knows.

### Survey

[Survey](../skills/survey.md) shows which of a workstream's epics and stories are ready, in progress, blocked, or complete.

## Tips

- **Real seams only.** Workstreams that are really skill groups ("backend team," "frontend team") rather than work seams add coordination overhead without adding clarity.
- **One owner.** If nobody owns it, it's not a workstream.
- **Contracts are testable text, not aspirations.** "We'll coordinate" is where parallel work fails.
- **Integration points on milestones that matter.** Not every milestone. The ones where workstreams actually converge.
- **Blocked needs a reason.** Name the blocker, or the status is just noise.
- **One-workstream projects don't need workstreams.** That's just a project.

## Related

- [Project](project.md) — container the workstream lives inside.
- [Milestone](milestone.md) — sync points where workstreams converge.
- [Epic](epic.md) — epics live inside one workstream.
- [Story](story.md) — stories live inside one workstream.
- [ADR](adr.md) — breaking contract changes warrant one.
- [Plan skill](../skills/plan.md) — decompose projects into workstreams.
- [Artifacts overview](README.md)
