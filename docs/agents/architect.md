# architect

**Type:** autonomous agent. **Purpose:** produce a complete technical design for a project or epic.

The interactive [develop skills](../skills/) help an engineer think and build in dialogue. architect works differently. Hand it a project or epic and it reads the codebase deeply, considers alternatives, drafts a spec and the ADRs that go with it, and stress-tests the design through multi-lens critique. What comes back is a proposal you review, not a draft you co-author.

## When to reach for it

- **Project-scale design.** The work spans multiple subsystems. You want a coherent proposal rather than a series of small decisions.
- **You want something to react to.** A complete draft, even one you end up rewriting, gets faster to a good design than a blank page.
- **Unfamiliar territory.** A new module, a new integration, a part of the codebase you haven't touched. The agent reads everything and grounds the design in what already exists.
- **Before breaking work into stories.** A spec that names the components, contracts, and migration path lets planning produce stories that hang together.

## What it does

### Reads the codebase thoroughly

Architecture, data models, API layer, auth model, test infrastructure. How similar concerns are handled today, what's established versus ad hoc, what's fragile. The design references specific files and patterns rather than generic recommendations.

### Explores alternatives

For any non-trivial design, the agent considers at least two viable approaches and presents each fairly: what it is, how it works, strengths, weaknesses, codebase fit, future flexibility. The rejected alternatives sit in the spec so you can see the shape of the decision.

### Drafts the spec and ADRs

Writes a complete specification covering context, current state, proposed approach, alternatives considered, non-functional requirements, adjacent opportunities, migration and evolution, open questions, and risks. Records hard-to-reverse or non-obvious decisions as ADRs linked from the spec.

### Stress-tests the draft

Runs a multi-lens critique (architect, QE, DevOps, security, new team member, on-call) against the draft. For project-scale designs, fans out focused sub-agent reviews on distinct aspects (data model, API, infrastructure, migration) and consolidates findings. Updates the spec where critique is valid; flags judgment calls in the briefing.

### Presents a structured briefing

Leads with the recommendation and the decisions that need your input. Risks, review summary, and artifact paths follow. The full spec is available on request rather than dumped into the briefing.

## What it doesn't do

- **Co-author interactively.** If you want to iterate decision by decision, use [`/spec`](../skills/spec.md). architect is the right move when you want a proposal to review.
- **Implement.** It produces the design. Implementation goes to [developer](developer.md), usually dispatched by [tech-lead](tech-lead.md).
- **Decide for you.** Open questions and judgment calls come back labeled. You resolve them.
- **Invent patterns.** It grounds decisions in what exists. If a new pattern is warranted, the spec says so and explains why.

## How it operates

- **Read before writing.** Prose gets drafted after the codebase pass, not before.
- **Alternatives get treated fairly.** If the agent can't articulate why someone might prefer the one it didn't choose, it hasn't thought hard enough.
- **Complexity earns its place.** No over-engineering for hypothetical futures. The design serves the work on the roadmap, not speculative extensions.
- **Migrations get real attention.** Schema changes, data transformations, and backward compatibility are where designs most often fail in practice.
- **Short when short is right.** A simple design gets a short spec. The agent says so explicitly rather than padding.
- **Lead with decisions.** The briefing opens with what needs your input, not with preamble.

## How to invoke

Ask in plain language. Examples:

```
Architect the notification system project.
```

```
Design the auth migration epic. Read the current session
code thoroughly before proposing.
```

```
I need a technical proposal for the search rewrite. Give
me something to react to.
```

The agent will run for several minutes and return a briefing with the spec and ADR paths.

## Tips

- **Point at a work item.** The agent works best when `$ARGUMENTS` identifies a specific project, epic, or feature area with goals and constraints already written down.
- **Link discovery context if you have it.** The `from_discovery` field on the project lets the agent read the originating idea and opportunity. A design without product context optimizes for the wrong things.
- **Read the alternatives section.** The recommendation is easier to evaluate when you've seen what was considered and rejected.
- **Resolve open questions before implementation.** The spec carries them explicitly. Leaving them for the developer to guess at is where designs go wrong.
- **Use [tech-lead](tech-lead.md) when the project is large.** tech-lead dispatches architect for epics that need design, then sequences implementation against the resulting specs.

## Related

- [Develop overview](../develop.md) — where architect fits in the plugin.
- [`/spec`](../skills/spec.md) — the interactive alternative for small-to-medium scope design.
- [`/assess`](../skills/assess.md) — the skill whose stress-test move architect uses internally; you can run it yourself against an existing spec.
- [tech-lead](tech-lead.md) — dispatches architect when coordinating delivery.
- [developer](developer.md) — implements against the specs architect produces.
