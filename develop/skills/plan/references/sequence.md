# Sequencing

Order work into milestones where each milestone proves something and enables the next.

## M1 is special

M1 should be the **thinnest end-to-end slice that proves the architecture**.

This is the single most misunderstood planning move. The instinct is to put
"easy stuff" or "one small feature" in M1. That produces an M1 that ships but
doesn't derisk the plan. When M2 starts, the hard integration problems surface
— and by then, you're committed.

A good M1 exercises every integration point in the system, even if thinly. It's
allowed to be ugly. It's allowed to have hardcoded values. It just has to go
end-to-end.

**Test for M1 design:** if M1 succeeds, does the team believe the rest of the
plan will work? If not, M1 is proving the wrong thing.

## Demo criteria

Every milestone needs `demo_criteria` — a concrete, observable thing that must
be true when the milestone is "done." Examples:

- ✅ "User can click 'Sign in with Google' and land on the authenticated
  dashboard"
- ✅ "Ingest pipeline processes a 10K-row test file end-to-end in under 2
  minutes"
- ❌ "Authentication works"
- ❌ "Ingest is performant"

If you can't write demo criteria, the milestone isn't scoped yet.

## Milestone types

Use `milestone_type` to categorize:

- **value** — user-visible capability ships
- **integration** — workstreams converge; system integrates
- **foundation** — architectural prerequisite (infra, schema, framework) —
  necessary but not user-visible

A plan with only `value` milestones is suspicious — foundation and integration
work has to happen somewhere, and if it's not called out, it'll surprise the
team.

## Workstream deliverables

For each milestone, name what each workstream delivers. Specifically, not
vaguely.

```yaml
workstream_deliverables:
  - workstream: workstream-backend-auth
    delivers: "Google OAuth endpoint, token issuance"
  - workstream: workstream-frontend-auth
    delivers: "Login button, token storage, auth state"
```

"Backend is ready" is not a deliverable. Make it concrete.

## Integration points

Integration points are where workstreams meet. They're the highest-risk part of
any multi-workstream plan. Surface them in two places:

1. On the workstream: `integration_points` notes which milestones require
   synchronization and why
2. In the pre-mortem: the risks around integration are often the top risks

## Common failures

- **M1 is "all the hard parts."** Rescope.
- **M1 is "the easy path."** The architecture stays untested.
- **Milestones with no demo.** Wish list, not plan.
- **Workstream deliverables as buzzwords.** "High quality," "robust,"
  "scalable" are not deliverables. What *behavior* does the workstream ship?
