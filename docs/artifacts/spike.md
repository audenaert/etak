# Spike

**Role:** time-boxed investigation that produces a finding, a proof of concept, or an ADR.

## Why it exists

A spike exists to reduce uncertainty before committing to a direction. Some questions can't be answered from the code or from experience; they need investigation. A spike makes that investigation explicit, bounded, and ended by a concrete outcome.

The time-box is the point. A spike that takes "as long as it needs" is not a spike; it's drift. When the box is up, you either have your answer, or you have a new understanding of why the question was harder than expected. Both are valuable. Silent extension is the failure mode.

A good spike produces one of three outcomes. A *finding* is a concise written answer to the question. A *proof of concept* is code that demonstrates feasibility (a branch, a PR, a gist). An *ADR* is a hard-to-reverse decision that comes out of the investigation. If the outcome is "we need another spike," the original wasn't scoped tightly enough.

## When to create one

- When a decision is blocked on a question the team can't answer from existing code or experience.
- When evaluating a library, vendor, or architectural approach before committing.
- When feasibility is genuinely unknown (new protocol, new data volume, new integration).
- When a spec has so many open questions that resolving them requires investigation.

## Schema

```yaml
---
name: "Evaluate OAuth libraries for multi-provider support"
type: spike
status: planned  # planned | in_progress | complete
time_box: "2 days"
question: "Which OAuth library best supports Google + GitHub + SAML with minimal custom code?"
decision_criteria: "Supports all three providers, <500 lines of glue code, active maintenance"
outcome: null  # finding | proof_of_concept | adr
result: null
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Descriptive title naming the investigation. |
| `type` | yes | Always `spike`. |
| `status` | yes | Current lifecycle state. |
| `time_box` | yes | Days, not weeks. If you need weeks, it's probably a project. |
| `question` | yes | One concrete question the spike answers. |
| `decision_criteria` | yes | How you'll know the answer is good enough. |
| `outcome` | no | `finding`, `proof_of_concept`, or `adr`. Set on completion. |
| `result` | no | The actual answer, concise. Set on completion. |

## Status lifecycle

```
planned  ─────▶  in_progress  ─────▶  complete
```

- **planned** — scoped but not started.
- **in_progress** — investigation active.
- **complete** — time-box hit or answer found. `outcome` and `result` filled in.

## Relationships

Spikes are generally standalone. They may reference the work item they unblock in the body. If the outcome is `adr`, the ADR it produces lives in `docs/development/adrs/` and is linked from the spike body.

## Example

```markdown
---
name: "Evaluate OAuth libraries for multi-provider support"
type: spike
status: complete
time_box: "2 days"
question: "Which OAuth library best supports Google + GitHub + SAML with minimal custom code?"
decision_criteria: "Supports all three providers, <500 lines of glue code, active maintenance, actively tested against current Node LTS"
outcome: adr
result: "Passport.js with official strategies for Google, GitHub, and SAML. Glue code estimate 300 lines. Produced ADR-001."
---

## Investigation Plan

1. List candidate libraries (passport.js, openid-client, grant, custom on node-fetch)
2. For each: check provider coverage, maintenance activity, and line count of
   a minimal Google login integration
3. For the top two: prototype a Google login flow end-to-end
4. Compare prototypes against decision criteria

## What to Look At

- Provider strategy coverage: Google, GitHub, SAML
- Last release date, open issue count, weekly downloads
- Minimal integration: just the callback and session hand-off, no UI
- Async/await ergonomics with our current codebase

## How to Evaluate

Score each candidate against the decision criteria. Proceed with the highest
scorer unless there's a clear red flag (abandoned, critical unresolved issue).

## Finding

Passport.js wins on provider coverage and ecosystem. Official strategies exist
for all three providers. Glue code in the prototype was 280 lines. openid-client
is cleaner for OIDC specifically but would require custom SAML handling.
Recommendation: passport.js. ADR-001 captures the decision.
```

## Useful actions

### Create

Through [plan](../skills/plan.md) when a decision is blocked, or through [spec](../skills/spec.md) when a draft spec has too many open questions.

### Close with an outcome

When the time-box is up or the answer is clear, set `status: complete`, `outcome`, and `result`. If the outcome is `adr`, write the ADR and link it in the body. If `proof_of_concept`, note where the code lives. If `finding`, write the finding directly into the body so it doesn't evaporate.

### Time-box breach

Don't quietly extend. Three options:

- **Accept partial answer.** Write up what you know; ship a provisional finding.
- **Consciously extend.** Record why; note the new deadline.
- **Abandon.** You learned the problem is different than expected. Record that; it's valuable.

## Tips

- **One question.** If the spike has two questions, it's two spikes.
- **Days, not weeks.** Longer means it's a project.
- **Decision criteria upfront.** Otherwise the investigation has no terminal condition.
- **Outcome-typed.** Finding, POC, or ADR. Not "more questions."
- **Don't spike what you can grep.** If the answer is in the code or in a manual, read it instead.
- **Time-box breach is a signal, not a problem.** Record what you learned about the question's shape.

## Related

- [ADR](adr.md) — common outcome of a spike.
- [Spec](spec.md) — may spawn spikes to close open questions.
- [Task](task.md) — use a task instead when the work is "do X," not "figure out how to X."
- [Plan skill](../skills/plan.md) — scope a spike.
- [Artifacts overview](README.md)
