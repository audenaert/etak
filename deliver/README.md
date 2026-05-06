# deliver

The delivery plugin for Etak. A specialist team in the terminal: code review, story verification, release engineering, security audit, infrastructure and operations, and documentation. Each phase has an interactive skill (you pair with it) and an autonomous agent (it works on its own and reports back).

This directory holds the plugin manifest, skill definitions, and agent definitions. Documentation for using the plugin lives in the repo-wide docs tree.

## Start here

- [**Install**](../README.md#install) — how to register this plugin with Claude Code.
- [**Overview**](../docs/deliver.md) — the skills, agents, and where they fit.
- [**Skill definitions**](skills/) — each skill's full specification.
- [**Agent definitions**](agents/) — each agent's full specification.

## Contents

| Directory | Purpose |
|-----------|---------|
| `.claude-plugin/plugin.json` | Plugin manifest |
| `skills/review/` | Self-review and PR feedback before submitting |
| `skills/verify/` | Story verification — AC coverage, test strategy, done means done |
| `skills/ship/` | Release engineering — diagnose CI, cut releases, get to green |
| `skills/secure/` | Threat modeling and risk audit |
| `skills/operate/` | Infrastructure as code, pipelines, observability |
| `skills/docs/` | Keep docs in sync with reality |
| `skills/foundation/` | Shared delivery model + interaction guidelines (Claude-invocable, hidden from `/` menu) |
| `agents/reviewer.md` | Autonomous code review across multiple dimensions |
| `agents/quality-engineer.md` | Autonomous story and AC verification |
| `agents/release-engineer.md` | Autonomous release and CI repair |
| `agents/security-lead.md` | Autonomous threat modeling and risk audit |
| `agents/devops.md` | Autonomous infrastructure and operations |
| `agents/tech-writer.md` | Autonomous documentation sync |

## Relationship to other etak plugins

- **develop** — primary upstream. Stories, specs, ADRs, and tasks flow into deliver work via `from_discovery` and parent links. When develop is not installed, deliver agents read the diff, branch name, and PR description directly.
- **discovery** — secondary upstream. `quality-engineer` follows `from_discovery` links from stories to assumptions when present, so verification can prefer the riskiest beliefs first. When discovery is not installed, verification falls back to AC coverage alone.
- **design** (future) — UX/IxD. May inform reviewer's user-facing change pass.

The plugin works standalone. It gets sharper when companions are installed.

## Internal artifact writers

Deferred to a future version. The deliver-side artifacts that would benefit from durable storage (release notes, risk registry, runbooks) are not modeled in v0.1. Today the skills and agents read upstream artifacts and write directly to the codebase, the PR, or the running system.

## License

MIT — see [LICENSE](LICENSE). Copyright (c) 2026 Neal Audenaert.
