# Epic

**Role:** themed group of related stories that delivers a coherent capability. Sits between project and story.

## Why it exists

An epic is the middle layer of the containment hierarchy. It names a theme (a capability you can describe in a sentence) and holds the stories that build it. "OAuth2 provider integration" is an epic; the stories under it cover each provider flow, the callback handler, the error paths, the session hand-off.

The epic earns its place when stories are obviously related and ship together. It gives those stories a shared home, a shared workstream, and usually a shared milestone. Without it, you have a flat list of stories that each pretend to be independent when they aren't.

Don't create an epic for every story cluster. One-story epics are noise. Multi-workstream epics are a smell; the boundary is probably wrong. When the theme is tight and the stories naturally go together, the epic is useful; otherwise, let the project hold the stories directly.

## When to create one

- When multiple related stories form a coherent capability you can describe in one sentence.
- When decomposing a project and you notice natural story clusters along a single workstream.
- When one milestone is driven by a single theme of work that deserves a container.
- When the stories under it will realistically complete within one or two milestones.

## Schema

```yaml
---
name: "OAuth2 provider integration"
type: epic
status: draft  # draft | ready | in_progress | complete
parent: project-oauth2-migration
children:
  - story-user-can-sign-in-with-google
  - story-user-can-sign-in-with-github
workstream: workstream-backend-auth
milestone: milestone-m2-multi-provider
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short, thematic title for the capability. |
| `type` | yes | Always `epic`. |
| `status` | yes | Current lifecycle state. |
| `parent` | yes | Slug of the project this epic belongs to. |
| `children` | yes | Slugs of stories inside the epic. |
| `workstream` | yes | Slug of the workstream that owns the epic. |
| `milestone` | no | Slug of the milestone this epic targets, if any. |
| `from_discovery` | no | Slug of the validated discovery idea, if the epic originated there. |

## Status lifecycle

```
draft  ─────▶  ready  ─────▶  in_progress  ─────▶  complete
```

- **draft** — captured but not yet implementable. Stories are rough or missing.
- **ready** — at least one child story is ready to implement.
- **in_progress** — first story started.
- **complete** — all child stories complete.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `parent` | [Project](project.md) |
| Outgoing | `workstream` | [Workstream](workstream.md) |
| Outgoing | `milestone` | [Milestone](milestone.md) |
| Outgoing | `from_discovery` | [Idea](idea.md) in `docs/discovery/` |
| Incoming | Stories link via `parent` | this epic |
| Incoming | Specs link via `for` | this epic |
| Incoming | Cross-story tasks link via `parent` | this epic |

## Example

```markdown
---
name: "OAuth2 provider integration"
type: epic
status: ready
parent: project-oauth2-migration
children:
  - story-user-can-sign-in-with-google
  - story-user-can-sign-in-with-github
  - story-user-sees-clear-error-on-oauth-failure
workstream: workstream-backend-auth
milestone: milestone-m2-multi-provider
---

## Scope

The user-facing sign-in flows for each supported OAuth provider, including the
callback handler, session issuance, and error paths. Provider selection UI and
enterprise SSO are out of scope.

## Acceptance Criteria

- A user can complete sign-in with any supported provider and land authenticated
- Cancellation and failure paths return the user to the sign-in screen with a
  clear message
- No password-based sign-in code path is reachable from the UI after this epic
  completes

## Notes

The token management epic (sibling) produces the session abstraction consumed by
these stories. Integration happens at milestone M2.
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when a project's stories start to cluster by theme. Don't force the layer if the clustering is weak.

### Identify stories

Within the epic, sketch the set of stories it contains. Use [plan](../skills/plan.md) to enumerate candidates, then refine each through the [story](story.md) artifact.

### Assess readiness

```
Is this epic ready?
```

[Assess](../skills/assess.md) checks that stories are identified, workstream is set, and the target milestone is clear.

### Survey

[Survey](../skills/survey.md) shows which child stories are ready, blocked, or in progress, and summarizes the epic's state.

## Tips

- **Thematic, not bucket-shaped.** "OAuth provider integration" is a theme; "Auth work" is a bucket.
- **One workstream.** Cross-workstream epics usually signal the boundary is wrong. Split or retheme.
- **Don't make one-story epics.** If there's only one story, let the project own it directly.
- **Milestone-sized or smaller.** An epic that spans more than two milestones wants to be a project.
- **Epic ACs, not story ACs.** Epic-level criteria describe the capability the epic delivers; story-level ACs stay on the stories.

## Related

- [Project](project.md) — container the epic lives inside.
- [Story](story.md) — user-frame child of an epic.
- [Workstream](workstream.md) — the delivery track that owns the epic.
- [Milestone](milestone.md) — checkpoint the epic targets.
- [Plan skill](../skills/plan.md) — decomposes projects into epics.
- [Artifacts overview](README.md)
