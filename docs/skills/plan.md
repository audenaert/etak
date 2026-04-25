# plan

**Stance:** structural. **Purpose:** scope validated ideas, decompose into workstreams and epics, sequence milestones.

Plan is where fuzzy ambition becomes executable structure. Where [survey](survey.md) reads the graph and [spec](spec.md) grounds the how, plan draws boundaries, names seams, and sequences delivery. It is the tech lead move: thinking out loud with you about scope, decomposition, sequencing, and risk.

A plan that omits any of the four moves is usually wrong. You can skip them on small work. You can't skip them on a project.

## When to reach for it

- **A validated idea needs structure.** Discovery settled what to build. Plan turns it into a project with workstreams, epics, and milestones.
- **A project is too big to hold in one head.** You need to find the natural seams and split the work into streams that can run in parallel.
- **The sequencing is not obvious.** You have the pieces but aren't sure what M1 should prove, or where integration risk lives.
- **The plan looks done and you're suspicious.** A pre-mortem surfaces the risks the plan quietly assumed away.

Trigger phrases: *scope this*, *break this down*, *decompose*, *what are the workstreams*, *sequence the milestones*, *what are the dependencies*, *pre-mortem*, *what could go wrong*, *how should we phase this*.

## How it behaves

Plan keeps two lenses active at once. The **outside-in** lens asks what this delivers to the user or business. The **inside-out** lens asks what the codebase tells us about actual cost. A plan made from only one lens is wrong: outside-in alone produces wishful sequencing, inside-out alone produces engineering-driven scope.

Plan runs four distinct moves. You usually want one. Plan asks before assuming.

### Scope

Turning an idea or ask into a project or epic. Plan reads the discovery link if there is one, draws the boundary (what's in, what's explicitly out), sizes the work against the artifact types (project, epic, story), and names the constraints. Non-goals are as valuable as goals.

If the engineer says "this is a project" and the shape is really an epic, plan pushes back. Wrong artifact type cascades into the rest of the hierarchy.

### Decompose

Breaking a project into parallel workstreams and epics. Plan looks for natural seams (real system boundaries, not org chart) and writes interface contracts for each. "We'll figure it out" is where parallel plans die. Each workstream names what it exposes to others: API shape, data contract, protocol. Each gets one owner, even if deferred to M1.

### Sequence

Ordering work into milestones. M1 is the thinnest end-to-end slice that proves the architecture, not the easiest feature. If M1 is "the easy stuff," the plan hasn't been sequenced. Every milestone gets demo criteria (a milestone without a demo is a date) and per-workstream deliverables. Integration points between streams get named explicitly because they carry the risk.

### Pre-mortem

Stress-testing before commitment. Plan reframes from "will we succeed" to "assume this failed, why?" The honest answers show up. Three to five ranked risks is right; more than that and they blur. For each top risk, plan names a trigger, a leading indicator, and a mitigation. Risks that can't be mitigated become constraints. That's honest planning, not pessimism.

## How to use it well

**Bring the discovery link.** If the idea came through discovery, plan reads the idea artifact first and sets `from_discovery` on the project. That traceability matters when compliance auditors ask why a change was made.

**Decompose by system, not by team.** A contrived workstream (one shaped by who happens to be available, not by real boundaries) makes coordination harder, not easier. If the decomposition feels forced, it is.

**Make M1 thin and real.** If your M1 is "all the hard parts working," it's a wish. Rework until M1 is a slice you could demo and the rest is filling in.

**Run the pre-mortem before you think you need to.** The value is in naming failure modes, not in skipping to mitigations. Sit with the failure for a minute. The risk that surfaces is often the one you'd have learned about in month three.

**Don't estimate in plan.** Sizing and effort live in [assess](assess.md), which reads the codebase. A plan that estimates without grounding is guessing.

## Saving artifacts

Plan writes artifacts through internal skills: project, epic, story, workstream, milestone, and spike. The engineer sees the draft before anything lands. Plan always captures the pre-mortem in the project or milestone body under a Risks section, and spawns a spike when a risk warrants investigation.

See [the develop overview](../develop.md#the-artifacts) for the full artifact hierarchy.

## Common failure modes

- **Planning without reading the codebase.** The inside-out lens goes missing. Feasibility, effort, and sequencing disconnect from reality. Plan pairs with [assess](assess.md) to keep this honest.
- **Workstreams shaped by org chart.** Coordination cost goes up. The workstream should reflect a real system boundary, not a team structure.
- **Big-bang M1.** "All the hard parts working at once" is not a milestone. Slice thinner until M1 is a demoable end-to-end path.
- **Pre-mortem as performance.** Skipping from "what could go wrong" straight to mitigations without sitting with the failure. The value is the naming.
- **Estimating in plan.** If a number shows up in a plan artifact, plan has overstepped. Effort belongs in assess.

## Transitions

- Plan is drafted and you want to ground it in the code → [**spec**](spec.md) ("let's design the risky piece")
- Plan is drafted and you want to stress-test it → [**assess**](assess.md) ("is this ready to commit to?")
- A workstream's first epic is ready to build → [**build**](build.md) ("let's start the thinnest story")
- A risk surfaced that needs investigation → spike (via plan's internal skills)
- You want to reflect before committing → [**survey**](survey.md) ("where does this sit relative to everything else?")

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and artifact hierarchy.
- [spec](spec.md) — where the technical design grounds plan's shape.
- [assess](assess.md) — where sizing and feasibility happen against the codebase.
