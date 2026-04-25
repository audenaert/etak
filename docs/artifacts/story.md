# Story

**Role:** user-frame increment of value with testable acceptance criteria. The primary implementation unit.

## Why it exists

A story describes a change from the user's point of view: what a persona wants, what capability they get, and what outcome they're after. It is the bridge from product intent to engineering work. When you read a story, you should see the user standing at the center.

Stories hold the engineering team to the user frame. The formulaic opener (*as a persona, I want a capability, so that an outcome*) forces three honest answers. If any slot is fuzzy, the story isn't ready. If the persona is always "an engineer," you're writing a task, not a story.

Stories are where acceptance criteria live. ACs are testable assertions about what the user can do when the story is complete. A story without ACs is a title. A story with ACs that restate the title is a formality. Well-written ACs are the contract between product and engineering, and the seed of the test plan.

## When to create one

- When decomposing an epic into user-facing increments.
- When a standalone user capability needs tracking outside an epic (rare).
- When a validated idea moves into execution and the first engineering unit is a specific user-facing change.
- When someone describes a feature request. Capture it as a story if it passes the persona-goal-outcome test.

## Schema

```yaml
---
name: "User can sign in with Google"
type: story
status: draft  # draft | ready | in_progress | complete
parent: epic-oauth2-provider-integration
children: []  # optional tasks, only when decomposition helps
workstream: workstream-backend-auth
milestone: milestone-m1-google-oauth-login
from_discovery: idea-modern-auth-flow
user_story: |
  As an unauthenticated visitor who already uses Google Workspace,
  I want to sign in using my Google account,
  so that I don't have to remember a separate password for this product.
acceptance_criteria:
  - "Given an unauthenticated user, when they click 'Sign in with Google', then they are redirected to Google's OAuth consent screen"
  - "Given a user who completes Google OAuth, when redirected back, then they are authenticated and see their dashboard"
  - "Given a user who cancels the Google consent screen, when they return to the app, then they see a clear message and can retry"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short user-facing title describing what the user can do. |
| `type` | yes | Always `story`. |
| `status` | yes | Current lifecycle state. |
| `parent` | yes | Slug of the parent epic (or explicitly standalone). |
| `children` | no | Slugs of tasks under this story. Optional. |
| `workstream` | yes | Slug of the workstream that owns the story. |
| `milestone` | no | Slug of the target milestone. |
| `from_discovery` | no | Slug of the validated discovery idea, if any. |
| `user_story` | yes | Formulaic persona-goal-outcome statement. |
| `acceptance_criteria` | yes | List of testable ACs, one per item. |

## Tasks are optional

Tasks under a story are optional. If the implementation is obvious from the ACs and a quick look at the codebase, skip them. Break a story into tasks when:

- The technical work isn't user-visible (migrations, feature flags, telemetry)
- Multiple PRs will close the story and you want each to map to a trackable unit
- Decomposition is itself useful design work
- Compliance requires per-change traceability

For clear, single-PR stories, direct implementation is honest. Don't add tasks because the template suggests them.

## Status lifecycle

```
draft  ─────▶  ready  ─────▶  in_progress  ─────▶  complete
```

- **draft** — captured but not yet implementable. ACs rough or missing.
- **ready** — ACs are testable, parent epic exists, approach is clear (from codebase, from a spec, or from tasks).
- **in_progress** — work started.
- **complete** — ACs verified; PR merged.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `parent` | [Epic](epic.md) |
| Outgoing | `workstream` | [Workstream](workstream.md) |
| Outgoing | `milestone` | [Milestone](milestone.md) |
| Outgoing | `from_discovery` | [Idea](idea.md) in `docs/discovery/` |
| Incoming | Tasks link via `parent` | this story |
| Incoming | Specs link via `for` | this story |

## Example

```markdown
---
name: "User can sign in with Google"
type: story
status: ready
parent: epic-oauth2-provider-integration
children: []
workstream: workstream-backend-auth
milestone: milestone-m1-google-oauth-login
from_discovery: idea-modern-auth-flow
user_story: |
  As an unauthenticated visitor who already uses Google Workspace,
  I want to sign in using my Google account,
  so that I don't have to remember a separate password for this product.
acceptance_criteria:
  - "Given an unauthenticated user, when they click 'Sign in with Google', then they are redirected to Google's OAuth consent screen"
  - "Given a user who completes Google OAuth, when redirected back, then they are authenticated and see their dashboard"
  - "Given a user who cancels the Google consent screen, when they return to the app, then they see a clear message and can retry"
  - "Given a user whose Google email is not allow-listed for the tenant, when redirected back, then they see an access-denied message with support contact info"
---

## Context

First provider flow for the OAuth2 migration. Google Workspace is the most
common identity provider among current enterprise evaluators, so Google is M1.

## User Goals

Enterprise users already signed into Google don't want to create or remember
another password. They want sign-in to feel like every other Google-integrated
tool they use.

## Notes

Session hand-off uses the existing session abstraction; see spec
`oauth2-token-management-design.md` for how the OAuth access token maps into
a session. No new session storage is required for this story.

## Out of Scope

- Sign-up (this story is sign-in for existing users)
- Account linking (covered by a later story)
- Multi-factor enrollment prompts
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when decomposing an epic, or directly when someone describes a user-facing change. The plan skill walks the persona-goal-outcome framing.

### Refine ACs

ACs rot. When picking up a story for implementation, re-read them and sharpen. [Assess](../skills/assess.md) surfaces ACs that aren't testable, ACs that encode implementation, and missing states (errors, empty states, loading states).

### Produce a spec

```
Write a spec for this story.
```

[Spec](../skills/spec.md) drafts a grounded technical design when the implementation isn't obvious. Not every story needs one.

### Break into tasks

When decomposition adds value, [plan](../skills/plan.md) generates task candidates. Each task describes a technical change that enables part of the story.

### Assess readiness

```
Is this story ready to build?
```

[Assess](../skills/assess.md) checks the readiness rules of thumb.

### Build

When `status: ready`, hand the story to [build](../skills/build.md) or the developer agent. The story's ACs drive the test plan and the self-review at the end.

## Tips

- **Persona-grounded.** A specific user or role, not "the system" or "we."
- **Testable ACs.** Each AC should map to at least one verifiable check. "It feels responsive" does not.
- **Vertical slice.** One user-visible increment. If it fans out into infrastructure and cross-cutting refactors, those are tasks underneath — or the story is too big and should split.
- **Implementation stays out of ACs.** "User sees a Bootstrap modal" mixes user behavior with implementation. Strip the implementation.
- **Tasks when they add clarity.** Skip them when the story is a single obvious PR. Ceremony isn't rigor.
- **`from_discovery` is optional.** Populate it when the story traces back to a validated discovery idea; omit for engineering-judgment work.

## Related

- [Epic](epic.md) — container the story lives inside.
- [Task](task.md) — optional engineering-frame unit that enables part of a story.
- [Spec](spec.md) — grounded technical design for a story when needed.
- [Idea](idea.md) — the validated discovery idea a story may trace back to.
- [Plan skill](../skills/plan.md) — capture and refine stories.
- [Assess skill](../skills/assess.md) — readiness and AC critique.
- [Build skill](../skills/build.md) — implement a ready story.
- [Artifacts overview](README.md)
