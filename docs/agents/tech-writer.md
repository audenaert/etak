# tech-writer

**Type:** autonomous agent. **Purpose:** correct documentation drift autonomously — review and update API docs, architecture, user docs, and screenshots to match the current state of the codebase.

The interactive [`/docs`](../skills/docs.md) skill collaborates on individual pages or audits. The tech-writer agent does the same work autonomously across whole doc sets: detects existing conventions, walks the docs against the code, updates what's drifted, captures new screenshots, and reports what needed manual action.

## When to reach for it

- **Documentation has drifted across many pages.** Walking each page interactively is tedious; the agent does the audit and fixes in one pass.
- **A feature shipped without documentation.** The agent reads the code, detects the project's documentation conventions, and writes new pages in the matching style.
- **A release is going out and screenshots need refreshing.** The agent navigates via Playwright, captures new screenshots in the project's existing format, replaces old ones.
- **`developer` (when `etak-develop` is installed) just opened a PR.** It can dispatch the tech-writer to update docs for the change.

## What it does

### Detect existing conventions

Before writing anything, the agent reads the project's documentation style: what tool generates API docs, what style for inline docs (JSDoc, docstrings), what diagram format (Mermaid, PlantUML, ASCII), what level of detail, what tone (formal, conversational), where screenshots live, what changelog format. Documentation that looks inconsistent is worse than slightly stale documentation.

### Four documentation surfaces

**API documentation** (machine-derived). OpenAPI/Swagger, Zod/Joi/Yup schemas, JSDoc/TSDoc, Python docstrings, Go doc comments, GraphQL schema. The agent checks new endpoints/functions/types are documented; descriptions match current signatures; return types and errors are documented; examples are current. For schema-derived docs, regenerates via the project's generation step.

**Architectural documentation** (human-written). `docs/architecture/`, module READMEs, ADRs. The agent checks component diagrams against current module structure; new modules reflected; data flow descriptions match; dependency relationships accurate.

**End-user documentation.** `docs/`, separate doc sites (Docusaurus, MkDocs, GitBook, VitePress), in-app help, CHANGELOG.md. The agent checks feature descriptions match behavior; screenshots are current; instructions still work; changelog reflects shipped changes.

**Visual captures (screenshots).** Via Playwright when available for web. Saves to the existing image directory with descriptive filenames at consistent viewport sizes, capturing meaningful state (logged in, with data, features in context). When capture isn't possible, describes exactly what's needed — URL, required state, what should be shown.

### Update each affected surface

For each surface, the agent finds what's drifted, updates it (preserving the user's frame), regenerates if generation is configured, and verifies the output. When docs and code disagree, the code is right — the agent updates the docs unless the code is the bug, in which case it surfaces that for the engineer rather than silently masking the bug.

### Report

A structured summary: what was updated per surface, screenshots captured, what could not be updated and what manual action it needs.

## What it doesn't do

- **Write specs.** Specs describe intent before implementation. That's [`etak-develop /spec`](../skills/spec.md). Docs describe what the code does after implementation.
- **Generate API reference from inline comments.** That's a build tool's job. The agent handles the human-curated layer above auto-generation.
- **Maintain a marketing site.** Docs are technical documentation. Marketing copy lives elsewhere.
- **Translate.** Localization is a separate workflow.
- **Silently mask code bugs.** When docs match the spec and code doesn't, the agent surfaces the discrepancy for the engineer to fix the code.

## How it operates

- **Match the project.** Tone, format, detail level, conventions exactly. Don't introduce a new style.
- **Code is the source of truth.** When docs and code disagree, update docs unless the code is the bug.
- **Screenshots are evidence.** Realistic state — real data, logged-in users, features in context. Empty placeholder data looks unprofessional.
- **Don't document the obvious.** A function named `getUserById(id: string): User` doesn't need a "Gets a user by ID" comment. Document the non-obvious — error conditions, side effects, performance, edge cases.
- **Changelog entries are for users.** Translate engineering language to user-visible behavior.
- **When you can't capture screenshots**, describe exactly what's needed for someone to capture manually.

## How to invoke

Ask in plain language. Examples:

```
Update the docs for the notification system PR.
```

```
Run a documentation audit — find drift between docs and code.
```

```
Refresh screenshots for the settings UI.
```

```
Document the new payments API.
```

The agent reads the relevant code (and spec/story when `etak-develop` is installed), detects conventions, updates each affected surface, captures screenshots when possible, and reports back with what was updated and what needs manual action.

## Tips

- **Run after a feature ships.** A doc page written before the implementation lands is documentation written against the spec. After landing, the agent can check it against the actual code.
- **Pair with releases.** A release without doc updates is a release that ships with stale documentation. Dispatch the tech-writer when [release-engineer](release-engineer.md) cuts a tag.
- **For drift-correction sweeps, scope to a doc set.** "Audit `docs/api/`" is more tractable than "audit all docs."
- **Trust the manual-action list.** When the agent reports "could not update X," that's the work it really cannot do alone — usually because the app isn't running or auth state isn't available.

## Related

- [Deliver overview](../deliver.md) — where tech-writer fits.
- [`/docs`](../skills/docs.md) — the interactive alternative.
- [release-engineer](release-engineer.md) — pairs naturally with tech-writer at release time.
- [reviewer](reviewer.md) — when reviewing PRs, often surfaces docs that need updates the tech-writer can do.
