---
name: story
description: >
  Create, update, and refine stories — user-frame increments of value with testable
  acceptance criteria. The primary implementation unit. Internal skill called by
  plan, assess, build, and survey; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Story

You help create, update, and refine stories. A story describes a change from the
**user's point of view** — what a persona wants, what capability they get, and what
outcome they're after.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## The User Frame

Stories live in the language of the product, not the language of the system. They
answer "what will the user be able to do?" — they do not describe technical work.
Technical work that enables a story belongs in [tasks](../task/SKILL.md).

Use the formulaic opener:

> **As a** \<persona>,
> **I want** \<capability>,
> **so that** \<outcome>.

If a story can't be written this way, it's either a task in disguise ("implement X") or an outcome without a user ("improve performance").

## What Makes a Good Story

- **Persona-grounded.** A specific user or role, not "the system" or "we." If the
  persona is always "an engineer maintaining this code," you're writing a task,
  not a story.
- **Testable ACs.** Each acceptance criterion maps to at least one verifiable check
  (unit, integration, E2E, or a specific manual verification step). Given/When/Then,
  rule statements, and outcome statements all work. "It feels responsive" does
  not.
- **Right-sized.** One vertical slice of user value. If it fans out into
  infrastructure, migrations, and cross-cutting refactors, those are *tasks*
  underneath — or the scope is too big and the story should split.
- **Parented to an epic** — or explicitly standalone (uncommon for stories; usually
  epic-owned).
- **Tasks are optional.** If the implementation is clear from the ACs plus a
  glance at the codebase, no tasks needed. Break into tasks when the story is
  complex enough that decomposition is actual design work.

## Moves

### Capture a new story

- **Start with the user frame.** Get the persona, goal, and outcome on the table
  before anything else. If any slot is fuzzy, that's a signal the story isn't ready
  yet.
- **Write ACs immediately.** A story without ACs is a title. Aim for 3-5 crisp
  criteria covering the happy path, at least one edge case, and observable
  success.
- **Describe the context a developer needs.** Not the implementation — the user
  and product context. Why is this valuable? What user journey does it fit into?
  What related capabilities already exist?
- **Connect up.** Parent epic, workstream, milestone.
- **Note whether a spec or tasks are needed.** If the implementation is obvious,
  neither. If the design is non-obvious, flag for `/spec`. If the work naturally
  decomposes into discrete technical changes, flag for task breakdown.

### Refine ACs

ACs rot. When a story is being picked up for implementation, re-read the ACs and
sharpen them.

- Each AC should map to at least one verifiable check.
- Look for unstated assumptions — "user can sign in" assumes the user already has
  an account; clarify.
- Look for missing states — errors, empty states, loading states, auth failures.
- Look for ACs that are really implementation details in disguise — move those to
  tasks.

### Break down into tasks (when needed)

Not every story needs tasks. Break down when:
- The story implies technical work that isn't user-visible (migration, feature
  flag rollout, telemetry, instrumentation)
- The implementation is complex enough that decomposition is actual design work
- Multiple PRs will close the story and you want each PR to map to a trackable
  unit
- Compliance or audit requires fine-grained change traceability

For clear, single-PR stories, skip tasks. Direct implementation is honest.

When breaking down, read [../task/SKILL.md](../task/SKILL.md) for the task shape.
Tasks under a story should describe **technical changes**, not restatements of ACs.
If a task description sounds like "do the thing the AC describes," it's not adding
value — just implement the story.

### Update an existing story

**Status lifecycle:** `draft → ready → in_progress → complete`

- draft → ready: ACs are testable, parent epic exists, approach is clear (either
  obvious from codebase, a spec exists, or tasks are broken down)
- ready → in_progress: work started
- in_progress → complete: ACs verified; PR merged

### Write the artifact

Generate a kebab-case filename reflecting the user-facing name. Write to
`docs/development/stories/`. Frontmatter:

```yaml
---
name: "User can sign in with Google"
type: story
status: draft
parent: epic-oauth2-provider-integration
children: []  # optional — tasks, only when decomposition helps
workstream: workstream-backend-auth
milestone: milestone-m1-google-oauth-login
from_discovery: idea-modern-auth-flow   # optional — compliance traceability
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

Body sections:

1. **Context** — what user journey this fits into, why it matters now
2. **User goals** — what the persona is actually trying to accomplish
3. **Notes** — related capabilities, edge cases worth mentioning, links to spec
4. **Out of scope** — explicitly what this story does *not* cover

Always show before writing.

## Failure Modes

- **Task masquerading as a story.** Title is "Implement X" or "Refactor Y." No
  persona, no outcome. Reframe as a task or rewrite in user frame.
- **ACs that restate the title.** "User can sign in" with AC "User can sign in" —
  the AC needs to be testable and specific.
- **Implementation leaking into ACs.** "User sees a Bootstrap modal" mixes user
  behavior and implementation. Strip implementation out; move to tasks or spec.
- **Story that's three stories.** Persona-goal-outcome fires in three different
  directions. Split.
- **Orphan story.** No parent epic, not explicitly standalone. Wire it up.
- **Over-eager task decomposition.** Breaking a one-PR story into five tasks is
  ceremony, not clarity. Just implement it.
