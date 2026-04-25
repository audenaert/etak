---
name: work-item
description: >
  Create and update standalone work items — bugs, chores, and enhancements that
  don't require the full project/epic hierarchy. Internal skill called
  by survey and build; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Work Item

You help create and update the three standalone work item types: **bug**, **chore**,
**enhancement**. They share a common shape but differ by lifecycle and framing.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

All three live together in `docs/development/work-items/`. Distinguish them by the
`type` field.

## Bug

A defect — the software does not behave as specified.

**Good shape:**
- **Reproducible** — steps to reproduce, even if intermittent
- **Observable** — expected vs actual, not vague ("it's broken")
- **Severity** — critical, high, medium, low; be honest

**Moves:**
- Capture the report in the user's language first, then structure
- Reproduce or ask for help reproducing; unreproduced bugs are hypotheses
- Note environment: version, platform, data state

**Status lifecycle:** `open → investigating → fix_in_progress → resolved`

```yaml
---
name: "OAuth token refresh fails silently on slow connections"
type: bug
status: open
severity: high
parent: null
---
```

Body: steps to reproduce, expected vs actual, environment.

## Chore

Maintenance work — dependency updates, cleanups, migrations that aren't tied to a
story.

**Good shape:**
- **Scoped** — one clear thing, not "general cleanup"
- **Justified** — why now? Security, deprecation deadline, performance, developer
  pain
- **Risk noted** — what could break

**Status lifecycle:** `todo → in_progress → done`

```yaml
---
name: "Update OAuth dependency to v4.2"
type: chore
status: todo
parent: null
---
```

Body: why this is needed, any risks.

## Enhancement

A small standalone improvement — better UX, minor feature polish, developer
quality-of-life — that doesn't need story-level process.

**Good shape:**
- **Small** — if it's bigger than a story, it *is* a story
- **Low ceremony** — no need for ACs if the change is obvious
- **Value-stated** — what this improves

**Status lifecycle:** `todo → in_progress → done`

```yaml
---
name: "Add loading spinner to login button"
type: enhancement
status: todo
parent: null
---
```

Body: what it improves, why it matters.

## Writing the Artifact

Generate a kebab-case filename. Write to `docs/development/work-items/`. Use the
schema for the appropriate type. Always show before writing.

## Failure Modes

- Bug with no reproduction — it's a hypothesis; say so.
- Enhancement that's really a story — promote it. Run it through the story skill.
- Chore dumping ground — "cleanup sprint" full of unscoped chores. Push for
  specificity.
- "Bug" that's really a feature request — reframe as enhancement.
