# ADR

Reference loaded by [develop:artifacts](SKILL.md). See [model.md](model.md) for the work graph: common fields, typed links, lifecycles, naming, readiness.

You help create and update architecture decision records. An ADR captures a
*hard-to-reverse* technical decision so that a future reader can understand what
was chosen, why, and what it commits the project to.

## The Decision Ladder

Defer decisions to the lowest level where they're relevant:

| Level | Home | Example |
|---|---|---|
| Implementation detail | developer + task body | naming, internal helper shape, algorithm choice |
| Project-scoped technical choice | spec | which Zod schema validator to use, table layout for one feature, error code mapping for one endpoint |
| Architecturally significant | ADR | substrate technology, cross-cutting framework, protocol commitment, hard-to-reverse vendor choice |

Each rung up the ladder adds review and audit overhead. Use the lowest rung that fits. ADRs are valuable precisely because they're rare — when every project-internal decision becomes an ADR, the audit trail loses signal.

## Schema

```yaml
---
name: "ADR-001: Use passport.js for OAuth provider abstraction"
type: adr
status: proposed  # proposed | accepted | deprecated | superseded
for: project-oauth2-migration
superseded_by: null
---
```

Body sections:
1. **Question** — what was being decided, framed as a question (one sentence)
2. **Decision** — what we chose, in active voice; first sentence restates the title's commitment in plain language
3. **Context** — the forces that drove the choice
4. **Alternatives considered** — what else; why rejected
5. **Consequences** — what this commits us to; good and bad

Filename: `adr-NNN-<kebab-slug>.md` where `NNN` is sequential project-wide.
Check existing ADRs (and unmerged branches) to find the next integer.

## When to Write an ADR

An ADR is for a *significant architectural decision*. Use this triage:

**Write an ADR when ALL of these are true:**
- Impact extends beyond the current project (cross-cutting infrastructure, shared patterns, technology stack)
- The decision is hard to reverse (weeks-or-more of work to undo)
- Future readers from other projects will need to know this

**Write a spec section, not an ADR, when:**
- The decision is project-internal (single epic, single service)
- The cost of revisiting later is bounded to this project
- It's a "how we built this feature" choice, not a "how we build things going forward" choice

**Push the decision into the task or the developer's judgment when:**
- It's an implementation detail with no architectural impact
- Reasonable engineers could pick either way and the team won't be affected six months later

## What Makes a Good ADR

- **Short** — ADRs are read, not skimmed. Keep them under a page when possible.
- **Title is the decision, not a topic.** Active voice, complete sentence: "Adopt PostgreSQL as the V0 substrate" not "Database choice." See Failure Modes for examples.
- **Question first.** A reader who knows neither the title nor the body should orient in two sentences: the Question and the Decision.
- **Decision before context.** ADRs get scanned as decision lookups. Lead with the answer; back it up with context.
- **Alternatives considered** — at least two, ideally three. If there was no
  alternative, it wasn't a decision.
- **Consequences, good and bad** — what we're committing to. This is the part
  people actually care about in retrospect.
- **Dated implicitly by the filename** — ADR-NNN is sequenced; that tells you the
  order.
- **Use diagrams when they clarify.** State diagrams for state-machine decisions; architecture comparison diagrams for option-A-vs-B alternatives. Place inline near the section they support — typically in **Decision** (state machine being adopted) or **Alternatives considered** (architectural comparison). Use Mermaid in fenced code blocks. ADRs do not need a frontmatter diagram index; keep them lightweight.

## Moves

### Capture a new ADR

- **Run the Decision Ladder triage first.** Is this an architectural decision (ADR), a project-scoped choice (spec), or an implementation detail (task)? Default to the lowest rung that fits.
- **Force the question into one sentence.** "Should we use a graph database or a relational database for the opportunity graph?" is a question. "Database stuff" is not.
- **Force the decision into one sentence.** "Adopt PostgreSQL as the V0 substrate" is a decision. "PostgreSQL" is not.
- **Verify Question and Decision pair.** The Decision should answer the Question. If they don't pair, sharpen one or both.
- **What context forced this?** Requirements, constraints, prior decisions.
- **What alternatives were considered?** Name them. Say why they were rejected.
- **What does this commit us to?** Library ownership, upgrade paths, expertise investment, closed-off options.

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

Filename: `adr-NNN-kebab-case-slug.md`, written to the canonical path in the artifacts registry.
Include frontmatter with `name` (starting with "ADR-NNN:"), `type: adr`,
`status: proposed`, `for` (project slug), `superseded_by: null`. Body order:

1. **Question** — what was being decided
2. **Decision** — what we chose; first sentence restates title in plain language
3. **Context** — the forces
4. **Alternatives considered** — what else; why rejected
5. **Consequences** — what this commits to; good and bad

Always show before writing.

## Failure Modes

- **ADR for a tactical project-internal decision.** Single-feature field drops, per-type enum value choices, single-spec validation strategy — these belong in the spec's Decisions section. Demote and consolidate.
- **Title that names a topic, not a decision.** "Authentication strategy" is a topic. "Use passport.js for OAuth" is a decision.
- **Question and Decision misaligned.** The Question should be the question the Decision answers. If they don't pair, sharpen one or both.
- **ADR for a reversible decision.** Noise.
- **ADR without alternatives.** A memo, not a decision record.
- **ADR without consequences.** The most valuable section, missing.
- **Amending an accepted ADR instead of superseding.** Breaks the audit trail. Write a new one.
- **ADR that's really a spec.** Decision records are short; if it's a page of design, it's a spec.
