# Development Graph Model

Artifacts live in `docs/development/` organized by type. See [schemas.md](schemas.md) for field definitions, status lifecycles, and typed links.

## Work item hierarchy

```
Project → Epic → Story → Task
```

Standalones (no parent required): Bug, Chore, Enhancement, Spike.

## Two frames

**Story** = user frame. "As a \<persona>, I want \<capability>, so that \<outcome>." Describes change from the user's perspective.

**Task** = engineering frame. Describes a technical change that enables or supports a story or epic. Tasks are optional — skip when the story is a single obvious PR. Use tasks for non-user-visible work (migrations, feature flags, telemetry) or cross-story technical changes; parent is the epic for cross-story work, otherwise the story.

## Design artifacts

**Spec** — technical specification. `for` a project, epic, or story.  
**ADR** — architecture decision record. Immutable after `accepted`; write a new ADR to supersede, don't amend.

## Traceability

`from_discovery` links a development artifact to the discovery idea it originated from. One-way: development points back, discovery never points forward. Populate on projects and stories from validated ideas. Omit for bugs, chores, and enhancements from production or engineering judgment.
