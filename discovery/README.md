# discovery

The discovery plugin for Etak. A thinking partner for product managers and designers: explore customer needs, map the opportunity space, brainstorm solutions, surface assumptions, and test your riskiest bets.

This directory holds the plugin manifest, skill definitions, and agent definitions. Documentation for using the plugin lives in the repo-wide docs tree.

## Start here

- [**Install**](../README.md#install) — how to register this plugin with Claude Code.
- [**Quick start**](../docs/quickstart.md) — five-minute orientation for a busy PM.
- [**Tutorials**](../docs/tutorials/) — runnable walkthroughs.
- [**Skills reference**](../docs/skills/) — what each skill does and when to reach for it.
- [**Artifacts reference**](../docs/artifacts/) — the things this plugin builds.

## Contents

| Directory | Purpose |
|-----------|---------|
| `.claude-plugin/plugin.json` | Plugin manifest |
| `skills/orient/` | Home base — find your bearings |
| `skills/explore/` | See the space, generate options |
| `skills/sound/` | Surface hidden assumptions |
| `skills/critique/` | Stress-test from outside |
| `skills/prioritize/` | Converge on what matters |
| `skills/experiment/` | Design, run, and propagate tests |
| `skills/artifacts/` | Type registry, schemas, and per-type content guidance for discovery artifacts. Hidden from `/` menu (Claude-invocable only). |
| `agents/product-researcher.md` | Autonomous competitive and market research |

## Relationship to other etak plugins

- **design** (future) — sibling. UX and interaction design; often draws on discovery's opportunity space but is a separate concern.
- **develop** — downstream. When an idea is ready to build, development picks it up and references the source via the `from_discovery` link on projects and stories. Discovery never points forward; traceability is owned by the development side.
- **deliver** (future) — further downstream. Code review, QE verification, CI, security audit, release, docs.

## License

MIT — see [LICENSE](LICENSE). Copyright (c) 2026 Neal Audenaert.
