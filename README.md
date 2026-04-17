# Etak

A thinking partner for product discovery, delivered as a [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) plugin.

Discovery is a conversation with the situation. You are trying to stay oriented in a space where the customer is moving, the market is moving, and your own understanding is a few steps behind both. Etak gives you a thinking partner that holds the structure for you: it grows a living opportunity graph from the hallway conversations, half-formed hypotheses, and customer quotes you are already producing, and it helps you make decisions from it.

Etak runs in the terminal, alongside the rest of your development work. It writes nothing you can't read, edit, or delete yourself. Every artifact is a markdown file in your repo.

## Install

**Prerequisites.** Claude Code installed and authenticated. See [Claude Code setup](https://docs.claude.com/en/docs/claude-code/setup).

**1. Clone the repo.**

```bash
git clone https://github.com/nealaudenaert/etak.git ~/etak
```

**2. Register the discovery plugin in your project's `.claude/settings.json`.**

```json
{
  "plugins": [
    { "path": "/home/you/etak/discovery" }
  ]
}
```

Use an absolute path. Restart Claude Code if it was running.

**3. Confirm it loaded.**

```
/plugins
```

You should see `etak-discovery` listed with six skills and one agent.

That is the whole install. There is no database, no web service, and no account to create. Discovery artifacts land in `docs/discovery/` in whatever repo you point Claude Code at.

## Thirty-second orientation

You talk to Claude Code the way you normally would. Etak listens for discovery moves and activates the right skill.

- "I've been hearing customers complain about search." → Etak opens a reflection, captures the signal, and asks what you want to do with it.
- "I have an idea for how to fix onboarding." → Etak captures the idea and traces it back to an opportunity and an objective.
- "What are we assuming?" → Etak surfaces the beliefs your idea rests on and flags the riskiest ones.
- "What should I work on?" → Etak reads your graph and tells you the highest-leverage next move.

You do not need to learn skill names. Describe the move you want to make. Etak picks the skill.

## Read next

- [**Quick start**](docs/quickstart.md) — five minutes, grounded in how a busy PM actually works.
- [**Tutorials**](docs/tutorials/) — runnable walkthroughs, each paired with a set of questions worth thinking about first.
- [**Skills reference**](docs/skills/) — what each skill does and when to reach for it.
- [**Artifacts reference**](docs/artifacts/) — the things Etak builds and how they connect.
- [**About the name**](docs/context/about-the-name.md) — the Polynesian navigation concept the tool is built around. Start here if you want to understand the philosophy.
- [**Foundations**](docs/context/foundations.md) — the research that informs how Etak works. Optional, but useful if you want to go deeper.

## For plugin authors

Etak is structured as a plugin marketplace. See [CONTRIBUTING.md](CONTRIBUTING.md) for how to add a plugin and [`registry.json`](registry.json) for the current catalog.

## License

Copyright (c) Neal Audenaert. All rights reserved.
