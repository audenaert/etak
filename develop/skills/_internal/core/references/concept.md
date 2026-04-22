# The Development Graph

The development system maintains a **persistent, evolving map** of your engineering
work — what's been decided, what's planned, what's in flight, what's shipped, and the
technical reasoning behind each decision.

This map lives in `docs/development/` as markdown artifacts with typed links between
them. It is your team's institutional memory for how the software gets built.

## Why This Exists

Engineering teams lose decisions constantly. The reasoning behind an architecture
choice is in a Slack thread that scrolls away. A workstream's interface contract lives
in someone's head. A spike's findings evaporate when the author changes teams. Stories
merge without a clear record of what "done" meant. Months later, someone reopens the
question and the whole debate repeats.

The development graph captures this knowledge in a structured, navigable form that
persists across conversations, sprints, and team changes. Its value compounds over
time:

- **Grounded specs** make implementation decisions reproducible
- **ADRs** prevent relitigating settled questions
- **Workstream contracts** let parallel teams integrate without surprises
- **Stories with testable ACs** are the contract of "done"
- **Linked artifacts** make it possible to trace any line of code back to the business
  outcome it serves

## The Structure

The development graph is a directed graph with two layers of node types.

**Work items** (what we're doing):

```
Initiative          Multi-project strategic container
  └─ Project        Bounded deliverable
       ├─ Epic      Themed group of related stories
       │    └─ Story     User-facing increment with acceptance criteria
       │         └─ Task     Developer-scoped implementation step
       ├─ Workstream    Parallel delivery track
       └─ Milestone     Sequenced project checkpoint
```

Plus standalones that don't require the full hierarchy: **Bug**, **Chore**,
**Enhancement**, **Spike**.

**Two frames, one graph.** Stories describe change in the **user's frame** — "as a
persona, I want a capability, so that an outcome." Tasks describe change in the
**engineering frame** — technical work the system needs to do for the story to
exist. A story can live without tasks when implementation is obvious; tasks are
the place for non-user-visible work (migrations, feature flags, telemetry) or
cross-story technical changes parented at the epic level.

**Design artifacts** (how we're building it):

```
Spec        Technical specification, scoped to a project/epic/story
  └─ ADR    Architecture decision record
```

Typed links connect them: stories are `parent`-ed by epics, epics by projects, projects
by initiatives. Specs are `for` a work item. ADRs are produced from specs. Workstreams
and milestones reference each other through interface contracts and delivery
commitments.

## How It Connects to Discovery

Development doesn't stand alone. Ideas originate upstream in
[etak-discovery](../../../../discovery/README.md) — customer opportunities surfaced,
solutions brainstormed, assumptions tested.

The bridge: when an idea's critical assumptions are validated, it gets **scoped** into
the development graph. The resulting initiative, project, or story carries a
`from_discovery` link back to the idea it came from. This is a one-way reference —
development points back, discovery does not point forward — and the **development
side owns the traceability**. You can always ask "why are we building this?" and
follow the link; and in regulated environments, the `from_discovery` → story → task
→ PR chain satisfies the common compliance requirement that production changes
trace back to the change request that initiated them.

Not every work item starts in discovery. Bugs arrive from production. Chores come from
dependency updates. Enhancements come from engineering judgment. The `from_discovery`
link is optional; the work item graph stands on its own.

## How to Work With It

Like the discovery graph, this is not a waterfall. You can enter anywhere:

- **Top-down**: Start with an initiative, decompose into projects, epics, stories
- **Bottom-up**: Start with a concrete story or bug, trace upward to its project
- **Middle-out**: Start with a spec or ADR; the work items it governs fall into place

The natural rhythm is: **survey** the state (what's ready, what's blocked, what's
stalled) → **plan** what to do next (scope, decompose, sequence) → **spec** how to
build it (grounded design, recorded decisions) → **assess** readiness (critique
specs, refine ACs, size) → **build** (TDD, commit, PR) → back to survey with what
you've learned.

The skills help you work through this rhythm without needing to remember which tool
to use — describe what you want to do and the system routes you to the right process.

## Relationship to the 4Ds

Etak organizes work into four phases: **Discover, Design, Develop, Deliver**.

- **Design** (future etak-design plugin) handles UX and interaction design.
  Wireframes, flows, interaction models. That work produces inputs to stories and
  specs — but the UX artifacts live in the design plugin, not here.
- **Develop** (this plugin) handles technical design and implementation. Specs and
  ADRs are technical design; they answer "how will we build this?" That's a Develop
  concern, not a Design concern, in this taxonomy.
- **Deliver** (future etak-deliver plugin) handles the downstream concerns: code
  review, QE verification, security audit, CI/CD, release, operational docs.
  Develop-phase work produces PRs that enter the Deliver pipeline.

Agents in etak-deliver can be invoked *during* Develop for shift-left concerns — a
security-lead doing threat modeling on a new spec, or a quality-engineer reviewing
testability of acceptance criteria before implementation starts. The plugins are
independent; the workflows span them.
