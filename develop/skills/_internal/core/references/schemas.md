# Development Node Schemas

All development artifacts live in `docs/development/` within the current project,
organized by type:

```
docs/development/
  initiatives/
  projects/
  epics/
  stories/
  tasks/
  work-items/    # bug, chore, enhancement
  spikes/
  specs/
  adrs/
  workstreams/
  milestones/
```

## File Naming

Use kebab-case derived from the node name:

- `oauth2-migration.md`
- `user-can-sign-in-with-google.md`
- `adr-001-use-passport-js-for-oauth.md`

ADRs are numbered sequentially per project (`adr-001-*`, `adr-002-*`). Other artifacts
use a name-derived slug.

## Common Fields

Every work item shares a small common header:

```yaml
---
name: "Human-readable title"
type: initiative | project | epic | story | task | bug | chore | enhancement | spike | spec | adr | workstream | milestone
status: <varies by type>
parent: <slug of parent work item, if any>
children: []  # slugs of child work items
from_discovery: <slug of discovery idea, if originated there>
---
```

Design artifacts (`spec`, `adr`) use `for` instead of `parent` to indicate the work
item they describe. Workstreams and milestones use `project` to attach to their
project.

## Typed Links

| From        | Link field                | To                                     |
|-------------|---------------------------|----------------------------------------|
| Work item   | `parent`                  | Parent work item                       |
| Work item   | `children`                | Child work items                       |
| Work item   | `from_discovery`          | Discovery idea (etak-discovery)        |
| Epic/Story  | `workstream`              | Workstream                             |
| Epic/Story  | `milestone`               | Milestone                              |
| Project     | `workstreams`             | Workstreams within the project         |
| Project     | `milestones`              | Milestones within the project          |
| Spec        | `for`                     | Project / Epic / Story                 |
| Spec        | `adrs`                    | ADRs produced from this spec           |
| ADR         | `for`                     | Project                                |
| ADR         | `superseded_by`           | Later ADR that replaces this one       |
| Workstream  | `project`                 | Containing project                     |
| Workstream  | `interface_contracts`     | API contracts with other workstreams   |
| Workstream  | `integration_points`      | Milestone sync points                  |
| Milestone   | `project`                 | Containing project                     |
| Milestone   | `workstream_deliverables` | What each workstream delivers          |

## Initiative

Multi-project strategic container.

```yaml
---
name: "Platform authentication overhaul"
type: initiative
status: proposed  # proposed | active | paused | complete | abandoned
parent: null
children:
  - project-oauth2-migration
from_discovery: idea-modern-auth-flow
---
```

Body: Strategic context, success criteria, timeline constraints.

## Project

Bounded deliverable. The coordination hub for most work.

```yaml
---
name: "OAuth2 migration"
type: project
status: scoping  # scoping | planning | in_progress | complete | abandoned
parent: initiative-platform-auth-overhaul
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

Body: Project overview, goals, constraints, team.

## Workstream

Parallel delivery track within a project.

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

Body: Scope boundary, what's included, what's excluded, key decisions.

## Milestone

Sequenced project checkpoint. Thin, end-to-end, demo-able.

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
    delivers: "Google OAuth endpoint, token issuance"
  - workstream: workstream-frontend-auth
    delivers: "Login button, token storage, auth state"
demo_criteria: "User can click 'Sign in with Google' and land on authenticated dashboard"
---
```

Body: What this milestone proves, what it enables, what it defers.

## Epic

Themed group of related stories.

```yaml
---
name: "OAuth2 provider integration"
type: epic
status: draft  # draft | ready | in_progress | complete
parent: project-oauth2-migration
children:
  - story-google-oauth-flow
  - story-github-oauth-flow
workstream: workstream-backend-auth
milestone: milestone-m2-multi-provider
---
```

Body: Scope, acceptance criteria at the epic level.

## Story

User-frame increment with testable acceptance criteria. Stories describe the change
from the user's point of view. Technical work that enables a story belongs in
**tasks** (see below), not in the story itself.

Stories use the formulaic opener "As a \<persona>, I want \<capability>, so that
\<outcome>" in the `user_story` field, plus testable ACs in `acceptance_criteria`.

