---
name: task
description: >
  Create and update tasks — engineering-frame units of technical work that enable
  or support a story or epic. Tasks are optional and complement stories; they
  don't duplicate them. Internal skill called by plan and build; never invoked
  directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Task

You help create and update tasks — units of **technical work** that enable or
support a story or epic. Tasks describe changes to the *system*; stories describe
changes to the *user's experience*. That's the distinction to hold.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## The Engineering Frame

Tasks live in the language of the system, not the language of the user. A task
describes a technical change, an engineering deliverable. "Migrate users table to
add `provider_id` column" is a task. "User can sign in with Google" is a story.

A well-written task answers:
- **What changes** — files, schemas, configs, endpoints, interfaces
- **Why** — which story or epic this enables, what constraint it lifts
- **How big** — roughly a few hundred lines of code, 1-3 days for a human
  developer, or one focused AI session

Tasks should be small enough to complete in one sitting and produce one coherent
commit or PR. If a task doesn't fit that shape, it's probably a story or a small
project.

## When to Create Tasks

**Not every story needs tasks.** This is important. For clear stories where the
implementation is obvious from the ACs plus a quick look at the codebase, skip
tasks and implement directly. The story itself is the unit of work.

Create tasks when:

- **Technical work isn't user-visible.** Database migrations, feature flag
  rollout, telemetry setup, refactors that unblock a feature, test infrastructure.
  These need a home that isn't an AC.
- **Multiple technical changes compose a single user capability.** Breaking the
  work into tasks makes each change reviewable independently.
- **A technical change supports multiple stories.** Cross-story tasks have
  `parent: <epic>` instead of a single story.
- **Compliance or audit requires fine-grained traceability.** One PR per task,
  one task per distinct change.
- **Decomposition is actual design work.** For hard stories, breaking down tasks
  *is* the engineer's (or AI's) thinking-out-loud. Not ceremony — real reasoning.

Avoid tasks when:

- The story is a single obvious PR
- The "tasks" would just restate the ACs in engineering voice
- You're adding tasks because the template suggests them, not because they add
  clarity

## What Makes a Good Task

- **Engineering-framed.** No personas, no outcomes. File paths, schemas,
  interfaces, configurations.
- **One change.** A task is a thing a PR closes. If it takes multiple PRs, it's
  multiple tasks.
- **Parented correctly.** Usually `parent: <story>`. For cross-story work, `parent:
  <epic>`.
- **Not a restatement of the AC.** If the story says "user can sign in with
  Google" and the task says "implement user signing in with Google," one of
  them needs to go — probably the task.

## Moves

### Capture a task under a story

- **Name the technical change.** "Add Google OAuth callback handler to auth
  router" names a concrete change. "Work on OAuth callback" does not.
- **Implementation notes.** Files involved, approach, any gotchas. Enough that
  someone picking up the task knows where to start.
- **Sequence if needed.** Some tasks block others; note the ordering.

### Capture a cross-story task at the epic level

When a single technical change enables multiple stories — a schema change, a
shared utility, a migration — create the task with `parent: <epic>` and note in
the body which stories depend on it.

### Update a task

**Status lifecycle:** `todo → in_progress → done | blocked`

- todo → in_progress: work started
- in_progress → done: PR merged or change verified
- any → blocked: capture *what* blocks it — "waiting on review" is a waiting
  state, not a block; "waiting on workstream B's endpoint" is a block

### Write the artifact

Generate a kebab-case filename describing the technical change. Write to
`docs/development/tasks/`. Frontmatter:

```yaml
---
name: "Add Google OAuth callback handler to auth router"
type: task
status: todo
parent: story-user-can-sign-in-with-google   # or an epic slug, for cross-story work
workstream: workstream-backend-auth
---
```

Body sections:

1. **What changes** — concretely, at the level of files and interfaces
2. **Why** — which story or epic this enables, or what technical constraint it
   lifts
3. **Approach** — one-paragraph sketch; full design belongs in a spec
4. **Open questions** — anything that needs resolving before or during the work

Always show before writing.

## Failure Modes

- **Task that restates the AC.** Adds no information. Either delete it and just
  implement the story, or reframe the task around the technical change.
- **Task that's really a story.** It describes user-visible behavior with a
  persona and outcome — should be promoted.
- **Task that's really a spike.** It says "investigate X" or "figure out how to
  Y." Spikes are a separate artifact with a time-box and decision criteria. Use
  [../spike/SKILL.md](../spike/SKILL.md).
- **Ceremonial decomposition.** Six tasks under a one-PR story. Delete the tasks;
  just implement.
- **Orphan task.** No parent story or epic. Either wire up or — if it's really
  standalone operational work — reclassify as a chore.
- **"Work on X" tasks.** Vague scope, no clear end. Rewrite as a concrete change
  or split into multiple tasks with specific deliverables.
