# Spec

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create and update technical specifications. A spec is *grounded*: it
references the actual codebase, not a hypothetical one.

## Schema

```yaml
---
name: "OAuth2 token management design"
type: spec
status: draft  # draft | review | approved | superseded
for: project-oauth2-migration
adrs: []
diagrams:
  - kind: erd | sequence | class | state | architecture
    title: "..."
    anchor: "#..."   # heading anchor where the diagram lives in the body
---
```

Body sections (recommended):

1. **Context** — what problem, what constraints, why now
2. **Current state** — what exists in the codebase today, relevant files and patterns
3. **Proposed approach** — the design, grounded in existing code
4. **Alternatives considered** — what was rejected and why
5. **Non-functional requirements** — performance, reliability, security, observability, scalability, accessibility
6. **Adjacent opportunities** *(project-scale specs)* — roadmap work this design enables or simplifies
7. **Migration & evolution** *(when applicable)* — how the system evolves; data migration paths if schema or contracts change
8. **Open questions** — what still needs resolution
9. **Risks** — what could go wrong, likelihood, impact, mitigation

## What Makes a Good Spec

- **Grounded in the codebase** — file paths, existing patterns, actual
  abstractions. If you haven't read the code, you can't write the spec.
- **Scoped** — `for` points at a specific project, epic, or story. Specs that try
  to describe "the whole system" are usually too abstract to be useful.
- **Names alternatives** — the "alternatives considered" section is where the
  reader learns why this approach was chosen over others.
- **Surfaces open questions** — honest about what isn't settled yet.
- **Records consequences** — NFR implications, future flexibility, what this
  closes off.
- **Specs describe how, not what for whom.** Stories live in story artifacts (see [story.md](story.md)). A spec section labeled with a persona, formal ACs in a "Story N" structure, or a vertical slice of user value belongs in a story file, not the spec body.
- **Use cases vs stories.** A spec can — and often should — describe scenarios the design serves: "When a user creates an idea, the CLI translates the slug to an ID and..." That's design context. What it can't have is a "## Story 3 — Idea CRUD" section with persona + ACs treated as the story spec.
- **Diagrams are mandatory where they apply, embedded in narrative.**
  - **ERD** — required for any DB schema change. Place at the top of the Data Model section.
  - **Sequence diagrams** — required for any new cross-component flow. Place after the prose narrative of the flow.
  - **Class diagrams** — required for non-trivial new type hierarchies. Place where the types are introduced.
  - **State diagrams** — when a stateful entity is being designed. Place where the entity is introduced.
  
  All diagrams use Mermaid in fenced code blocks (no inline images). Embed in the section where they support the prose; do not collect them in a separate "Diagrams" section. Enumerate them in the frontmatter `diagrams: [...]` index so reviewers can verify presence and jump via anchor.

## Moves

### Capture a new spec

Always read the code first. Before writing prose, at minimum:

- Grep for the subsystems the spec touches
- Read the top of each relevant file
- Check how similar features are implemented today
- Note the testing pattern used by neighbors

Then draft the spec through the body sections listed in the Schema above.
Adjacent Opportunities and Migration & Evolution are project-scale; skip
them for story- or epic-scoped specs.

### Update an existing spec

Specs evolve during review. Common updates:
- Refine proposed approach based on feedback
- Add alternatives that came up in discussion
- Close open questions as they're resolved
- Link new ADRs under the `adrs` field

**Status lifecycle:** `draft → review → approved → superseded`

- draft → review: ready for others to read
- review → approved: consensus or decision to proceed; implementation may start
- approved → superseded: a new spec replaces this one; set `superseded` and link
  the successor in body

Approved specs are *not* immutable — they evolve — but the approval was a
point-in-time decision. Material changes after approval should be called out in
the spec body so a future reader understands what changed.

### Invoke ADR creation

When the spec introduces a hard-to-reverse decision (framework choice, data
model commitment, protocol selection, third-party integration that's expensive
to swap), read [adr.md](adr.md) and create the ADR. Link it in
the spec's `adrs` field.

### Write the artifact

Generate a kebab-case filename and write to the canonical path in the artifacts registry. Use the frontmatter and body sections from the Schema above. Always show before writing.

## Failure Modes

- Spec without codebase reading — an opinion piece, not a spec. Push back.
- Spec that duplicates the story AC — doesn't add value. Specs explain *how*,
  stories explain *what and why*.
- Spec without alternatives — reader can't evaluate the decision.
- Spec with every question open — useful as a spike, not as a spec. Either
  resolve the questions or convert to a spike.
- Spec for something a grep + short ADR would handle — don't over-spec routine
  work.
- **Spec missing required diagrams.** DB schema change without an ERD; cross-component flow without a sequence diagram. Verify presence via the frontmatter `diagrams` index.
- **Diagrams in a separate "Diagrams" section disconnected from prose.** Embed inline; the prose explains intent and the diagram makes it concrete.
- **Project-scoped decisions in ADRs.** Project-scoped technical choices belong in the spec's Decisions section. ADRs are for architectural impact (cross-project, hard-to-reverse). See [adr.md](adr.md) for the Decision Ladder.
- **Story-shaped content in the spec.** Persona blocks, formal AC tables, story-by-story sections — these belong in story artifacts. Demote into scenarios/use-cases that support the design narrative, or extract into proper story files (owned by tech-lead via the plan skill).
