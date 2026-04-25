# Etak Plugin Marketplace

This repo is a plugin marketplace — a registry of Claude Code plugins (skills and agents). It is not an application.

## Layout

```
<plugin-name>/                 # one top-level dir per plugin
  .claude-plugin/plugin.json   # manifest (see plugin-schema.json)
  skills/<skill>/SKILL.md      # skill definitions
  agents/<agent>.md            # agent definitions (single markdown file with frontmatter)
  docs/                        # plugin documentation
  README.md                    # plugin overview
.claude-plugin/marketplace.json  # catalog Claude Code reads on install
plugin-schema.json             # JSON Schema for plugin manifests
```

## Conventions

- Every plugin directory must contain `.claude-plugin/plugin.json` conforming to `plugin-schema.json`.
- Plugin names use the `etak-` prefix in manifests (e.g. `etak-discovery`), but directory names omit it (e.g. `discovery/`).
- Do not modify plugins without updating their version number.
- See `docs/meta/skill-design-guide.md` for authoring conventions.
