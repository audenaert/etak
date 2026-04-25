# assess

**Stance:** evaluative. **Purpose:** readiness, feasibility, sizing, and stress-test — the quality gate between planning and building.

Assess is where work gets examined before the team commits to it. Where [plan](plan.md) shapes scope and [spec](spec.md) grounds design, assess asks whether the resulting artifact is actually buildable, how hard it is, and what will bite mid-implementation. It catches the vague AC, the missing alternative, and the hidden coupling while they're still cheap to fix.

Assess is read-only by default. It observes, surfaces, and suggests. It only modifies artifacts when you explicitly ask it to apply a fix.

## When to reach for it

- **Before pulling stories into an iteration.** A readiness check surfaces the stories that aren't yet buildable, cheaper to catch now than mid-sprint.
- **Before committing to a project.** A feasibility pass grounded in the codebase often reshapes the scope, either shrinking it, expanding it, or surfacing a stepping stone that changes sequencing.
- **Before approving a spec.** A stress-test with two or three personas catches implicit assumptions and AC gaps that would otherwise surface at PR review.
- **Mid-flight refinement.** A story's ACs feel mushy and the implementer has started. A refinement pass tightens the vague terms before more code lands.

Trigger phrases: *is this ready*, *ready check*, *how hard is this*, *feasibility*, *poke holes*, *stress test*, *critique this spec*, *estimate*, *size this*, *refine this*, *is this solid*.

## How it behaves

Assess is specific, not performatively negative. "This is complex" is useless. "The middleware at `src/auth.ts:45` doesn't support multiple providers, and this story implies it will" is useful. It calibrates to maturity: a draft doesn't need production-readiness standards, a ready story does.

Assess has four lenses. You usually want one. Assess asks before running all four.

### Ready

Gate check against a Definition of Ready. Is the work item well-defined enough to build? Assess scans for vague ACs, unresolved dependencies, missing spec anchors, and produces a traffic-light verdict (ready, almost ready, not ready) with a specific list of what's missing. Per-artifact checklists live in assess's internal references for story, epic, and project.

### Feasibility

Grounded in the codebase, not in vibes. Assess reads the files the work touches, names the existing patterns, names what's new ground, and surfaces the complexity hotspots. The output is a map of effort, risk, dependencies, and (when there is one) a stepping stone: a smaller piece of work that's independently valuable and makes the larger work cheaper later.

Feasibility has shell access for investigation: `git log` on a path to gauge churn, directory scans to size a change, build or test dry-runs when cheap. It stays read-only. Feasibility never mutates the repo.

### Size

Relative sizing against a baseline. XS, S, M, L, XL, where XL means "break this up." Assess flags outliers, suggests splits, and notes when a story needs a spike before it can be sized. Spikes never get sized; they carry a time box instead.

### Stress-test

Multi-lens critique for specs and stories; risk-surfacing for projects. Assess adopts personas (architect, QE, on-call, security, new team member) or runs a pre-mortem-style failure surfacing. The output is a consolidated finding list with severities and concrete fixes. Refinement (tightening vague ACs, removing implementation leakage, adding missing edge cases) folds into this lens. When the critique surfaces a fix, assess offers to apply it.

## How to use it well

**Pick the lens.** Running all four on every artifact is overkill. Ready before a sprint, feasibility before a commitment, size when you need a number, stress-test before approval. Assess asks which you want.

**Let feasibility reshape scope.** The most valuable feasibility outputs are the ones that change the plan. A project that looked like a month turns out to be a week because the pattern already exists. A story that looked trivial reveals a dependency nobody saw. Take the output seriously.

**End on an action, not a verdict.** A readiness check that produces "not ready" and nothing else is half the work. Assess offers the follow-ups: rewrite the vague AC, draft the missing spec, kick a spike, promote the artifact to ready.

**Use the "new team member" persona.** It's the sharpest stress-test lens. It catches the implicit knowledge that long-tenured engineers don't notice they're using.

**Don't estimate before reading the code.** A guess dressed up as a number is worse than no number. If the engineer wants sizing and assess hasn't read the files, assess pushes back or runs feasibility first.

## Saving artifacts

Assess is read-only by default. When you accept a suggested fix (rewriting an AC, promoting a story from draft to ready, applying a stress-test finding), assess writes the change through the relevant internal skill. The engineer approves the diff before it lands.

See [the develop overview](../develop.md#the-artifacts) for the artifact types assess evaluates.

## Common failure modes

- **Assessing without reading the code.** Every lens except ready gets weaker. Don't skip the reading.
- **Critique that fills rounds instead of surfacing real issues.** If a round mostly restates earlier concerns, stop. Volume is not depth.
- **Blocking on low-weight findings.** A stylistic nit is not a readiness failure. Name it, don't gate on it.
- **Estimating before reading the code.** The number is a guess. Push back or move to feasibility first.
- **Forcing a stepping stone.** If there isn't one, say so. Manufacturing a split that's really a dependency adds ceremony without value.
- **Stress-test as performance.** Running the personas for the sake of running them. If each round produces nothing new, the value has stopped.

## Transitions

- Ready check returns "not ready" with vague ACs → [**assess**](assess.md) refinement, or [**plan**](plan.md) to rescope
- Feasibility reveals a missing design → [**spec**](spec.md)
- Feasibility reveals an unknown → spike (via assess's internal skills)
- Stress-test surfaces an AC gap → refinement within assess, or back to the story author
- Ready check returns "ready" → [**build**](build.md)
- Test coverage looks thin → [**test**](test.md) to derive cases from ACs

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and artifact hierarchy.
- [plan](plan.md) — the upstream scoping that assess evaluates.
- [spec](spec.md) — the design artifact assess stress-tests before approval.
- [critique](critique.md) — the discovery-side analog, for ideas and opportunities rather than stories and specs.
