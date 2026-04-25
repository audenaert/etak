# Chore

**Role:** standalone work item for maintenance. Dependency updates, cleanups, migrations that aren't tied to a story.

## Why it exists

Chores are the honest name for work that has to happen but isn't user-facing. Updating a library, cleaning up dead code, renaming something pervasively, running a migration that unlocks no story directly. This work is real; it just doesn't fit the story frame, and forcing it into one produces thin, invented personas.

The chore artifact gives this work a visible home. It keeps maintenance from disappearing into unlabelled PRs. It also keeps stories honest: when an engineer proposes "implement X" as a story with no user, the alternative is a chore or a task, not a fake persona.

A good chore is scoped and justified. "General cleanup" is a dumping ground, not a chore. "Update OAuth dependency to v4.2" is a chore. The justification matters too: security patch, deprecation deadline, performance issue, developer pain. If there's no justification, the chore isn't urgent, and it can probably be deferred or absorbed into related work.

## When to create one

- When a dependency needs updating and no user-facing story drives it.
- When a cleanup or refactor is worth doing but isn't tied to a feature.
- When a migration, schema change, or internal reorg is needed on its own schedule.
- When tooling work (CI, build, linters) needs a home outside a story.

## Schema

```yaml
---
name: "Update OAuth dependency to v4.2"
type: chore
status: todo  # todo | in_progress | done
parent: null
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Concrete description of the maintenance work. |
| `type` | yes | Always `chore`. |
| `status` | yes | Current lifecycle state. |
| `parent` | no | Usually `null`. May reference a related project or epic. |

Chores live in `docs/development/work-items/` alongside bugs and enhancements, distinguished by `type`.

## Status lifecycle

```
todo  ─────▶  in_progress  ─────▶  done
```

- **todo** — ready to pick up.
- **in_progress** — work started.
- **done** — merged and verified.

## Relationships

Chores are usually standalone. They may reference a related project or epic via `parent` when the maintenance is adjacent to active work.

## Example

```markdown
---
name: "Update OAuth dependency to v4.2"
type: chore
status: todo
parent: null
---

## Why

The current version (v3.9) has a published advisory for token replay in
certain misconfigurations. We are not affected by the specific misconfiguration,
but staying on an advisory-bearing version is a defensible-risk cost we don't
want to carry.

## Scope

- Bump `passport-oauth2` from 3.9.2 to 4.2.1 in `package.json`
- Update one deprecated call site in `src/auth/session.ts` (API change around
  `serializeUser` signature)
- Run the auth test suite; add one new test for the deprecated-call replacement

## Risks

- Breaking API change in `serializeUser` signature; one call site to update
- No production rollout complexity; library is consumed only by the server
- Rollback is a revert of the two-file change
```

## Useful actions

### Create

Through [survey](../skills/survey.md) when maintenance needs surfacing, or directly when an engineer identifies work that needs doing.

### Execute via build

[Build](../skills/build.md) or the developer agent picks up a chore much like a story, minus the user-frame. Scope, change, test, merge.

### Group thoughtfully

Some chores cluster (dependency updates across a package family). Grouping can make sense; a "cleanup sprint" with twelve unscoped chores does not. Prefer specific chores.

## Tips

- **Scoped, not a dumping ground.** "General cleanup" is not a chore. "Remove unused imports from `src/auth/*`" is.
- **Justified.** Why now? Security, deprecation, performance, developer pain. If there's no reason, defer.
- **Risks named.** Even for minor chores. "Rollback is a revert" is a valid note.
- **Don't promote trivial chores.** A one-line README fix doesn't need an artifact; just commit it.
- **Don't demote real work.** If the "chore" spans a week and touches half the codebase, it's a project.

## Related

- [Bug](bug.md) — for defects, not maintenance.
- [Enhancement](enhancement.md) — for small improvements to behavior, not internals.
- [Task](task.md) — use this when the maintenance directly enables a story.
- [Survey skill](../skills/survey.md) — surface maintenance work.
- [Build skill](../skills/build.md) — execute a chore.
- [Artifacts overview](README.md)
