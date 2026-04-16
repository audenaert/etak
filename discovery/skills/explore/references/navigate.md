# Navigate the Opportunity Space

## What This Is For

This is the "seeing" side of explore. You help the PM look at what exists in the
opportunity space -- the full picture, a filtered slice, or a single node and its
connections. Navigation is never just display; it always carries a point of view about
where to focus next.

## Principles

**The graph is in `docs/discovery/`.** Use Glob to find `.md` files, read their
frontmatter to build the picture, and parse typed links (`supports`, `addresses`,
`assumed_by`, `tests`, `result_informs`) to understand relationships. If `docs/discovery/`
doesn't exist, say so and suggest starting wherever feels natural.

**Show structure, not data.** The PM wants to understand the shape of the space, not
read a database dump. Lead with the summary table, then the tree. When the space is
large, summarize and offer to drill in rather than flooding the conversation.

**Every display carries a recommendation.** After showing the graph, say where you'd
focus and why. Use the signal priority order from the core guidelines: stale experiments,
high-risk untested assumptions, unsounded ideas, sparse opportunity spaces, validated
ideas ready to scope. Present 1-3 recommendations, leading with the single most
impactful action.

**Context travels with the node.** When showing a single artifact, always show its
neighborhood -- one level up (what it supports) and one level down (what supports it).
A node without context is meaningless.

## Moves

### Full overview

When the PM asks to see the space without specifying a focus. Show:

A summary table with counts by level and status:
```
## Discovery Overview

| Level        | Total | By Status                              |
|--------------|-------|----------------------------------------|
| Objectives   | 3     | 2 active, 1 paused                     |
| Opportunities| 7     | 5 active, 1 paused, 1 resolved         |
| Ideas        | 12    | 4 draft, 5 exploring, 2 validated, 1 ready_for_build |
| Assumptions  | 18    | 10 untested, 5 validated, 3 invalidated|
| Experiments  | 4     | 1 planned, 2 running, 1 complete       |
```

Then the tree structure with emoji prefixes and indentation:
```
📋 Objective: Increase researcher engagement (active)
  ├── 🔍 Opportunity: Researchers struggle to find related works (active)
  │   ├── 💡 Idea: AI-powered related works suggestions (exploring)
  │   │   ├── ❓ Assumption: Researchers trust AI suggestions (untested, importance: high, evidence: low)
  │   │   └── ❓ Assumption: Embedding similarity maps to relevance (untested, importance: high, evidence: medium)
  │   └── 💡 Idea: Citation graph explorer (draft)
  └── 🔍 Opportunity: No way to track reading progress (active)
```

For assumptions, always show importance and evidence levels in the tree.

### Filtered view

When the PM asks about a specific type or status ("show me untested assumptions",
"what objectives do we have", "active opportunities"). Show matching nodes with their
immediate context -- one level up and one level down -- so the PM can see where each
node sits in the graph.

### Single node view

When the PM asks about a specific artifact ("tell me about the search idea", "what's
connected to the engagement objective"). Show:

- Full content of the node
- Parents (what it supports, addresses, etc.)
- Children (what supports, addresses it)
- All frontmatter fields including status
- A read on the node's health using the artifact-level signals from the guidelines

### Tree from a root

When the PM asks for a tree rooted at a specific node ("show me the tree for the
engagement objective"). Show the full subtree below that node with the same emoji and
indentation format as the full overview.

### Topic search

When the PM describes an area rather than a specific artifact ("what do we have on
onboarding", "anything about search"). Search across node names and content, then
display matching nodes grouped by level with their connections.

## Reading the Conversation

**"Show me" / "what's there" / "where are we"** -- full overview unless they specify
a focus.

**A specific artifact name** -- single node view with neighborhood.

**A type or status filter** -- filtered view.

**"Tree" or "tree for X"** -- tree display rooted at the specified node or full tree.

**A topic or domain area** -- topic search.

**After displaying, the PM reacts to a gap** -- this is the transition to brainstorming.
Don't wait for them to explicitly say "brainstorm" -- if they notice an objective with
no opportunities and say "hmm, we should think about that," you're already in
brainstorm territory.

## Failure Modes

**Data dump.** Showing everything at full detail when the space has fifty nodes. Always
summarize first, then offer to drill in.

**Display without point of view.** Showing the graph and stopping. The PM came to you
to understand where things stand, which means they want to know what to do about it.
Always include a recommendation.

**Orphan blindness.** Missing nodes that aren't connected to anything -- ideas without
an opportunity, assumptions without an idea. These are worth flagging: "I notice
[idea] isn't connected to any opportunity -- is that intentional?"

**Stale read.** Using cached knowledge of the graph instead of actually reading
`docs/discovery/`. Always read from disk -- the PM may have edited files directly.

## Transitions

- The PM sees a gap and wants to fill it --> shift to brainstorming (same skill, see `brainstorm.md`)
- The PM sees a problem and wants to examine it --> suggest **critique**
- The PM sees untested assumptions and wants to act --> suggest **sound** or **experiment**
- The PM wants to choose where to focus --> suggest **prioritize**
- The PM wants to step back and think --> suggest **orient**
