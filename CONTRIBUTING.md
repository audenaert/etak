# Contributing a Plugin

## Directory Structure

Each plugin lives in its own top-level directory:

```
your-plugin/
├── .claude-plugin/
│   └── plugin.json          # required — plugin manifest
├── skills/
│   └── your-skill/
│       ├── SKILL.md          # skill definition (frontmatter + instructions)
│       └── references/       # optional supporting docs
├── agents/
│   └── your-agent/
│       └── SKILL.md          # agent definition
├── docs/                     # optional documentation
└── README.md                 # required — plugin overview
```

## Plugin Manifest

Every plugin must have a `.claude-plugin/plugin.json` that conforms to [`plugin-schema.json`](plugin-schema.json).

### Required Fields

| Field | Description |
|-------|-------------|
| `name` | Unique identifier. Lowercase, alphanumeric, hyphens only. Prefix with `etak-`. |
| `description` | One-line summary (max 200 chars). |
| `version` | Semver string (e.g. `0.1.0`). |
| `author` | Object with `name` (required) and optional `url`. |
| `license` | SPDX identifier or `UNLICENSED`. |
| `categories` | Array of one or more categories (see below). |

### Optional Fields

| Field | Description |
|-------|-------------|
| `keywords` | Up to 10 search tags (lowercase, hyphens). |
| `skills` | Array of `{ name, description, path }` for each skill. |
| `agents` | Array of `{ name, description, path }` for each agent. |
| `dependencies` | Plugin names this plugin depends on. |
| `repository` | Source repo URL. |
| `homepage` | Documentation URL. |
| `minClaudeCodeVersion` | Minimum Claude Code version required. |

### Categories

`discovery` · `development` · `design` · `testing` · `devops` · `data` · `documentation` · `security` · `productivity` · `integration`

## SKILL.md Format

Each skill or agent needs a `SKILL.md` with YAML frontmatter:

```yaml
---
name: your-skill-name
description: >
  What this skill does. Include trigger phrases so Claude Code
  knows when to invoke it.
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---
```

The body contains the full instructions Claude Code follows when the skill is invoked.

## Submission Process

1. Create your plugin directory at the repo root.
2. Add `.claude-plugin/plugin.json` conforming to the schema.
3. Add at least one skill or agent with a `SKILL.md`.
4. Add a `README.md` describing the plugin.
5. Add an entry to `registry.json` with `name`, `description`, `version`, `path`, `author`, `license`, `categories`, `keywords`, `skills`, and `agents`.
6. Open a PR against `main`.

## Naming Conventions

- Plugin directories: lowercase, hyphens (e.g. `code-review`)
- Plugin names in manifests: prefix with `etak-` (e.g. `etak-code-review`)
- Skill/agent names: lowercase, hyphens, no prefix (e.g. `code-review`)
