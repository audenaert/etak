---
name: milestone
description: >
  Create and update milestones — sequenced project checkpoints that are thin,
  end-to-end, and demo-able. Internal skill called by plan; never invoked directly
  by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Milestone

You help create and update milestones — the checkpoints that sequence a project's
delivery. A good milestone proves something real; it isn't just a date on a
calendar.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Milestone

- **Thin, end-to-end** — especially M1, which should prove the architecture works
  end-to-end, not one vertical slice.
- **Demo-able** — `demo_criteria` should be a concrete, observable thing someone
  can see happen.
- **Cross-workstream** — most milestones are where workstreams converge. If a
  milestone involves one workstream, consider whether it's really an epic.
- **Typed** — value (user-visible capability), integration (workstreams meeting),
  foundation (architectural prerequisite).

## Moves

### Capture a new milestone

- **What does this prove?** Force one sentence.
- **What does it enable?** Downstream work unlocked by hitting this checkpoint.
- **What does it defer?** Explicitly carve out what's *not* in this milestone.
- **What does each workstream deliver for it?** Name the commitments per stream.
- **Demo criteria?** Concrete: "user can X" or "dashboard shows Y" or "request
  from A to B returns in <N ms." Testable, not aspirational.

### Sequence milestones

M1 is special. It's the thinnest slice that proves the architecture — not the
simplest feature. A project that makes M1 "user can log in" but has no plan for
how that login integrates with the rest of the system is going to hit the first
integration milestone and discover the architecture is wrong.

Later milestones add capability. They should build on M1, not replace it.

### Update status

**Status lifecycle:** `planned → in_progress → complete`

- planned → in_progress: first workstream starts delivering against this
  milestone
- in_progress → complete: demo criteria met, all workstream deliverables in

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/milestones/`. Include
frontmatter with `name`, `type: milestone`, `milestone_type`, `project`,
`status: planned`, `target_date` (optional), `workstream_deliverables` (list of
`{workstream, delivers}`), `demo_criteria`. Body: what it proves, what it enables,
what it defers. Always show before writing.

## Failure Modes

- Milestone without demo criteria — not a checkpoint, a date.
- Big-bang M1 ("all the hard parts working") — rescope to thinnest end-to-end.
- Milestones that are really "phases" with lots of stuff packed in — split.
- Workstream deliverables that are aspirational ("high quality," "robust") —
  make them testable.
