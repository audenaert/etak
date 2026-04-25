# etak-develop

The development plugin for Etak. A technical partner for engineering teams: turn validated ideas into work items, plan and sequence delivery, write grounded technical specs, record architectural decisions, and implement with TDD.

This directory holds the plugin manifest, skill definitions, and agent definitions. Documentation for using the plugin lives in the repo-wide docs tree.

## Start here

- [**Install**](../README.md#install) — how to register this plugin with Claude Code.
- [**Overview**](../docs/develop.md) — the skills, agents, and artifacts at a glance.
- [**Skill definitions**](skills/) — each skill's full specification.
- [**Agent definitions**](agents/) — each agent's full specification.

## Contents

| Directory | Purpose |
|-----------|---------|
| `.claude-plugin/plugin.json` | Plugin manifest |
| `skills/survey/` | Home base — navigate the work graph, see where things stand |
| `skills/plan/` | Scope, decompose, sequence, map dependencies |
| `skills/spec/` | Technical design — grounded specs and ADRs |
| `skills/assess/` | Stress-test work — readiness, feasibility, sizing, AC refinement |
| `skills/test/` | Strategy-first testing — write, run, debug |
| `skills/build/` | Interactive implementation — pair with the engineer |
| `skills/_internal/` | Internal artifact-writing skills |
| `agents/architect.md` | Autonomous technical design |
| `agents/tech-lead.md` | Orchestration across the delivery lifecycle |
| `agents/developer.md` | Autonomous implementation in isolated worktrees |

## Relationship to other etak plugins

- **etak-discovery** — upstream. Validated ideas flow in via the `from_discovery` link on projects and stories.
- **etak-design** (future) — UX/IxD. Informs stories and specs; separate concern from technical design.
- **etak-deliver** (future) — downstream. Code review, QE verification, CI, security audit, release, docs.

## License

Copyright (c) Neal Audenaert. All rights reserved.
