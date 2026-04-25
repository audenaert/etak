# Milestone

**Role:** sequenced project checkpoint. Thin, end-to-end, and demo-able. Cuts across workstreams.

## Why it exists

A milestone is a date with a demo. It names the point in a project's timeline when workstreams converge, something works end-to-end, and you can show it to someone. Not a phase. Not a category. A specific checkpoint with specific demo criteria.

The first milestone matters most. M1 should prove the architecture works end-to-end: the thinnest slice that shows a request making it from the front door to persistent state and back. It is not the easiest feature. It is not "all the hard parts working." It is the minimum that demonstrates the approach is sound, before later milestones pile capability on top.

Milestones also discipline sequencing. Later milestones build on M1; they don't replace it. If you find yourself planning M3 as a rewrite of M1's architecture, you learned something valuable at M1 and should rescope the project, not the milestone.

## When to create one

- When sequencing the work inside a project.
- When multiple workstreams need an explicit sync point.
- When a shipping checkpoint needs demo criteria and per-workstream deliverables.
- When a validated idea is large enough that the team needs to see intermediate progress, not just the final result.

## Schema

```yaml
---
name: "M1: Google OAuth login working end-to-end"
type: milestone
milestone_type: value  # value | integration | foundation
project: project-oauth2-migration
status: planned  # planned | in_progress | complete
target_date: null
workstream_deliverables:
  - workstream: workstream-backend-auth
    delivers: "Google OAuth callback, token issuance"
  - workstream: workstream-frontend-auth
    delivers: "Login button, token storage, auth state"
demo_criteria: "A user can click 'Sign in with Google' and land on the authenticated dashboard"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Sequenced name like "M1: ..." or "M2: ...". |
| `type` | yes | Always `milestone`. |
| `milestone_type` | yes | `value` (user-visible capability), `integration` (workstreams meeting), or `foundation` (architectural prerequisite). |
| `project` | yes | Slug of the containing project. |
| `status` | yes | Current lifecycle state. |
| `target_date` | no | Optional target date. |
| `workstream_deliverables` | yes | List of `{workstream, delivers}` — what each workstream owes this milestone. |
| `demo_criteria` | yes | Concrete, observable criterion someone can watch happen. |

## Status lifecycle

```
planned  ─────▶  in_progress  ─────▶  complete
```

- **planned** — defined and sequenced. No workstream has started against it yet.
- **in_progress** — at least one workstream is delivering against it.
- **complete** — demo criteria met; all workstream deliverables landed.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `project` | [Project](project.md) |
| Outgoing | `workstream_deliverables[].workstream` | [Workstream](workstream.md) |
| Incoming | Epics and stories link via `milestone` | this milestone |
| Incoming | Workstreams reference via `integration_points` | this milestone |

## Example

```markdown
---
name: "M1: Google OAuth login working end-to-end"
type: milestone
milestone_type: value
project: project-oauth2-migration
status: planned
target_date: 2026-05-30
workstream_deliverables:
  - workstream: workstream-backend-auth
    delivers: "GET /auth/google/callback exchanges code for a session, /auth/session returns the user"
  - workstream: workstream-frontend-auth
    delivers: "Login page with Google button, redirect to /auth/google, consumption of session cookie"
demo_criteria: "An unauthenticated user lands on /login, clicks 'Sign in with Google', completes the Google consent screen, and lands on /dashboard authenticated"
---

## What This Proves

The end-to-end OAuth flow works: client redirect, provider consent, server
callback, session issuance, authenticated state. The architecture holds.

## What This Enables

- M2 (multi-provider): adds GitHub and SAML against the abstraction this
  milestone establishes
- Removal of password-based sign-in paths once a second provider is live

## What This Defers

- Account linking across providers
- Enterprise SSO via SAML
- Session migration for existing password users
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when sequencing a project. The plan skill walks what each milestone proves, enables, and defers, plus the deliverables each workstream owes it.

### Sequence

M1 is special. Spend disproportionate time on it. Force the single sentence that describes what M1 proves; if you can't, the project isn't scoped yet.

### Update status

Advance as work against the milestone progresses. [Survey](../skills/survey.md) reads milestone state and summarizes which deliverables are in, which are outstanding.

## Tips

- **Demo criteria are testable, not aspirational.** "User can X" or "Dashboard shows Y" is concrete. "Robust" and "high quality" are not.
- **Thin, end-to-end.** Especially M1. A project that makes M1 "user can log in" with no plan for how that session integrates with the rest of the system will discover the architecture is wrong at M2.
- **Typed honestly.** A value milestone ships something a user sees. An integration milestone is workstreams meeting. A foundation milestone is architecture. Don't mislabel.
- **Cross-workstream.** Most milestones are where workstreams converge. A single-workstream milestone is usually an epic.
- **Defer explicitly.** What's out of this milestone matters as much as what's in.

## Related

- [Project](project.md) — container the milestone lives inside.
- [Workstream](workstream.md) — delivers into milestones via `workstream_deliverables`.
- [Epic](epic.md) — epics may target a specific milestone.
- [Story](story.md) — stories may target a specific milestone.
- [Plan skill](../skills/plan.md) — sequence milestones.
- [Artifacts overview](README.md)
