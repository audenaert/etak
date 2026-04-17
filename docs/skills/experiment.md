# experiment

**Stance:** rigorous but pragmatic. **Purpose:** turn beliefs into evidence, then propagate the evidence back into the graph.

Experiment is the empirical engine of Etak. Where other skills work with beliefs and judgments, this one works with evidence. It helps you design tests that produce real signal, tracks experiments through their lifecycle, records what actually happened, and — most critically — makes sure the learning flows back into the opportunity graph.

The last part is what most teams miss. Running experiments is valuable. Propagating results is what makes the graph compound over time.

## When to reach for it

- **A high-importance, low-evidence assumption surfaces.** This is the canonical input. Sound produces the assumption; experiment produces the test.
- **Before significant investment.** The cheapest experiment is always cheaper than the feature it would de-risk. If you're about to scope a multi-sprint build, experiment on the riskiest assumption first.
- **When a decision is blocked on evidence.** Stakeholders disagree about whether users want X. Design an experiment. Let the evidence resolve the debate.
- **Results came in and you need to close the loop.** Recording and propagating results is half the skill. Don't skip it.

Trigger phrases: *design an experiment*, *test this assumption*, *how would we test*, *here are the results*, *the experiment showed*, *what happened with our test*.

## The five parts of a real experiment

A well-designed experiment has five parts. Without all five, you are not running an experiment — you are doing stuff and hoping to learn something.

1. **A clear assumption being tested.** Specific, testable, linked to the parent idea.
2. **An appropriate method.** See the method table below.
3. **Success criteria defined in advance.** Specific thresholds, including an invalidation threshold.
4. **An interpretation guide.** How to read edge cases and ambiguous results.
5. **Pre-committed action plans.** What you will do if validated, invalidated, or inconclusive.

Experiment pushes hard on every one. The two it pushes hardest on are success criteria and pre-commitment, because those are the two teams most often fake.

## Method selection

| Method | Best for | Evidence quality | Speed |
|--------|----------|------------------|-------|
| User interview | Desirability, qualitative texture | Moderate | Days |
| Prototype test | Usability, specific interactions | High | Days to a week |
| Fake door | Desirability at scale | High | A week |
| Concierge MVP | Viability, end-to-end feasibility | High | Weeks |
| Data analysis | Feasibility, some viability | Varies | Hours to days |
| A/B test | Causal assumptions with traffic | High | Weeks |
| Survey | Desirability at scale, with limits | Moderate | Days |

Experiment proposes a method based on the assumption type and asks you to confirm. If your context demands a different method — you have the traffic for an A/B but not the time for a prototype round, or vice versa — say so. The tool adapts.

The heuristic the tool follows: *the cheapest method that would settle the question with enough evidence to make the next decision.* Not perfect evidence. Decision-grade evidence.

## How it behaves

### Designing

Experiment walks you through all five parts. It refuses vague success criteria. It drafts action plans and asks you to confirm. It flags when the experiment is larger than needed for the decision at stake, and when it's smaller.

### Running

The `running` state is mostly out of the tool's hands — you go do the work. But experiment will notice experiments that have been running longer than their estimated duration and prompt: "this was planned to take a week, it's been three. Still running?"

### Recording

Enter what actually happened. The tool interprets against the pre-committed criteria. If the result is ambiguous, the pre-committed action for inconclusive applies. If the result contradicts the pre-committed interpretation (e.g., you got 5 of 8 when 6 of 8 was the bar, and you're tempted to call it validation), the tool flags the tension and asks you to confirm. The flag is not a judgment. It is a question.

### Propagating

This is the skill's highest-value activity, and the one most often skipped.

**Update the tested assumption.** Validated, invalidated, inconclusive. The status and evidence level update accordingly.

**Review impact on parent ideas.** A critical assumption invalidated doesn't just change one artifact. It may undermine the idea it supports. The tool surfaces this explicitly: "this assumption was critical to [idea]. Now that it's invalidated, can the idea be adapted, or should it be shelved?"

**Check for shared assumptions.** If the tested assumption appears across multiple ideas, one experiment may have informed several lines of thinking. The tool traces all the connections.

**Suggest next actions.** Don't leave the PM staring at updated statuses. Design a follow-up experiment? Pivot the idea? Brainstorm alternatives? The tool proposes.

## How to use it well

**Run the experiment that matters most, not the one easiest to run.** The temptation is always to design the cheap experiment first. Sometimes it's right — small evidence fast is better than large evidence slow. Sometimes the expensive experiment is the one worth running because it's the one that would change the decision.

**Write success criteria before you run.** The first time a set of results feels "close enough" to validation, remember why you wrote criteria in advance.

**Pre-commit honestly.** An action plan you won't honor is worse than no action plan. If you write "we will shelve the idea if invalidated" and you know in your heart you won't shelve the idea no matter what, write what you actually will do. Maybe "we will run a second experiment with a different method." That's a real pre-commitment.

**Capture learning beyond the headline result.** Experiments almost always produce a secondary observation more valuable than the primary one. Users clicked the button you thought they would, but on the way they asked for something completely different. Capture it. Often the secondary observation becomes the next opportunity.

**Propagate promptly.** Results captured but not propagated are the single most common waste mode in discovery. The tool's signal-priority readout flags these — they appear as "stale experiments" at the top of [orient](orient.md). Don't let them accumulate.

## Common failure modes

- **Pre-commitment theater.** Writing action plans everyone knows won't be followed. Useless. Write what you'll actually do, even if it's less ambitious than what sounds good.
- **Running an experiment to confirm something you already believe.** If you cannot imagine being surprised, don't run it. The experiment is producing social permission, not evidence.
- **Letting inconclusive slide into "sort of validated."** Inconclusive is a valid result with a pre-committed action. Honor it.
- **Running experiments sequentially when they could be parallel.** If two assumptions can be tested independently, run the tests at the same time. The tool can note this.
- **Recording results but not propagating.** The assumption gets updated, but the idea's status doesn't change, the sibling assumptions don't get re-rated, and the shared assumptions across other ideas don't get flagged. Propagation is the step.

## Transitions

Experiments generate learning, and learning opens new paths:

- Invalidation opens new directions → [**explore**](explore.md) ("the assumption failed, but the conversations revealed something — want to brainstorm alternatives?")
- Results reveal new assumptions → [**sound**](sound.md) ("the experiment surfaced a feasibility concern we hadn't considered")
- Multiple experiments complete, time to reassess → [**prioritize**](prioritize.md) ("new data on three assumptions, want to re-rank?")
- Step back and reflect on what you've learned → [**orient**](orient.md) ("let's take stock of what these experiments are telling you")

## Related

- [Skills index](README.md)
- [Experiment artifact](../artifacts/experiment.md) — the structure of the artifact the skill produces.
- [Designing experiments tutorial](../tutorials/designing-experiments.md) — runnable walkthrough.
- [Assumption artifact](../artifacts/assumption.md) — the input to experiment design.
