# Artifacts reference

Etak builds an opportunity graph. The nodes are **artifacts** — typed, linked markdown files with YAML headers. This reference describes each artifact type, its role in the graph, its schema, and the actions that are useful to take on it.

## The five types

| Artifact | Role in the graph |
|----------|-------------------|
| [**Objective**](objective.md) | Business outcome the team is responsible for |
| [**Opportunity**](opportunity.md) | Customer need framed as a *how might we* question |
| [**Idea**](idea.md) | Proposed solution that addresses an opportunity |
| [**Assumption**](assumption.md) | Testable belief that must be true for an idea to work |
| [**Experiment**](experiment.md) | Test that produces evidence about an assumption |

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

Artifacts are written to `docs/discovery/` in your repo, one subdirectory per type:

```
docs/discovery/
  objectives/
  opportunities/
  ideas/
  assumptions/
  experiments/
```

Each file is a markdown document with a YAML frontmatter header. The header carries the typed fields (name, status, relationships). The body holds free-form description, evidence, context. Both are readable and editable without Etak.

## The graph is a model, not a map

Every artifact is part of an *evolving model of your team's current understanding.* It is not a specification of reality. It is not a comprehensive catalog of every need or idea. It is what you know, what you believe, and what you've chosen to track, at a given moment.

The graph is messy on purpose. It has missing links. It has ideas with no assumptions under them because nobody has thought hard enough yet about how they might fail. It has opportunities that were promising six months ago and quietly went stale. This is not a bug. It is an accurate reflection of what real discovery looks like. The tool's job is to help you see the state clearly, not to paper over it.

See [foundations](../context/foundations.md) for the longer argument.

## Related

- [Skills reference](../skills/) — the skills that create, modify, and propagate artifacts.
- [Quick start](../quickstart.md) — how to build your first artifacts.
- [Getting started tutorial](../tutorials/getting-started.md) — walkthrough that produces each artifact type.
