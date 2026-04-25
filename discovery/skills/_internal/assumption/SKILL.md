---
name: assumption
description: >
  Create and update assumptions — testable beliefs underlying ideas. Handles creation,
  importance/evidence rating, and status updates. Internal skill called by sound,
  critique, and experiment.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Assumption

You help create and update assumptions — testable beliefs that underlie ideas. An
assumption is something that must be true for an idea to work. Making assumptions
explicit is one of the most valuable things the discovery system does — it transforms
invisible bets into testable hypotheses.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Assumption

- **Stated as a testable belief** — not a vague risk. "Users might not like it" is vague.
  "Users will complete onboarding without external help" is testable.
- **Specific enough to design an experiment for** — if you can't imagine testing it,
  it's too abstract.
- **Categorized** — desirability, usability, feasibility, viability, or causal.
- **Rated** — importance (if wrong, does the idea fail?) and evidence (how much do we
  actually know?).

## Moves

### Create from sound or critique

When called from sound or critique, you'll receive one or more assumptions to create.
For each:
- Refine the statement into a testable belief
- Assign category (desirability, usability, feasibility, viability, causal)
- Discuss importance with the PM — "If this is wrong, what happens to the idea?"
- Discuss evidence — "What do you already know about this? What's your confidence based on?"
- Set initial importance/evidence ratings (high/medium/low)

When batch-creating, ask: "I've identified N assumptions. Want me to create artifacts
for all of them, or just the high-priority ones?"

### Promote a critique finding

When called from critique to promote a risk finding, you receive the finding text,
the originating critique slug, and the finding's name within that critique. Create
the assumption artifact normally and include a body note recording its origin —
the critique slug and the finding's name. The frontmatter follows the standard
assumption schema; provenance lives in the body, not as a new field.

The forward link from the finding to this assumption (recorded in the finding's
`resolution` field by the critique skill) is the authoritative file-side
representation of the `BECAME_ASSUMPTION` edge in the graph.

Decline if the calling skill tries to promote a strength finding — strengths are not
testable beliefs.

### Update status

Status lifecycle: `untested → validated → invalidated`

When updating from experiment results:
- Set new status based on experiment outcome
- Review impact on parent ideas (via `assumed_by` links)
- If invalidated: what does this mean for the idea? Can it be adapted, or should it
  be shelved? Are there alternative approaches?
- If validated: what's the next riskiest assumption? Is the idea ready to advance?
- Check for shared assumptions — this result may inform other ideas too

### Update ratings

Importance and evidence ratings change as you learn more. Evidence especially shifts
when experiments complete, when new customer data arrives, or when market conditions
change.

### Write the artifact

Generate a kebab-case filename. Write to `docs/discovery/assumptions/`. Include
frontmatter with `name`, `type: assumption`, `status: untested`, `importance`,
`evidence`, `assumed_by` link(s), and body with description, why it matters, and
what evidence exists. Always show before writing.

## Failure Modes

- Assumption is actually a risk ("something bad might happen") — reframe as a belief
  that could be true or false
- Assumption is untestable ("the market will shift") — make it more specific and
  time-bounded
- All assumptions rated high importance — push back. If everything is critical, nothing
  is. Which 3 would you test if you could only test 3?
- Assumption is shared across ideas but only linked to one — check for other ideas
  that implicitly depend on the same belief
