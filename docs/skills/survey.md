# survey

**Stance:** receptive. **Purpose:** navigate the work graph, see what's ready, blocked, or stalled, decide what to pick up next.

Survey is home base for the develop plugin. Where [plan](plan.md) and [spec](spec.md) do structural work and [build](build.md) does the inner loop, survey does none of that. It reads the graph and reflects it back so you can decide where to go.

It is the lowest-formalism skill in development. It writes nothing, prescribes nothing. The engineer stays in charge; survey just makes the state of the work legible.

## When to reach for it

- **Re-entry after time away.** You've been out for a week and need to know what moved, what stalled, what needs your attention.
- **Picking the next story.** You finished a story and want to know what's ready to start now, not a dump of the whole backlog.
- **Checking progress against a milestone.** You want to know whether M1 is on track or whether the integration points are slipping.
- **Stale artifact sweeps.** You suspect specs have drifted, spikes have sat past their time-box, or stories have been in-progress with no commits.

Trigger phrases: *where are we*, *what's next*, *what's blocked*, *catch me up*, *state of development*, *what's in flight*, *what's stale*, *what should I work on*.

## How it behaves

Survey reads the development graph under `docs/development/` and presents the one or two things that matter. It does not produce an exhaustive status report. The signal priority is opinionated: stalled in-progress stories come first, then ready stories without a clear approach, then vague ACs, then spikes past their time-box, then stale drafts with stories building against them, then unacknowledged integration contracts.

End state is always a specific recommendation, not a menu. "The most impactful thing you could pick up right now is..." — named, with a reason.

### Re-entry

Ask "catch me up on the OAuth project" and survey reads the project, its workstreams and milestones, the stories under each epic, and the recent changes. It leads with what moved or what's stuck, not a table of everything. If a workstream has slipped behind its M1 commitment, that's the lead. If nothing has slipped, say that too.

### Direction-seeking

Ask "what should I work on?" and survey filters to stories you could actually start now: ready status, no open blockers, clear approach. It surfaces two or three candidates, names why each is a good pick (small, unblocks other work, serves a near-term milestone), and asks which resonates before going deeper.

### Reflection on shape

Ask "are we actually making progress on M1?" and survey pulls back. It looks at the graph as a whole and names the pattern: stories piling up in draft without moving to ready, spikes accumulating without findings, integration points between workstreams slipping. The goal is to help you see the shape, not to score the team.

### Stale sweeps

Ask "what's stale?" and survey scans for the specific staleness signals: stories in-progress with no recent commits, specs still in draft while stories build against them, spikes past their time-box with no outcome, ADRs stuck in proposed, workstream interface contracts not acknowledged by consumers. It surfaces the single most impactful item first, not a full report.

## How to use it well

**Start here after time away.** Survey is cheap. Running it before plan or build gives you the lay of the land, and often surfaces the story you were about to pick up as blocked or the spec you were about to write as already drafted.

**Let the recommendation push you, then push back.** Survey names one thing. If it's not what you want to work on, say so. The conversation is the value, not the verdict.

**Ask it to scope the view.** "Show me only the payments workstream" or "just the M2 stories" narrows the noise. A survey of the whole graph is rarely what you need.

**Use it to notice patterns, not to audit people.** If three stories have been in-progress for two weeks with no commits, that's a signal about capacity or blockers, not a reason to chase individuals. Survey reflects; it does not judge.

## Saving artifacts

Survey does not write artifacts. It reads them. When the conversation crystallizes into work, survey hands off to the skill that writes: plan for scoping, spec for design, build for implementation.

The artifacts survey reads live under `docs/development/`. See [the develop overview](../develop.md#the-artifacts) for the full hierarchy.

## Common failure modes

- **Asking survey to do the work itself.** Survey is not plan, spec, or build. If you want a spec, go to spec. Survey's job ends at "here's what's there; here's what matters."
- **Treating the output as a status report.** Survey leads with what matters, not with everything. If you want a full dump, ask for one explicitly, and expect noise.
- **Skipping it on re-entry.** Diving straight into build after a week away guarantees rework. Five minutes of survey saves an hour of backtracking.
- **Running it on a graph you haven't maintained.** Survey reflects what's written. If no one updates story status or spec state, the reflection is stale. Maintain the graph or the skill has nothing to read.

## Transitions

Survey doesn't do the work itself. When the question crystallizes, it hands off.

- "Let's scope this" → [**plan**](plan.md)
- "We need a technical design" → [**spec**](spec.md)
- "Is this ready to start?" → [**assess**](assess.md)
- "Let me write tests for this" → [**test**](test.md)
- "Let's implement" → [**build**](build.md)
- "I want to reflect on the broader problem" → [**orient**](orient.md) (discovery side)

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and the artifact hierarchy survey reads.
- [orient](orient.md) — the discovery-side counterpart for taking stock before deciding where to go.
