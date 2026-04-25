---
name: adr
description: >
  Create architecture decision records — immutable-after-acceptance records of
  hard-to-reverse technical decisions. Internal skill called by spec and
  architect; never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# ADR

You help create and update architecture decision records. An ADR captures a
*hard-to-reverse* technical decision so that a future reader can understand what
was chosen, why, and what it commits the project to.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## When to Write an ADR

Not every decision is an ADR. Use the hard-to-reverse test:

**Write an ADR when:**
- Swapping the decision later would cost weeks or more
- The decision affects how the team writes code (framework, data model, protocol)
- The decision closes off options (choosing one vendor over another, one approach
  over another)
- A future reader will ask "why did we do it this way?"

**Don't write an ADR when:**
- The decision is easily reversed (variable name, file layout, internal helper
  shape)
- The decision is one of many similar small ones (that's just code review
  territory)
- The answer is obvious from the code or industry practice

## What Makes a Good ADR

- **Short** — ADRs are read, not skimmed. Keep them under a page when possible.
- **Context before decision** — the forces that drove the choice matter more than
  the choice itself.
- **Alternatives considered** — at least two, ideally three. If there was no
  alternative, it wasn't a decision.
- **Consequences, good and bad** — what we're committing to. This is the part
  people actually care about in retrospect.
- **Dated implicitly by the filename** — ADR-NNN is sequenced; that tells you the
  order.

## Moves

### Capture a new ADR

- **Force the decision into one sentence.** "We will use passport.js for OAuth
  provider abstraction" is a decision. "Authentication strategy" is not.
- **What context forced this?** Requirements, constraints, prior decisions.
- **What alternatives were considered?** Name them. Say why they were rejected.
- **What does this commit us to?** Library ownership, upgrade paths, expertise
  investment, closed-off options.

### Number the ADR

ADRs are numbered sequentially per project. Check
`docs/development/adrs/` for existing ADRs, find the highest `adr-NNN-*`
filename, and use the next integer. Zero-pad to three digits.

### Update an existing ADR

ADRs in `proposed` state may be revised freely. ADRs in `accepted` state are
effectively immutable — if the decision changes, write a *new* ADR that
supersedes the old one.

**Status lifecycle:** `proposed → accepted (→ deprecated | superseded)`

- proposed → accepted: consensus reached or decision ratified
- accepted → deprecated: decision no longer applies but no replacement (rare;
  usually implies the feature went away)
- accepted → superseded: a new ADR replaces this one; set `superseded_by` on
  this ADR

### Write the artifact

Filename: `adr-NNN-kebab-case-slug.md`. Write to `docs/development/adrs/`.
Include frontmatter with `name` (starting with "ADR-NNN:"), `type: adr`,
`status: proposed`, `for` (project slug), `superseded_by: null`. Body:

1. **Context** — the forces
2. **Decision** — what we chose, in plain language
3. **Alternatives considered** — what else; why rejected
4. **Consequences** — what this commits to; good and bad

Always show before writing.

## Failure Modes

- ADR for a reversible decision — noise.
- ADR without alternatives — a memo, not a decision record.
- ADR without consequences — the most valuable section, missing.
- Amending an accepted ADR instead of superseding — breaks the audit trail.
  Write a new one.
- ADR that's really a spec — decision records are short; if it's a page of
  design, it's a spec.
