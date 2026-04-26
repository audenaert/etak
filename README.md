# Etak

A thinking partner for product teams, delivered as a [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) plugin marketplace.

Etak gives you a thinking partner that holds the structure for you. Discovery is a conversation with the situation: customer signals, half-formed hypotheses, and competitive context turn into a living opportunity graph you can make decisions from. Development is the conversation with the codebase: validated ideas turn into specs, plans, and shipped code with the discipline to keep traceability intact.

Etak runs in the terminal, alongside the rest of your work. It writes nothing you can't read, edit, or delete yourself. Every artifact is a markdown file in your repo.

## Plugins

Etak is a marketplace. Each plugin is installed on its own.

- **discovery** — product discovery. Explore the opportunity space, surface and test assumptions, prioritize, run experiments. Artifacts land in `docs/discovery/`.
- **develop** — software development. Scope validated ideas, plan and sequence the work, write grounded specs and ADRs, implement with TDD. Artifacts land in `docs/development/` and link back to discovery.
- **deliver** — software delivery. Review code, verify stories, ship safely, audit security, operate infrastructure, keep docs in sync. Pairs with develop at PR and release time.

Planned: **design** (UX/IxD).

## Install

**Prerequisites.** Claude Code installed and authenticated. See [Claude Code setup](https://docs.claude.com/en/docs/claude-code/setup).

**1. Add the marketplace.**

In Claude Code, run:

```
/plugin marketplace add audenaert/etak
```

**2. Install the plugins you want.**

```
/plugin install discovery@etak
/plugin install develop@etak
/plugin install deliver@etak
```

**3. Reload.**

```
/reload-plugins
```

**4. Confirm.** Run `/plugin` and open the *Installed* tab. You should see `discovery` (six skills, one agent), `develop` (six skills, three agents), and/or `deliver` (six skills, six agents).

That is the whole install. There is no database, no web service, and no account to create.

### Installing from a local clone

If you want to hack on the plugins rather than just use them:

```bash
git clone https://github.com/audenaert/etak.git ~/etak
```

Then point the marketplace command at your local path:

```
/plugin marketplace add ~/etak
/plugin install discovery@etak
/plugin install develop@etak
/plugin install deliver@etak
/reload-plugins
```

Edits take effect on the next reload.

## Thirty-second orientation

You talk to Claude Code the way you normally would. Etak listens for the move you want to make and activates the right skill.

**In discovery:**
- "I've been hearing customers complain about search." → captures the signal, asks what you want to do with it.
- "I have an idea for how to fix onboarding." → captures the idea and traces it back to an opportunity and an objective.
- "What are we assuming?" → surfaces the beliefs your idea rests on, flags the riskiest ones.

**In development:**
- "Scope this idea into a project." → turns a validated idea into a project with workstreams, epics, and milestones.
- "Is this story ready to build?" → readiness check. What's missing, what's vague, what needs a spike.
- "How should we build this?" → grounded spec, reading the actual codebase before proposing a design.

You do not need to learn skill names. Describe the move you want to make. Etak picks the skill.

## Read next

- [**Quick start**](docs/quickstart.md) — five minutes, grounded in how a busy PM actually works.
- [**Quick start for engineers**](docs/quickstart-engineer.md) — the same five-minute shape, adapted for a senior engineer evaluating `etak-develop`.
- [**Tutorials**](docs/tutorials/) — runnable walkthroughs, each paired with a set of questions worth thinking about first.
- [**Discovery skills**](docs/skills/) — what each discovery skill does and when to reach for it.
- [**Develop overview**](docs/develop.md) — the development plugin's skills, agents, and artifacts.
- [**Deliver overview**](docs/deliver.md) — the delivery plugin's skills and agents, paired with develop at PR and release time.
- [**Artifacts reference**](docs/artifacts/) — the things Etak builds and how they connect.
- [**About the name**](docs/context/about-the-name.md) — the Polynesian navigation concept the tool is built around.
- [**Foundations**](docs/context/foundations.md) — the research that informs how Etak works.

## For plugin authors

Plugin contributions are not open yet. The plugin format is still evolving and there are foundational design decisions to settle before external contributions will land well. See [`docs/meta/skill-design-guide.md`](docs/meta/skill-design-guide.md) for the authoring conventions when that changes.

## License

Copyright (c) Neal Audenaert. All rights reserved.
