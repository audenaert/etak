---
name: epic
description: >
  Create, update, and refine epics — themed groups of related stories that deliver
  a coherent capability. Internal skill called by plan and survey; never invoked
  directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Epic

You help create, update, and refine epics — themed containers for related stories. An epic is big enough to need breakdown into stories, small enough to complete within a milestone or two.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Epic

- **Themed** — a clear, coherent capability. "OAuth provider integration" is a theme;
  "Auth work" is a bucket.
- **Sized to a milestone or two** — if it spans more than that, consider splitting.
- **Child stories identifiable** — even if not all ready yet, you know roughly what
  stories it contains.
- **Parented to a project** — epics live inside projects, not standalone.

## Moves

### Capture a new epic

- **What capability does this deliver?** Name the outcome the user gets.
- **What stories will it contain?** Rough enumeration — not all ready.
- **Which workstream owns it?** Epics are generally workstream-scoped; cross-workstream
  epics are a smell.
- **Which milestone does it target?** If none, ask whether it's a stretch or
  misscoped.

### Update an existing epic

**Status lifecycle:** `draft → ready → in_progress → complete`

- draft → ready: at least one child story is ready to implement
- ready → in_progress: first story started
- in_progress → complete: all children complete

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/epics/`. Include
frontmatter with `name`, `type: epic`, `status: draft`, `parent` (project),
`children: []`, `workstream`, `milestone`. Body: scope, epic-level ACs, links to
spec if one exists. Always show before writing.

## Failure Modes

- Epic spans multiple workstreams — either split, or retheme around what the
  workstreams truly share.
- Epic is really one story — don't create the middle layer when it adds no structure.
- Epic is really a project — if it has its own milestones and workstreams, promote it.
- No parent project — orphan epic. Either find the project or create one.