```yaml
---
name: "User can sign in with Google"
type: story
status: draft  # draft | ready | in_progress | complete
parent: epic-oauth2-provider-integration
children: []  # optional — tasks, only when decomposition adds clarity
workstream: workstream-backend-auth
milestone: milestone-m1-google-oauth-login
from_discovery: idea-modern-auth-flow  # optional — compliance traceability
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

Body: context, user goals, notes, out-of-scope. Links to spec if one exists.

Tasks are **optional**. For clear stories where the implementation is obvious from
the ACs plus a quick look at the codebase, skip tasks. Break down into tasks when
the work includes non-user-visible technical changes (migrations, feature flags,
telemetry), spans multiple PRs, or when decomposition is itself useful design work.

## Task

Engineering-frame unit of technical work that enables or supports a story or epic.
Tasks describe changes to the *system*, not to the *user's experience*. A task
should be a few hundred lines of code / 1-3 days of work for a human, or one
focused AI session.

Tasks usually parent to a story. For cross-story technical work that enables
multiple stories (schema changes, shared utilities, migrations), task parent can
be an epic instead.

```yaml
---
name: "Add Google OAuth callback handler to auth router"
type: task
status: todo  # todo | in_progress | done | blocked
parent: story-user-can-sign-in-with-google  # or epic slug for cross-story work
workstream: workstream-backend-auth
---
```

Body: what changes (files, interfaces, schemas), why (which story/epic this
enables), approach, open questions.

**When to create tasks:**

- Technical work isn't user-visible (migrations, feature flags, telemetry,
  refactors)
- Multiple technical changes compose one user capability
- A technical change supports multiple stories (parent is the epic)
- Compliance requires per-change traceability
- Decomposition is actual design work for a complex story

**When to skip tasks:**

- Story is a single, obvious PR
- "Tasks" would just restate ACs in engineering voice

## Work Items (Bug, Chore, Enhancement)

Standalone work items that don't require the initiative/project/epic hierarchy. They
live together in `docs/development/work-items/` and differ by `type` and lifecycle.

### Bug

```yaml
---
name: "OAuth token refresh fails silently on slow connections"
type: bug
status: open  # open | investigating | fix_in_progress | resolved
severity: high  # critical | high | medium | low
parent: null
---
```

Body: Steps to reproduce, expected vs actual behavior, environment.

### Chore

```yaml
---
name: "Update OAuth dependency to v4.2"
type: chore
status: todo  # todo | in_progress | done
parent: null
---
```

Body: Why this is needed, any risks.

### Enhancement

```yaml
---
name: "Add loading spinner to login button"
type: enhancement
status: todo  # todo | in_progress | done
parent: null
---
```

Body: What it improves, why it matters.

## Spike

Time-boxed investigation. Produces a finding, a proof-of-concept, or an ADR.

```yaml
---
name: "Evaluate OAuth libraries for multi-provider support"
type: spike
status: planned  # planned | in_progress | complete
time_box: "2 days"
question: "Which OAuth library best supports Google + GitHub + SAML with minimal custom code?"
decision_criteria: "Supports all 3 providers, <500 lines of glue code, active maintenance"
outcome: null  # finding | proof_of_concept | adr
result: null
---
```

Body: Investigation plan, what to look at, how to evaluate.

## Spec

Technical specification. Grounded in the actual codebase, not hypothetical
architecture.

```yaml
---
name: "OAuth2 token management design"
type: spec
status: draft  # draft | review | approved | superseded
for: project-oauth2-migration
adrs: []
---
```

Body sections (recommended):

1. **Context** — what problem, what constraints, why now
2. **Current state** — what exists in the codebase today, relevant files and patterns
3. **Proposed approach** — the design, grounded in existing code
4. **Non-functional requirements** — performance, reliability, security
5. **Alternatives considered** — what was rejected and why
6. **Open questions** — what still needs resolution
7. **Risks** — what could go wrong

## ADR

Architecture decision record. Immutable once accepted; create a new ADR if the
decision changes.

```yaml
---
name: "ADR-001: Use passport.js for OAuth provider abstraction"
type: adr
status: proposed  # proposed | accepted | deprecated | superseded
for: project-oauth2-migration
superseded_by: null
---
```

Body sections:

1. **Context** — what forced the decision
2. **Decision** — what we chose
3. **Alternatives considered** — what else we evaluated
4. **Consequences** — what this commits us to, good and bad

## Status Lifecycles

| Type        | Statuses                                                  |
|-------------|-----------------------------------------------------------|
| Initiative  | proposed, active, paused, complete, abandoned             |
| Project     | scoping, planning, in_progress, complete, abandoned       |
| Epic        | draft, ready, in_progress, complete                       |
| Story       | draft, ready, in_progress, complete                       |
| Task        | todo, in_progress, done, blocked                          |
| Bug         | open, investigating, fix_in_progress, resolved            |
| Chore       | todo, in_progress, done                                   |
| Enhancement | todo, in_progress, done                                   |
| Spike       | planned, in_progress, complete                            |
| Spec        | draft, review, approved, superseded                       |
| ADR         | proposed, accepted, deprecated, superseded                |
| Workstream  | active, blocked, complete                                 |
| Milestone   | planned, in_progress, complete                            |

## Traceability and Compliance

`from_discovery` is a **one-way** link: development points back to discovery, discovery never points forward. Populate on any artifact that originated from a validated discovery idea. Omit for bugs, chores, and enhancements from production, ops, or engineering judgment.

For compliance, pair `from_discovery` with task-level granularity (one PR per task) to produce the full chain: discovery idea → story → task → merged commit.

## Readiness Rules of Thumb

A story is **ready** when:

- It has a clear user-facing name and a complete `user_story` (persona, capability,
  outcome)
- Its acceptance criteria are testable (each AC should map to at least one verifiable
  check)
- It has a parent epic (or is explicitly a standalone)
- Any dependencies are identified
- Either its approach is obvious from the codebase, a spec exists, or tasks are
  broken down

An epic is **ready** when:

- Its stories are identified (not necessarily all ready)
- Workstream assignment is clear
- Its milestone (if any) is identified

A project is **ready** to enter `in_progress` when:

- Its workstreams are defined with interface contracts
- At least one milestone is sequenced
- The thinnest M1 proves architecture end-to-end (not just one vertical slice)
- Key risks are named
