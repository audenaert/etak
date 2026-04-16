# Etak Discovery

Product discovery plugin for Claude Code. A thinking partner for product managers and
designers — explore customer needs, map the opportunity space, brainstorm solutions,
surface assumptions, and test your riskiest bets.

## Installation

Add to your project's `.claude/settings.json`:

```json
{
  "plugins": [
    { "path": "/path/to/etak/discovery" }
  ]
}
```

## Skills

Six skills, progressing from fluid to structured:

| Skill | Stance | What it does |
|-------|--------|-------------|
| **orient** | Receptive | Find your bearings — think out loud, reflect, make sense of half-formed observations |
| **explore** | Generative | See the opportunity space, notice gaps, brainstorm possibilities |
| **sound** | Exploratory | Probe beneath the surface — surface and examine hidden assumptions |
| **critique** | Adversarial | Stress-test ideas and opportunities from multiple angles |
| **prioritize** | Evaluative | Converge — rank options and decide where to focus |
| **experiment** | Rigorous | Design tests, run them, record results, propagate learning |

You don't need to invoke skills by name. Describe what you want — "I have an idea",
"what are we assuming?", "which should we focus on?" — and the right skill activates.

## Agent

| Agent | Description |
|-------|-------------|
| **product-researcher** | Competitive research, market trends, and opportunity space analysis |

## Artifacts

Discovery builds an opportunity space as linked markdown artifacts in `docs/discovery/`:

| Artifact | Description |
|----------|-------------|
| Objective | Business outcome or strategic direction |
| Opportunity | Customer need framed as a "How might we..." question |
| Idea | Proposed solution addressing an opportunity |
| Assumption | Testable belief underlying an idea |
| Experiment | Method to test an assumption |

## The Discovery Rhythm

**Explore → Sound → Critique → Prioritize → Experiment** — and back again.

Generate options, reveal what they rest on, stress-test them, decide where to focus,
and test your riskiest bets. The rhythm applies at every level: opportunities, ideas,
assumptions. Orient is your home base — the place to pause, reflect, and decide where
to go next.

## License

Copyright (c) Neal Audenaert. All rights reserved.
