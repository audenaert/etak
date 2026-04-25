# tech-lead

**Type:** autonomous agent. **Purpose:** orchestrate delivery of a project, epic, or milestone.

The interactive [develop skills](../skills/) help an engineer work one move at a time. tech-lead operates at a different altitude. Hand it a project and it fills the gaps (missing specs, sketchy ACs, unresolved spikes), creates an execution plan, and dispatches the other agents to carry out the work. You review the plan before any agent runs, then check in at the cadence you choose.

## When to reach for it

- **Project or epic kickoff.** The work is scoped but not sequenced. You want a concrete plan and parallelized dispatch rather than driving each story yourself.
- **Coordinating across workstreams.** Multiple tracks can proceed in parallel if someone keeps the interface contracts straight. tech-lead is that someone.
- **Mid-project recovery.** The backlog has drifted. Stories are half-ready, specs are missing, statuses are stale. tech-lead reconciles the graph with reality and proposes next steps.
- **You want to step back.** You have other work. tech-lead runs at milestone cadence and reports when M1 is done.

## What it does

### Understands the scope

Loads the project or epic with its goals, constraints, and existing breakdown. Reads the milestone plan, specs, ADRs, and discovery context. Checks the current state of every item: draft, ready, in-progress, complete, blocked. Notes which specs exist, which stories have testable ACs, which spikes are still unresolved.

### Fills gaps autonomously

If structure is missing, tech-lead creates it and notes what it did. No breakdown becomes a `/plan decompose` call. No milestone plan becomes a `/plan sequence` call. Stories without ACs get initial drafts flagged for review. Complex work without a spec gets an [architect](architect.md) dispatch. Dependencies get mapped.

### Creates an execution plan

A concrete plan that names the scope (project, epic, or milestone), the operating cadence, the pre-work needed (architect dispatches, spikes), the items ready for implementation, what's blocked and why, what's deferred, which tracks can run in parallel with what interface contracts between them, and the top risks.

### Negotiates scope before dispatching

Presents the plan and asks two questions. Does this look right? How would you like to operate: milestone cadence, story cadence, or full auto? If the scope is large, offers to narrow. "This is 20 stories across 4 workstreams. Want me to focus on M1 first?"

### Dispatches agents and parallelizes

Pre-work first: [architect](architect.md) for missing specs, [developer](developer.md) in investigation mode for blocking spikes. Then readiness checks, then implementation dispatches. Independent work runs simultaneously across workstreams. Dependent work is sequenced behind its blockers.

### Checks in at cadence

Reports what completed since the last check-in, what's in progress, what's blocked, what's next, and what decisions need human judgment. Asks whether to proceed with the next batch.

### Wraps up

Delivery summary: stories implemented, PRs created, specs and ADRs produced, spikes resolved, quality status, open follow-ups.

## What it doesn't do

- **Write code, specs, or tests itself.** If it catches itself implementing, it stops and dispatches a developer.
- **Dispatch without approval.** The plan goes to you first. Agents don't run until you say go.
- **Make product decisions.** Ambiguous requirements and tradeoffs go back to you rather than getting papered over.
- **Silently re-scope.** If the plan is wrong, it pauses and re-plans rather than executing a broken one.
- **Replace [`/plan`](../skills/plan.md) or [`/survey`](../skills/survey.md).** Those are interactive. Use tech-lead when you want coordinated autonomous delivery instead.

## How it operates

- **Plan concretely, present before acting.** Never starts agent work without you seeing the plan.
- **Parallelize when work is genuinely independent.** Push back on false parallelism: fake seams produce coordination chaos.
- **Escalate early.** Ambiguous requirements, unresolvable merge conflicts, spec gaps, poor agent output go back to you immediately.
- **Track state honestly.** Work item statuses update as agents complete. The graph always reflects reality.
- **Respect the trust gradient.** First use gets story cadence. Trust builds, cadence widens. Full auto is earned.
- **Shift-left responsibility.** QE and security reviews belong to the planned `etak-deliver` plugin. Until then, tech-lead notes where specialist review is warranted and what risk is accepted by deferring.

## How to invoke

Ask in plain language. Examples:

```
Lead the notification system project.
```

```
Coordinate delivery of the auth migration epic.
Operate at milestone cadence.
```

```
Run M2 of the search rewrite. Full auto if the plan looks
clean, otherwise check in after the architect pass.
```

```
The checkout project has drifted. Reconcile the backlog
and propose next steps.
```

The agent loads the work, fills gaps, and presents a plan for your approval before dispatching anything.

## Tips

- **Start with narrower scope.** First time using the agent, point it at a single epic or milestone rather than the whole project. Scope-narrowing is the sharpest move it offers; take advantage of it.
- **Review the pre-work section first.** Missing specs and unresolved spikes are where projects stall. tech-lead flags them up top.
- **Pick the cadence honestly.** Story cadence is fine. Milestone cadence requires trusting the agent to keep going without you. Full auto is the end of the gradient, not the default.
- **Read the parallel tracks.** If two workstreams have an interface contract, that contract is where integration bugs live. Stress-test it before dispatching both in parallel.
- **Let it re-plan.** Execution reveals things planning missed. If tech-lead pauses and asks to re-plan, that's the agent working correctly.

## Related

- [Develop overview](../develop.md) — where tech-lead fits in the plugin.
- [architect](architect.md) — dispatched by tech-lead for epics that need technical design.
- [developer](developer.md) — dispatched by tech-lead for ready stories, bugs, and spikes.
- [`/plan`](../skills/plan.md) — the interactive alternative when you want to drive breakdown and sequencing yourself.
- [`/survey`](../skills/survey.md) — read the work graph interactively; tech-lead automates the same view.
- [`/assess`](../skills/assess.md) — readiness checks tech-lead runs before dispatching.
