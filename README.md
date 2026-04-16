# Etak — Plugin Marketplace for Claude Code

A curated registry of plugins (skills and agents) for Claude Code.

## Available Plugins

| Plugin | Category | Description |
|--------|----------|-------------|
| [etak-discovery](discovery/) | discovery | Product discovery — explore customer needs, map opportunities, brainstorm solutions, test assumptions |

## Installation

Add a plugin to your project's `.claude/settings.json`:

```json
{
  "plugins": [
    { "path": "/path/to/etak/discovery" }
  ]
}
```

Replace the path with the absolute path to the plugin directory on your machine.

## Browsing

- **Registry** — [`registry.json`](registry.json) lists all plugins with metadata, categories, and keywords.
- **Schema** — [`plugin-schema.json`](plugin-schema.json) defines the manifest format for plugin authors.
- **Categories** — discovery, development, design, testing, devops, data, documentation, security, productivity, integration.

## License

Copyright (c) Neal Audenaert. All rights reserved.
