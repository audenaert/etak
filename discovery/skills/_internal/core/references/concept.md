# The Product Opportunity Space

The discovery system maintains a **persistent, evolving map** of your product's opportunity space — what you know about customer needs, the solutions you're considering, the assumptions behind them, and what you've learned from testing those assumptions.

This map lives in `docs/discovery/` as markdown artifacts with typed links between them. It is your team's institutional memory for product discovery.

## Why This Exists

Product teams lose knowledge constantly. Insights from user interviews fade. The reasoning behind decisions evaporates. Ideas get revisited without remembering why they were shelved. Assumptions go untested because no one remembers they were assumptions.

The opportunity space captures this knowledge in a structured, navigable form that persists across conversations, sprints, and team changes. Its value compounds over time:

- **Invalidated assumptions** save you from building the wrong thing
- **Validated assumptions** give you confidence to commit
- **The graph itself** becomes a map of what you know and don't know — making gaps visible

## The Structure

The opportunity space has a five-node core hierarchy plus two peripheral types.

The core hierarchy:

```
Objective          Why — the business outcome you're pursuing
  └─ Opportunity   Who/What — customer needs framed as "How might we..." questions
       └─ Idea     How — proposed solutions that address opportunities
            └─ Assumption   What must be true for the idea to work
                 └─ Experiment   How we test whether it's true
```

The peripheral types extend the graph without being part of the spine:

- **Critique** — a single round of structured examination targeting an idea or opportunity. Findings can graduate into assumptions.
- **Memo** — a sustained analytical document (framework, survey, literature review, structured argument) that informs the opportunity space without being a gap to fill or a solution to build. Optionally `supports` an opportunity or objective; may be foundational and link to nothing.

Typed links connect them: opportunities `support` objectives, ideas `address` opportunities, assumptions are `assumed_by` ideas, experiments `test` assumptions. Critiques target ideas or opportunities; memos optionally `support` objectives or opportunities.

## How to Work With It

This is not a waterfall. You can enter anywhere:

- **Top-down**: Start with an objective, explore opportunities, brainstorm ideas
- **Bottom-up**: Start with an idea, trace upward to find what opportunity it serves and what objective it ladders to
- **Middle-out**: Start with a customer need you've observed, connect it up to objectives and down to ideas

The natural rhythm is: **discover** what's possible (navigate, brainstorm, prioritize) → **define** it in the graph (create and refine artifacts) → **validate** your thinking (surface assumptions, critique, design experiments) → back to discover with what you've learned.

The skills help you work through this rhythm without needing to remember which tool to use — describe what you want to do and the system routes you to the right process.
