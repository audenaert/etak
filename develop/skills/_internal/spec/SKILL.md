---
name: spec
description: >
  Create and update technical specifications — grounded design documents for a
  project, epic, or story. Internal skill called by the spec user skill and by
  architect agent; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep, Bash
---

# Spec

You help create and update technical specifications. A spec is *grounded*: it
references the actual codebase, not a hypothetical one.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

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

## Moves

### Capture a new spec

Always read the code first. Before writing prose, at minimum:

- Grep for the subsystems the spec touches
- Read the top of each relevant file
- Check how similar features are implemented today
- Note the testing pattern used by neighbors

Then draft the spec through the standard sections:

1. **Context** — what problem, what constraints, why now
2. **Current state** — what exists today, with concrete file references
3. **Proposed approach** — the design, referring to the current state
4. **Non-functional requirements** — performance, reliability, security,
   operability
5. **Alternatives considered** — what was rejected and why
6. **Open questions** — what still needs resolution (flag for assess)
7. **Risks** — what could go wrong

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
to swap), read [../adr/SKILL.md](../adr/SKILL.md) and create the ADR. Link it in
the spec's `adrs` field.

### Write the artifact

Generate a kebab-case filename. Write to `docs/development/specs/`. Include
frontmatter with `name`, `type: spec`, `status: draft`, `for` (project/epic/story
slug), `adrs: []`. Body uses the seven sections above. Always show before
writing.

## Failure Modes

- Spec without codebase reading — an opinion piece, not a spec. Push back.
- Spec that duplicates the story AC — doesn't add value. Specs explain *how*,
  stories explain *what and why*.
- Spec without alternatives — reader can't evaluate the decision.
- Spec with every question open — useful as a spike, not as a spec. Either
  resolve the questions or convert to a spike.
- Spec for something a grep + short ADR would handle — don't over-spec routine
  work.
