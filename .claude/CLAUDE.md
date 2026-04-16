# Etak Plugin Marketplace

This repo is a plugin marketplace — a registry of Claude Code plugins (skills and agents). It is not an application.

## Layout

```
<plugin-name>/                 # one top-level dir per plugin
  .claude-plugin/plugin.json   # manifest (see plugin-schema.json)
  skills/<skill>/SKILL.md      # skill definitions
  agents/<agent>/SKILL.md      # agent definitions
  docs/                        # plugin documentation
  README.md                    # plugin overview
registry.json                  # central catalog of all plugins
plugin-schema.json             # JSON Schema for plugin manifests
```

## Conventions

- Every plugin directory must contain `.claude-plugin/plugin.json` conforming to `plugin-schema.json`.
- Plugin names use the `etak-` prefix in manifests (e.g. `etak-discovery`), but directory names omit it (e.g. `discovery/`).
- When adding or modifying a plugin, keep `registry.json` in sync with the plugin's `plugin.json`.
- Do not modify plugins without updating their version number.
- See `CONTRIBUTING.md` for the full submission guide.
