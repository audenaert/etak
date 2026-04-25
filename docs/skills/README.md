# Skills reference

Etak ships two plugins today: `etak-discovery` and `etak-develop`. Each exposes six user-facing skills. You do not invoke them by name. You describe the move you want to make, and Etak picks the right one. The purpose of this reference is to make the menu legible so you recognize which move fits which moment in your own work.

## Discovery

For plugin-level context, see [the discovery overview](../discovery.md).

| Skill | Stance | The move it supports |
|-------|--------|----------------------|
| [**orient**](orient.md) | Receptive | Think out loud, find your bearings, decide where to go next |
| [**explore**](explore.md) | Generative | See the space, notice gaps, brainstorm possibilities |
| [**sound**](sound.md) | Exploratory | Surface the beliefs an idea is resting on |
| [**critique**](critique.md) | Adversarial | Stress-test an idea from outside perspectives |
| [**prioritize**](prioritize.md) | Evaluative | Converge on what matters most, given your constraints |
| [**experiment**](experiment.md) | Rigorous | Design tests, record results, propagate learning |

### The discovery rhythm

The skills are not a linear process. They form a rhythm you move through repeatedly, often in a single conversation.

**Explore → Sound → Critique → Prioritize → Experiment → back to Explore.**

You diverge (explore), examine the foundations (sound), stress-test from outside (critique), converge (prioritize), and test (experiment). Results feed the next round of generation. The rhythm applies at every level: opportunities, ideas, assumptions. **Orient** is the home base, the place you return to when you need to take stock before deciding which move is next.

### How to choose a discovery move

Match the skill to the state of your thinking.

- **You have a signal but no structure** → orient
- **You have structure but need more options** → explore
- **You have an idea that looks solid and you want to know what it's actually betting on** → sound
- **You have an idea and want to know what you're missing** → critique
- **You have too many options and finite time** → prioritize
- **You have a belief that would be cheaper to test than to build against** → experiment

## Develop

For plugin-level context, see [the develop overview](../develop.md).

| Skill | Stance | The move it supports |
|-------|--------|----------------------|
| [**survey**](survey.md) | Receptive | Home base. Navigate the work graph, see what's ready, blocked, or stalled |
| [**plan**](plan.md) | Structural | Scope validated ideas, decompose into workstreams and epics, sequence milestones |
| [**spec**](spec.md) | Pragmatic | Grounded technical design. Specs and ADRs against the actual codebase |
| [**assess**](assess.md) | Evaluative | Readiness, feasibility, sizing, critique before committing to build |
| [**test**](test.md) | Rigorous | Strategy-first testing. Plan from ACs, write at the right layer, debug flakes |
| [**build**](build.md) | Collaborative | The inner loop end to end. TDD pairing, self-review, PR, feedback |

### The develop rhythm

Development is not a linear waterfall. The skills form a loop you return to at every scale.

**Plan → spec → assess → build (with test) → back to plan for the next slice.**

You scope and sequence (plan), ground the design (spec), stress-test before committing (assess), and implement with care (build with test). When one slice lands, you look at the graph again (survey) and pick the next move. **Survey** is home base, the place you return to when you need to see the state of the work before choosing what's next.

### How to choose a develop move

Match the skill to the state of the work.

- **You want to know what's ready to build and what's blocked** → survey
- **You have a validated idea that needs structure** → plan
- **You have a story and need to decide how to build it** → spec
- **You have a story or spec and want to know if it holds up** → assess
- **You're about to write tests** → test
- **You're about to write code** → build

You rarely need to think this through explicitly. Describe what you want in plain language and the tool will route.

## Related reference

- [**Discovery overview**](../discovery.md) — the plugin-level context for the discovery skills.
- [**Develop overview**](../develop.md) — the plugin-level context for the develop skills, including the artifact hierarchy and agents.
- [**Artifacts reference**](../artifacts/) — the things the discovery skills produce.
- [**Product-researcher agent**](../agents/product-researcher.md) — autonomous research that feeds the interactive skills.
- [**Foundations**](../context/foundations.md) — why the skills are arranged this way.
