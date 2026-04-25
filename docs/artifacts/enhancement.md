# Enhancement

**Role:** standalone work item for a small improvement. Better UX, minor feature polish, developer quality-of-life, without needing story-level process.

## Why it exists

Some improvements are small enough that wrapping them in a story would be ceremony. A loading spinner on a button. A clearer empty state. A useful log line. These changes are valuable but they don't need a persona-goal-outcome statement and a set of acceptance criteria. They need a name, a reason, and a PR.

The enhancement artifact exists to hold these without promoting them or demoting them. Not a story (too small, no real persona frame). Not a chore (it changes behavior a user would notice). Not a bug (the software isn't broken, just less good than it could be).

The shape is intentionally low-ceremony. A sentence on what it improves. A sentence on why. Done. If you find yourself writing acceptance criteria for an enhancement, the work probably wants to be a story.

## When to create one

- When the improvement is small enough that a story would be overkill.
- When the change is user-visible but doesn't warrant a persona-goal-outcome frame.
- When UX polish is worth tracking but not large enough to be an epic or story.
- When a developer quality-of-life improvement benefits the team without being maintenance.

## Schema

```yaml
---
name: "Add loading spinner to login button"
type: enhancement
status: todo  # todo | in_progress | done
parent: null
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Concrete description of the improvement. |
| `type` | yes | Always `enhancement`. |
| `status` | yes | Current lifecycle state. |
| `parent` | no | Usually `null`. May reference a related project or epic. |

Enhancements live in `docs/development/work-items/` alongside bugs and chores, distinguished by `type`.

## Status lifecycle

```
todo  ─────▶  in_progress  ─────▶  done
```

- **todo** — ready to pick up.
- **in_progress** — work started.
- **done** — merged and verified.

## Relationships

Enhancements are usually standalone. They may reference a related project or epic via `parent` when the improvement lives near active work.

## Example

```markdown
---
name: "Add loading spinner to login button"
type: enhancement
status: todo
parent: null
---

## What It Improves

After a user clicks "Sign in with Google," there's a brief window before the
Google consent screen loads. Currently the button stays in its default state,
so a user who doesn't see immediate response is tempted to click again. A
spinner on the button (and disabling it on click) makes the loading state
obvious.

## Why It Matters

- Reduces duplicate OAuth initiation requests (minor backend noise)
- Standard UI affordance; its absence reads as sloppiness on a critical page
- Zero risk; isolated to the login button component
```

## Useful actions

### Create

Through [survey](../skills/survey.md) when polish candidates surface, or directly when someone notices an improvement worth tracking.

### Execute via build

[Build](../skills/build.md) picks up an enhancement much like a chore. The work is usually a single PR.

### Promote when needed

If an "enhancement" starts growing (acceptance criteria, multiple steps, design decisions), promote it to a [story](story.md). Don't let ceremony accrete on an artifact type that isn't designed for it.

## Tips

- **Small.** If it's bigger than a story, it *is* a story.
- **Low ceremony.** No persona, no ACs unless the change genuinely needs them.
- **Value stated.** One or two sentences on what this improves.
- **Don't pile them up.** A backlog of forty enhancements is a smell; most of them should either be promoted, rolled into a related story, or dropped.
- **Trivial enhancements don't need artifacts.** Just commit the change.

## Related

- [Story](story.md) — promote to this when the improvement gets large.
- [Chore](chore.md) — for maintenance work that isn't a behavior change.
- [Bug](bug.md) — for defects, not improvements.
- [Survey skill](../skills/survey.md) — surface enhancement candidates.
- [Build skill](../skills/build.md) — execute an enhancement.
- [Artifacts overview](README.md)
