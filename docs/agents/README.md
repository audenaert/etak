# Agents reference

Agents are autonomous workers. Unlike skills, which you converse with interactively, agents do longer-running work on their own and return a structured result. You invoke them when you want external context, bulk analysis, or synthesis that would be tedious to drive turn by turn.

The skills are the interactive tools. Agents feed them.

## Discovery agents

- [**product-researcher**](product-researcher.md) — autonomous competitive research, market trend analysis, and opportunity-space structural review. The thing you reach for when you need outside evidence before a discovery session.

## Develop agents

See [the develop overview](../develop.md) for context on how these fit together.

- [**architect**](architect.md) — produces a complete technical design for a project or epic. Reads the codebase deeply, considers alternatives, drafts spec and ADRs, stress-tests through multi-lens critique.
- [**tech-lead**](tech-lead.md) — orchestrates delivery. Breaks work into concrete items, fills gaps, creates an execution plan, dispatches the other agents, parallelizes independent work.
- [**developer**](developer.md) — autonomous TDD implementation in isolated git worktrees. Handles stories, bugs, and spikes. Honest self-review before submitting the PR.

## Deliver agents

See [the deliver overview](../deliver.md) for context on how these fit together.

- [**reviewer**](reviewer.md) — autonomous code review across six dimensions (problem fit, simplicity, critical zones, boundaries, security, tests). Posts findings to the PR.
- [**quality-engineer**](quality-engineer.md) — verifies a story autonomously. Walks each AC, evaluates test coverage at the right layer, optionally writes E2E tests to close gaps.
- [**release-engineer**](release-engineer.md) — diagnoses and fixes CI failures from logs; cuts releases, drafts notes in the user's frame.
- [**security-lead**](security-lead.md) — autonomous threat modeling, code audit, security test design, and a persistent risk registry.
- [**tech-writer**](tech-writer.md) — autonomous documentation drift correction across API docs, architecture, user docs, and screenshots.
- [**devops**](devops.md) — designs and implements IaC, CI/CD, containerization, observability, and environment management. Never applies to live infrastructure without explicit approval.

## How agents fit with skills

A useful pattern:

1. **Before exploring.** Run product-researcher to bring in competitive and market context. Skills that generate options ([explore](../skills/explore.md)) produce better options when grounded in external evidence.
2. **Before critique.** Competitor research often gives critique sessions specific material to work with — "how would \[competitor\] respond to this?" is a more interesting question when you know what \[competitor\] has actually been doing.
3. **Periodically.** Run opportunity-space analysis every few weeks to catch structural issues in the graph before they compound.

Agent findings are input, not output. Take the research into a discovery session and the interactive skills turn it into decisions.
