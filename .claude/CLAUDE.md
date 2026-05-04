# Etak Plugin Marketplace

This repo is a plugin marketplace — a registry of Claude Code plugins (skills and agents). It is not an application.

## Layout

```
<plugin-name>/                 # one top-level dir per plugin
  .claude-plugin/plugin.json   # plugin manifest
  skills/<skill>/SKILL.md      # skill definitions
  agents/<agent>.md            # agent definitions (single markdown file with frontmatter)
  docs/                        # plugin documentation
  README.md                    # plugin overview
.claude-plugin/marketplace.json  # catalog Claude Code reads on install
```

## Conventions

- Every plugin directory must contain `.claude-plugin/plugin.json`. The plugin's `name` must match the entry in `.claude-plugin/marketplace.json` and the directory name (e.g. directory `discovery/` ↔ name `discovery`).
- Plugin and marketplace manifests reference the upstream Claude Code JSON schemas via `$schema` for editor validation. See [Claude Code plugin reference](https://code.claude.com/docs/en/plugins-reference) for the authoritative field list.
- Do not modify plugins without updating their `version`.
- See `docs/meta/skill-design-guide.md` for authoring conventions.
