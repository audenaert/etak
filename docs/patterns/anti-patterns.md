# Anti-patterns

Five habits that erode the value of the graph. Each is easy to fall into because each feels productive in the moment. Each is named here because the costs accrue slowly and are hard to see until the graph has already lost its usefulness.

If one of these rings a bell, the remedy is usually small. If two or three do, it is worth sitting with why. They tend to share a root cause, which is treating the graph as documentation rather than as a model of the team's current understanding.

## Keeping everything

The symptom: a brainstorming session produces twenty ideas and you save all twenty. Six weeks later, nobody remembers which four were actually interesting. The opportunity has sixteen nodes of clutter around the four that mattered.

The underlying error is treating every generated artifact as valuable because it exists. Explore is biased toward abundance on purpose. It wants to put twenty options in front of you so you can see the shape of the space. The four you keep are the output. The sixteen you discard taught you what the space looks like and then got out of the way.

The fix: during generation, save the ideas worth developing and discard the rest in the same session. If an idea feels interesting but not yet worth a node, write it in a notebook. A half-formed thought in your notebook is more retrievable than a half-formed idea lost in a pile of eighteen siblings.

## Generating without constraint

The symptom: you ask the tool to brainstorm ideas for the engagement objective. It produces twelve ideas, all of which are plausible, none of which are the one you needed. You feel like the tool failed you.

The tool did not fail. You gave it nothing to push against. "Brainstorm ideas for engagement" is not a request, it is a weather report. The tool produces options at the level of specificity you gave it, and at that level, everything is plausible and nothing is sharp.

The fix: give the tool real constraint. An opportunity rather than an objective. A user segment rather than all users. A time window, a cost ceiling, a technical boundary. The constraint is what makes the generation useful. Explore reads what you give it and produces against it. Give it more.

## Rubber-stamping agent output

The symptom: the architect agent produces a spec. You skim the briefing, see that it is coherent and well-structured, and approve it. Two weeks into implementation, someone surfaces a load-bearing decision that was wrong. The decision was in the spec. You did not read that section carefully.

An agent briefing is a candidate, not a finished artifact. The autonomy that makes agents valuable also concentrates risk: a whole cascade of decisions gets made without your attention, and your review is the only place those decisions get human judgment applied. If the review is shallow, the judgment never happens.

The fix: when an agent briefing lands, read the open questions first, read the alternatives section second, challenge at least one citation before approving, and ask the agent what it felt least sure about. A briefing that survives all four is probably sound. A briefing that falls apart on one of them was not ready to ship. See [skill or agent](agent-vs-skill.md) for the longer treatment.

## Treating the graph as a roadmap

The symptom: the ideas on the graph become a feature list. The priority scores become a delivery sequence. The team starts building ideas in ranked order without running the experiments underneath. Six months in, the graph has shipped but nobody learned anything, and the outcomes did not move.

The graph is a model of the team's current understanding. A roadmap is a commitment to sequence. Treating them as the same thing converts hypotheses into commitments before the hypotheses have been tested. The pre-commitment step that experiments require (*if this test comes back invalidated, we will shelve the idea*) disappears, because the idea is already on the delivery calendar.

The fix: maintain the separation. Validated ideas are candidates for scoping into development. The sequencing decision lives in develop, with its own considerations: team capacity, technical dependency, strategic fit. The discovery graph is not a queue. When a stakeholder asks "what are we building next?", the answer comes from the development graph, grounded in the discovery graph through `from_discovery` links. Not from the ranked list of ideas in discovery.

## Skipping pre-commitment on experiments

The symptom: an experiment runs. Results come in. The team holds a meeting to discuss what the results mean. The discussion is a negotiation. The stakeholder who proposed the idea argues the results are inconclusive. The PM who proposed the test argues they are invalidating. Nobody wrote down, before the test ran, what outcome would have produced what action. The experiment produced nothing the team can act on.

Pre-commitment is the most important feature of the experiment skill and the most frequently abandoned. It is abandoned because it is uncomfortable. Writing down "if the signup conversion stays below 3%, we shelve the idea" before the test runs is a real commitment, and real commitments are harder to make than soft intentions.

The fix: do not skip the step. When the tool asks you to pre-commit to action plans for validated, invalidated, and inconclusive outcomes, answer the question. If you cannot answer it, that is a signal. You do not yet know what the test is for. Redesign the test until the pre-commitment is possible. An experiment you cannot pre-commit on is an experiment that will not change your mind, and an experiment that will not change your mind is not worth running.

## What these have in common

The through-line: each anti-pattern is the team acting as if the graph is a pile of documents. Keeping everything, generating without constraint, rubber-stamping, treating the graph as a roadmap, skipping pre-commitment. All five describe a team using the tool without applying the discipline the tool is built to support.

The graph is not the value. The thinking that produces the graph is the value. When the habits drift toward accumulation, meaning more ideas, more artifacts, more output, the graph grows and the thinking shrinks. The patterns in this directory are, collectively, about keeping that from happening.

## Related

- [Graph hygiene](graph-hygiene.md): the remedy for keeping everything.
- [Pushing back on the tool](pushing-back.md): the remedy for rubber-stamping.
- [Skill or agent](agent-vs-skill.md): the remedy for dispatching without readiness-checking.
- [Experiment skill](../skills/experiment.md): the pre-commitment step lives here.
