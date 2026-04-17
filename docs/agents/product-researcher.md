# product-researcher

**Type:** autonomous agent. **Purpose:** bring external evidence into the discovery graph.

The interactive [skills](../skills/) help you think clearly about what you already know. product-researcher adds what you don't know. It runs autonomously, does real web research, and returns structured findings you can take into a discovery session.

## When to reach for it

- **Entering a new space.** Before brainstorming ideas in a market you haven't worked in, get the lay of the land. Who already exists, what do they do, what have users said about them?
- **Validating a direction.** When you're about to commit to an opportunity or idea, check what's been tried. Often the "new" idea has been shipped three times and failed for reasons worth knowing.
- **Filling a gap in the graph.** If an opportunity has thin evidence, run targeted research to thicken it. If a high-risk assumption has no basis in external data, see what the world already knows.
- **Periodic structural review.** Every few weeks, run an opportunity-space analysis. It flags gaps, redundancy, and imbalance that accumulate gradually.

## What it does

### Competitive research

Researches direct competitors, adjacent solutions, and analogous products. Returns structured profiles: what they do, how they position, what users say works and doesn't, what patterns emerge across the category, what gaps exist in the current market.

Output is a brief you can read in ten minutes, not a report you have to skim. Sources cited, claims separated from speculation.

### Market trends

Researches technology shifts, user behavior changes, business model evolution, and regulatory developments in your space. Focuses on trends with evidence of real adoption — not hype. Returns implications for your opportunity graph: which trends strengthen which opportunities, which weaken them.

### Opportunity-space analysis

Reads your entire graph and analyzes it for structural health:

- **Gaps.** Objectives with few or no opportunities. Ideas without assumptions. Opportunities without ideas.
- **Redundancy.** Overlapping opportunities that should be consolidated. Duplicate ideas.
- **Consolidation opportunities.** Places where multiple artifacts could merge into a cleaner structure.
- **Imbalances.** Effort concentrated in one area while other strategic objectives are neglected.

Then does targeted research to fill the gaps it identifies.

### Deep dive

In-depth research on a specific opportunity or idea: how others have solved the problem, what evidence exists for the underlying need, what has been tried and failed, what enabling technology has recently emerged.

The right move when you're choosing between ideas or trying to independently validate an assumption without running an experiment.

## What it doesn't do

- **Make strategic decisions.** It presents evidence. You decide.
- **Modify the opportunity graph.** It suggests changes. You incorporate them through interactive discovery sessions.
- **Prioritize.** It informs prioritization by surfacing evidence. The call happens in [prioritize](../skills/prioritize.md).

Findings are input, not output. Take the research into a discovery session. Use [explore](../skills/explore.md) to brainstorm on top of it, [critique](../skills/critique.md) to pressure-test ideas against it, [prioritize](../skills/prioritize.md) to let it shift your ranking.

## Research quality principles

The agent follows explicit standards:

- **Primary sources over aggregators.** Product pages over blog roundups.
- **Evidence over opinion.** User numbers over analyst predictions.
- **Recency.** Prioritizes the last twelve to eighteen months.
- **Multiple perspectives.** Marketing material plus user reviews plus critical analysis.
- **Citations.** Everything with a source link.

The agent will flag when it can't find good evidence rather than filling the gap with speculation.

## How to invoke

Ask in plain language. Examples:

```
Research the observability alerting competitive landscape.
```

```
What are the current market trends around AI in customer support?
```

```
Analyze the structure of my opportunity graph and flag any gaps.
```

```
Do a deep dive on how other platforms handle paper bookmarking
for researchers.
```

The agent will run for a few minutes and return a brief.

## Tips

- **Start with competitive research when entering a new space.** Understanding what exists grounds your discovery work in reality. Brainstorming an opportunity space in ignorance of what competitors are doing is expensive.
- **Run opportunity-space analysis periodically.** Especially after a few discovery sessions. It catches structural issues that are hard to see from inside.
- **Deep dives are for specific decisions.** When you're choosing between ideas or trying to validate an assumption without an experiment, a focused deep dive provides targeted evidence.
- **Don't confuse research with decisions.** The research agent informs. The interactive skills help you act. Going straight from research to action skips the move where the team actually builds shared understanding.

## Related

- [Skills reference](../skills/) — interactive skills where research findings get incorporated.
- [Explore](../skills/explore.md) — use research to ground brainstorming.
- [Critique](../skills/critique.md) — use research to make critique rounds more specific.
- [Objectives](../artifacts/objective.md) — business outcomes research should connect to.
- [Opportunities](../artifacts/opportunity.md) — gaps research can fill.
