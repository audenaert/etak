# Scoping

Scoping turns a fuzzy ask into a bounded artifact — usually a project, sometimes
a standalone epic.

## Reading discovery when present

If the work originates from a discovery idea:

1. Read `docs/discovery/ideas/<slug>.md`.
2. Check the idea's status. `validated` or `ready_for_build` means assumptions
   have been tested — don't reopen them unless you have new information.
3. Read the assumptions marked `validated` and `invalidated`. What's been
   learned?
4. Note the opportunity and objective the idea ladders up to. That context
   shapes the project's success criteria.

When you create the project, set `from_discovery: <idea-slug>`.

## Drawing the boundary

The hardest scoping move is naming what's **out**. Explicit non-goals prevent
scope creep later.

For each candidate area of work, ask:

- Is this necessary for the stated outcome?
- Is this adjacent but separable (likely non-goal)?
- Is this prerequisite (might need its own project)?

A non-goal section is worth writing. "This project does not cover X, Y, or Z
— those are planned for a separate effort."

## Size check

Match the artifact type to the scope:

| If the work spans... | The artifact is a... |
|----------------------|----------------------|
| Weeks to a quarter, one team | Project |
| Days to a couple of weeks, one developer | Epic |
| A single PR's worth of user value | Story |
| A single technical change | Task |
| A time-boxed question | Spike |

If the scope feels off, push back on the artifact choice. "This feels too big for
one project — should we split it into two with an explicit dependency?"

## Constraints and dependencies

Name these explicitly in the artifact body:

- **Deadlines** — real ones (compliance, external partner) vs. aspirational
- **Team capacity** — who's doing this, at what load
- **Upstream dependencies** — projects, vendors, infrastructure that must land
  first
- **Downstream consumers** — teams or products waiting on this
- **Compliance requirements** — anything that affects traceability, audit, or
  approval workflow

## Common failures

- **Scoping without the codebase.** The ask sounds simple until you look at
  what touching it requires. At minimum, grep for the subsystems involved.
- **"Flexibility" as a non-goal.** "We can't commit to X" is often a way of
  avoiding the hard conversation. Say yes or no.
- **Scope that doesn't ladder to an outcome.** If the project can deliver its
  entire scope without the user noticing, the scope is wrong.
