# Project

**Role:** bounded deliverable. The coordination hub for most development work.

## Why it exists

A project is the unit where real coordination happens. It has an end. It has workstreams running in parallel. It has milestones that mark where those workstreams converge. It is small enough that one team (or a few working together) can hold the whole thing in their head, and large enough to justify decomposition.

Most development work lives at the project level. Epics, stories, and tasks are refinements below. The project is where scope, architecture, and sequencing meet, and where most of the cross-role coordination happens.

A project has a demo you can describe. "OAuth2 migration" is done when enterprise customers sign in via OIDC and the old password paths are gone. That demo is the single sentence the project exists to produce.

## When to create one

- When a validated idea has enough weight to need decomposition into parallel tracks.
- When work naturally spans multiple workstreams (frontend/backend, ingestion/serving) and needs interface contracts between them.
- When a discovery idea moves to `ready_for_build` and the engineering team needs a coordination artifact.
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

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short, concrete name for the deliverable. |
| `type` | yes | Always `project`. |
| `status` | yes | Current lifecycle state. |
| `children` | yes | Slugs of epics inside the project. |
| `workstreams` | yes | Slugs of workstreams that run inside this project. |
| `milestones` | yes | Slugs of sequenced milestones. |
| `from_discovery` | no | Slug of the validated discovery idea, if any. |

## Status lifecycle

```
scoping  ─────▶  planning  ─────▶  in_progress  ─────▶  complete
any      ─────▶  abandoned                                        (explicit stop)
```

- **scoping** — shape of the project still being worked out.
- **planning** — scope agreed; workstreams, milestones, and epics being defined.
- **in_progress** — at least one workstream is active against M1.
- **complete** — all milestones demo'd; demo criteria met.
- **abandoned** — stopped before completion. Record why.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `workstreams` | [Workstream](workstream.md) |
| Outgoing | `milestones` | [Milestone](milestone.md) |
| Outgoing | `from_discovery` | [Idea](idea.md) in `docs/discovery/` |
| Incoming | Epics link via `parent` | this project |
| Incoming | Specs and ADRs link via `for` | this project |

## Example

```markdown
---
name: "OAuth2 migration"
type: project
status: planning
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

## Overview

Replace the password-based auth stack with OAuth2, supporting Google as the first
provider and extensible to GitHub and SAML. The final demo is an enterprise user
signing in with Google and landing on the authenticated dashboard with a valid
session.

## Goals

- Remove the password login path from the application
- Support multiple identity providers behind a uniform token model
- Keep the existing authenticated session abstraction unchanged for downstream code

## Constraints

- Must ship in two quarters; enterprise procurement depends on it
- No breaking changes to the session API consumed by other services
- Existing users must migrate without manual intervention

## Team

- Backend: two engineers
- Frontend: one engineer
- Review: security lead, infra lead

## Non-Goals

- SAML integration (planned as a follow-on project)
- Passwordless magic-link flows
- Changes to authorization model
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when a validated idea is ready for execution. The plan skill walks the decomposition: what does done look like, what are the natural parallel tracks, what does M1 prove.

### Decompose into workstreams and milestones

Rarely first-session work. After the project shell exists, return to [plan](../skills/plan.md) to identify workstreams, their interface contracts, and a sequenced set of milestones. M1 should prove the architecture end-to-end, not demonstrate the easiest feature.

### Produce a technical spec

```
Write a spec for the OAuth2 migration.
```

[Spec](../skills/spec.md) drafts a grounded technical design against the actual codebase. Hard-to-reverse decisions that come out of the spec become ADRs.

### Assess readiness

```
Is this project ready to enter in_progress?
```

[Assess](../skills/assess.md) checks the readiness rules of thumb: workstreams have interface contracts, at least one milestone is sequenced, M1 proves architecture end-to-end, key risks are named.

### Survey

[Survey](../skills/survey.md) shows where each workstream stands against its milestones.

## Tips

- **Has a demo.** If you can't describe what done looks like in one sentence, the project isn't scoped yet.
- **M1 proves architecture.** The thinnest end-to-end slice. Not "the easiest feature" and not "all the hard parts working."
- **Workstreams with contracts.** "We'll figure out the API later" is where parallel work goes to die.
- **`from_discovery` carries the chain.** When an idea originated in discovery, the project is where traceability attaches for compliance.

## Related

- [Workstream](workstream.md) — parallel delivery track inside a project.
- [Milestone](milestone.md) — sequenced project checkpoint.
- [Epic](epic.md) — themed group of stories inside a project.
- [Spec](spec.md) — grounded technical design for a project.
- [Plan skill](../skills/plan.md) — scope and decompose projects.
- [Develop overview](../develop.md)
- [Artifacts overview](README.md)
