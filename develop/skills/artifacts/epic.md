# Epic

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create, update, and refine epics — themed containers for related stories. An epic is big enough to need breakdown into stories, small enough to complete within a milestone or two.

## Schema

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

Body: scope, acceptance criteria at the epic level.

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

Generate a kebab-case filename and write to the canonical path in the artifacts registry. Include
frontmatter with `name`, `type: epic`, `status: draft`, `parent` (project),
`children: []`, `workstream`, `milestone`. Body: scope, epic-level ACs, links to
spec if one exists. Always show before writing.

## Failure Modes

- Epic spans multiple workstreams — either split, or retheme around what the
  workstreams truly share.
- Epic is really one story — don't create the middle layer when it adds no structure.
- Epic is really a project — if it has its own milestones and workstreams, promote it.
- No parent project — orphan epic. Either find the project or create one.
