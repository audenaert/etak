# Pre-mortem

A pre-mortem is the single most cost-effective planning move and the one most
commonly skipped. The frame: **assume the project failed. Why?**

This reframe works because "why will we succeed" invites optimism and
confirmation bias. "Why did we fail" invites real answers.

## Running the session

1. **Set the stage.** "It's six months from now. The project didn't deliver.
   What happened?"
2. **Let the failure modes flow.** Capture everything — silly ones, political
   ones, technical ones, organizational ones. Don't filter.
3. **Cluster.** Group related failures. Themes emerge.
4. **Rank.** Which failure modes are most likely? Which would hurt most? The
   top-right quadrant (likely AND damaging) is where to focus.
5. **Pick 3-5 top risks.** More than that and they all blur together. The
   point is to name the ones that matter, not to make an exhaustive list.

## Per-risk structure

For each top risk, capture in the project body under a **Risks** section:

```markdown
### Risk: Workstreams don't integrate at M2

**Trigger:** Frontend and backend teams diverge on auth token shape mid-project.

**Leading indicator:** Interface contract changes in either workstream without
cross-workstream review.

**Mitigation:** Weekly contract review. Joint prototype of the token handoff in
M1 before M2 work begins.

**If not mitigated:** M2 slips by ~2 weeks while teams resync.
```

This structure forces honesty. If a risk can't be mitigated, you can still
name the cost — that's valuable.

## Categories of risk to probe

If the team is quiet, probe these categories:

- **Integration** — workstreams not meeting when expected
- **Dependencies** — upstream teams, vendors, infra not ready
- **Scope** — commitments quietly expanding mid-project
- **Team** — attrition, burnout, capacity misalignment
- **Architecture** — the plan assumes something the codebase won't support
- **Compliance / Security / Legal** — late-discovered requirements
- **Operational** — what happens when it's in production

The best pre-mortems surface risks in categories the team *didn't* think about.

## When a risk requires investigation

If a risk is real but the team doesn't know how to size it, spawn a spike.
Spikes are how pre-mortems convert into action: "We're not sure if our
database can handle the load" becomes a spike with a time-box and decision
criteria.

## When a risk is a constraint

Sometimes a risk can't be mitigated — regulatory uncertainty, team capacity,
external partner availability. Name it as a **constraint** rather than pretending
there's a mitigation. Honest planning beats performative mitigation.

## Common failures

- **Skipping to mitigations.** Value is in naming failures. Skipping it means
  you'll mitigate the wrong things.
- **Treating the pre-mortem as a brainstorm.** It's not — it's a ranking
  exercise. Brainstorm, then *rank*.
- **Writing the pre-mortem but not acting on it.** The risks should shape the
  plan. If M1 doesn't derisk the top risk, the plan isn't responding to the
  pre-mortem.
