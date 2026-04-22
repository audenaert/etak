# Artifacts reference

Etak builds two linked graphs, one for discovery and one for development. Each node is an **artifact**: a typed, linked markdown file with a YAML header. This reference describes both sets.

## Discovery artifact types

| Artifact | Role in the graph |
|----------|-------------------|
| [**Objective**](objective.md) | Business outcome the team is responsible for |
| [**Opportunity**](opportunity.md) | Customer need framed as a *how might we* question |
| [**Idea**](idea.md) | Proposed solution that addresses an opportunity |
| [**Assumption**](assumption.md) | Testable belief that must be true for an idea to work |
| [**Experiment**](experiment.md) | Test that produces evidence about an assumption |
| [**Critique**](critique.md) | Structured examination of an idea or opportunity from an outside perspective |

The discovery artifact set is intentionally open. These six types are the current shape of the discovery model, not a closed enumeration. As Etak's discovery discipline evolves — and as practice reveals artifact shapes that are systematically absent — new types will be added. Critique is the first addition beyond the original five, and it establishes the pattern: each addition ships as a file-based schema doc (here in `docs/artifacts/`) and as a graph node type in the data model simultaneously, so both representations stay in lockstep. Future additions follow the same discipline.

## How they connect

```
objective
  ↑ supports
opportunity
  ↑ addresses
idea
  ↓ assumed_by (inverse of)
assumption
  ↓ tests (inverse of)
experiment
```

Opportunities support one or more objectives. Ideas address one or more opportunities. Assumptions are held by one or more ideas. Experiments test one or more assumptions. The graph is not a tree — an idea can serve multiple opportunities, an assumption can underlie multiple ideas, and an experiment can inform multiple assumptions. That is deliberate. See [foundations](../context/foundations.md) on why the graph model is honest about shared structure that a tree cannot represent.

## Where they live

Discovery artifacts are written to `docs/discovery/` in your repo, one subdirectory per type:

```
docs/discovery/
  objectives/
  opportunities/
  ideas/
  assumptions/
  experiments/
```

Each file is a markdown document with a YAML frontmatter header. The header carries the typed fields (name, status, relationships). The body holds free-form description, evidence, context. Both are readable and editable without Etak.

## The development artifacts

When a validated idea moves into execution, work is tracked as a second graph under `docs/development/`. The development graph has a containment hierarchy (project, epic, story, task), plus parallel tracks and sync points (workstream, milestone), design artifacts (spec, ADR), time-boxed investigation (spike), and standalone work items (bug, chore, enhancement).

| Artifact | Role in the graph |
|----------|-------------------|
| [**Project**](project.md) | Bounded deliverable; the coordination hub for most work |
| [**Epic**](epic.md) | Themed group of related stories |
| [**Story**](story.md) | User-frame increment with testable acceptance criteria |
| [**Task**](task.md) | Engineering-frame technical change. Optional. |
| [**Workstream**](workstream.md) | Parallel delivery track within a project |
| [**Milestone**](milestone.md) | Sequenced project checkpoint, thin and demo-able |
| [**Spec**](spec.md) | Grounded technical design for a project, epic, or story |
| [**ADR**](adr.md) | Hard-to-reverse architectural decision |
| [**Spike**](spike.md) | Time-boxed investigation with a concrete outcome |
| [**Bug**](bug.md) | Standalone defect |
| [**Chore**](chore.md) | Standalone maintenance |
| [**Enhancement**](enhancement.md) | Small standalone improvement |

### How they connect

```
project ──── workstreams ──── milestones
   │ parent
   ▼
epic
   │ parent
   ▼
story
   │ parent (optional)
   ▼
task
```

Project, epic, story, and task form the containment hierarchy: one parent, one owner at each level. Workstream and milestone cut across the hierarchy; an epic or story lives in one workstream and may target one milestone. Specs and ADRs attach to a work item via `for`, not `parent`. Spikes, bugs, chores, and enhancements stand alone unless they reference a related work item.

### Tasks are optional

A story describes a change from the user's point of view: persona, capability, outcome, testable ACs. A task describes a change to the *system*: files, schemas, interfaces. When a story's implementation is obvious from the ACs and a glance at the codebase, tasks add ceremony without adding clarity. Create tasks when the technical work isn't user-visible (migrations, feature flags, telemetry), when multiple PRs close a story, when a technical change enables multiple stories (parent to the epic), or when decomposition is real design work. Otherwise, implement the story directly.

### From discovery to development

Every development artifact can carry a `from_discovery` field that links back to a validated idea in `docs/discovery/`. The link is one-way: development points back; discovery never points forward. This matches how compliance audit trails work in practice, where changes to production systems trace back to the change request that initiated them.

Populate `from_discovery` on the development artifact that first takes up the work, usually a project or story. Omit for bugs, chores, and enhancements that originate in production or engineering judgment.

### Where they live

Development artifacts are written to `docs/development/` in your repo, organized by type:

```
docs/development/
  projects/
  epics/
  stories/
  tasks/
  work-items/    # bug, chore, enhancement
  spikes/
  specs/
  adrs/
  workstreams/
  milestones/
```

Same shape as discovery artifacts: markdown body with YAML frontmatter. The authoritative schema for every type is in [`develop/skills/_internal/core/references/schemas.md`](../../develop/skills/_internal/core/references/schemas.md).

## The graph is a model, not a map

Every artifact is part of an *evolving model of your team's current understanding.* It is not a specification of reality. It is not a comprehensive catalog of every need or idea. It is what you know, what you believe, and what you've chosen to track, at a given moment.

The graph is messy on purpose. It has missing links. It has ideas with no assumptions under them because nobody has thought hard enough yet about how they might fail. It has opportunities that were promising six months ago and quietly went stale. This is not a bug. It is an accurate reflection of what real discovery looks like. The tool's job is to help you see the state clearly, not to paper over it.

See [foundations](../context/foundations.md) for the longer argument.

## Related

- [Develop overview](../develop.md) — the upstream view of the development graph.
- [Skills reference](../skills/) — the skills that create, modify, and propagate artifacts.
- [Quick start](../quickstart.md) — how to build your first artifacts.
- [Getting started tutorial](../tutorials/getting-started.md) — walkthrough that produces each artifact type.
