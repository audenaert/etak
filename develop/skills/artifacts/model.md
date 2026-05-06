# Development Work Graph

The model and rules that hold across every development artifact. For per-type
content guidance, see the corresponding `<type>.md` reference. For interaction
posture (thinking partner, signals to surface, anti-patterns), see
[guidelines.md](guidelines.md).

## Hierarchy

```
Project → Epic → Story → Task
```

Standalones (no parent required): Bug, Chore, Enhancement, Spike.

## Two Frames

**Story** = user frame. "As a \<persona>, I want \<capability>, so that
\<outcome>." Describes change from the user's perspective.

**Task** = engineering frame. Describes a technical change that enables or
supports a story or epic. Tasks are optional — skip when the story is a single
obvious PR. Use tasks for non-user-visible work (migrations, feature flags,
telemetry) or cross-story technical changes; parent is the epic for cross-story
work, otherwise the story.

## Design Artifacts

**Spec** — technical specification. `for` a project, epic, or story.
**ADR** — architecture decision record. Immutable after `accepted`; write a
new ADR to supersede, don't amend.

## Common Fields

Every work item shares a small common header:

```yaml
---
name: "Human-readable title"
type: project | epic | story | task | bug | chore | enhancement | spike | spec | adr | workstream | milestone
status: <varies by type>
parent: <slug of parent work item, if any>
children: []  # slugs of child work items
from_discovery: <slug of discovery idea, if originated there>
---
```

Design artifacts (`spec`, `adr`) use `for` instead of `parent` to indicate the
work item they describe. Workstreams and milestones use `project` to attach to
their project.

## Typed Links

| From        | Link field                | To                                     |
|-------------|---------------------------|----------------------------------------|
| Work item   | `parent`                  | Parent work item                       |
| Work item   | `children`                | Child work items                       |
| Work item   | `from_discovery`          | Discovery idea (discovery)             |
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

## Status Lifecycles

| Type        | Statuses                                                  |
|-------------|-----------------------------------------------------------|
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

## File Naming

Use kebab-case derived from the artifact name:

- `oauth2-migration.md`
- `user-can-sign-in-with-google.md`
- `adr-001-use-passport-js-for-oauth.md`

ADRs are numbered sequentially across the entire project (`adr-001-*`,
`adr-002-*`). Other artifacts use a name-derived slug. ADR numbering is
project-wide, not per-spec; check unmerged branches before claiming the next
integer.

## Traceability and Compliance

`from_discovery` is a **one-way** link: development artifacts point back to
discovery, discovery never points forward. Populate on any artifact that
originated from a validated discovery idea. Omit for bugs, chores, and
enhancements that came from production, ops, or engineering judgment.

For compliance, pair `from_discovery` with task-level granularity (one PR per
task) to produce the full chain: discovery idea → story → task → merged commit.

## Readiness Rules of Thumb

A **story** is ready when:

- It has a clear user-facing name and a complete `user_story` (persona,
  capability, outcome)
- Its acceptance criteria are testable (each AC should map to at least one
  verifiable check)
- It has a parent epic (or is explicitly a standalone)
- Any dependencies are identified
- Either its approach is obvious from the codebase, a spec exists, or tasks
  are broken down

An **epic** is ready when:

- Its stories are identified (not necessarily all ready)
- Workstream assignment is clear
- Its milestone (if any) is identified

A **project** is ready to enter `in_progress` when:

- Its workstreams are defined with interface contracts
- At least one milestone is sequenced
- The thinnest M1 proves architecture end-to-end (not just one vertical slice)
- Key risks are named
