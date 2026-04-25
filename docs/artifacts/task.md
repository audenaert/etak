# Task

**Role:** engineering-frame unit of technical work that enables or supports a story or epic. Optional.

## Why it exists

Tasks describe changes to the *system*, not to the *user's experience*. "Migrate users table to add `provider_id` column" is a task. "User can sign in with Google" is a story. The frame is deliberate: stories live in the language of the product; tasks live in the language of the code.

Tasks are optional. This is a settled design decision. For clear stories where the implementation is obvious from the ACs and a look at the codebase, tasks are ceremony. Skip them. Direct implementation is honest.

Create tasks when decomposition does real work. Non-user-visible technical changes (migrations, feature flags, telemetry, refactors) need a home that isn't a story AC. Cross-story technical work (schema changes, shared utilities) needs a parent that isn't a single story; use the epic. Multi-PR stories benefit from per-PR traceability. Complex stories where the decomposition *is* the design work benefit from the breakdown.

The test for whether a task earns its place is simple. If the task title reads like a restatement of the story's AC in engineering voice, delete the task and implement the story. If the task describes a concrete technical change that wouldn't have been obvious without the decomposition, keep it.

## When to create one

- When the technical work isn't user-visible (database migrations, feature flag rollout, telemetry setup, refactors that unblock a feature).
- When multiple technical changes compose a single user capability and you want each change reviewable independently.
- When a technical change supports multiple stories (parent to the epic instead of a single story).
- When compliance or audit requires fine-grained change traceability (one PR per task).
- When decomposition is real design work for a complex story.

## When to skip

- The story is a single obvious PR.
- "Tasks" would just restate the ACs in engineering voice.
- You're adding tasks because the template suggests them.

## Schema

```yaml
---
name: "Add Google OAuth callback handler to auth router"
type: task
status: todo  # todo | in_progress | done | blocked
parent: story-user-can-sign-in-with-google
workstream: workstream-backend-auth
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Concrete technical change, not "work on X". |
| `type` | yes | Always `task`. |
| `status` | yes | Current lifecycle state. |
| `parent` | yes | Slug of the parent story, or of an epic for cross-story tasks. |
| `workstream` | no | Slug of the workstream that owns the task. |

## Status lifecycle

```
todo  ─────▶  in_progress  ─────▶  done
any   ─────▶  blocked       ─────▶  in_progress  (unblocked)
```

- **todo** — ready to pick up.
- **in_progress** — work started.
- **done** — PR merged or change verified.
- **blocked** — external dependency is holding it up. Name the blocker. "Waiting on review" is a waiting state, not a block.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `parent` | [Story](story.md) or [Epic](epic.md) |
| Outgoing | `workstream` | [Workstream](workstream.md) |

## Example

```markdown
---
name: "Add Google OAuth callback handler to auth router"
type: task
status: todo
parent: story-user-can-sign-in-with-google
workstream: workstream-backend-auth
---

## What Changes

- `src/auth/router.ts`: add `GET /auth/google/callback` route
- `src/auth/providers/google.ts`: implement token exchange with Google
- `src/auth/session.ts`: issue session from OAuth token; no change to the public
  session API
- `tests/auth/google-callback.test.ts`: new file, covers success, cancel, and
  denied paths

## Why

Enables the "User can sign in with Google" story. The callback is the
server-side half of the flow; the client-side redirect is covered by the
frontend workstream in a sibling task.

## Approach

Use the existing `passport-google-oauth20` strategy for token exchange. Session
hand-off goes through the existing session factory; no new storage. All three
AC states map to distinct branches in the callback handler.

## Open Questions

- Error format for the denied-access state: JSON envelope or redirect with
  query param? Default to redirect with `?error=access_denied` unless the
  frontend wants otherwise.
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when decomposing a complex story, or through [build](../skills/build.md) when the engineer discovers mid-implementation that the work is naturally several changes.

### Cross-story tasks at the epic level

When a single technical change enables multiple stories (a schema change, a shared utility, a migration), create the task with `parent: <epic>` and note in the body which stories depend on it.

### Update status

Advance through the lifecycle as work proceeds. When blocked, name the blocker. [Survey](../skills/survey.md) reads blocked tasks and surfaces them during standups.

### Build

[Build](../skills/build.md) or the developer agent picks up a task in `todo`, works it through TDD, and opens a PR. On merge, status moves to `done`.

## Tips

- **One change, one PR.** A task is a thing one PR closes. If it takes multiple PRs, it's multiple tasks.
- **No personas, no outcomes.** File paths, schemas, interfaces, configurations. That's task language.
- **Name the concrete change.** "Add Google OAuth callback handler" is concrete; "Work on OAuth callback" is not.
- **Cross-story tasks parent to the epic.** Not to one of the stories they enable.
- **Blocked with a reason.** "Blocked" as status without a named blocker is noise.
- **Don't ceremonially decompose.** Six tasks under a one-PR story is theater.

## Related

- [Story](story.md) — the user-frame parent a task usually serves.
- [Epic](epic.md) — parent for cross-story technical work.
- [Spike](spike.md) — use this instead when the work is "figure out how to X" rather than "do X".
- [Chore](chore.md) — use this when the work is maintenance without a story parent.
- [Plan skill](../skills/plan.md) — decompose a story into tasks when it helps.
- [Build skill](../skills/build.md) — implement a task.
- [Artifacts overview](README.md)
