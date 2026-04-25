# Spec

**Role:** technical specification for a project, epic, or story. Grounded in the actual codebase, not hypothetical architecture.

## Why it exists

A spec describes how to build something. It sits next to a work item (a project, epic, or story) and answers the question the work item leaves open: what is the design?

The defining quality of an Etak spec is that it is *grounded*. It references the real codebase. It cites file paths, existing patterns, and the actual abstractions you'll extend or replace. A spec written without reading the code is an opinion piece. If you haven't looked at how similar features are implemented today, you can't write the spec.

Specs surface alternatives and open questions honestly. The "alternatives considered" section is where a later reader learns why this approach was chosen over others. The "open questions" section keeps the spec from pretending to settle things that aren't settled. Both sections do work.

Specs and ADRs are complementary. The spec describes the design; the ADR captures the hard-to-reverse decisions inside it. A good spec typically produces one or two ADRs. A spec that produces none is either routine enough not to need the spec, or hiding the real decisions.

## When to create one

- When a story's implementation isn't obvious from the ACs and a quick look at the codebase.
- When a project needs a coordinating design that multiple workstreams will read.
- When an epic introduces a design pattern that hasn't been used before in this codebase.
- When the architect agent is commissioned to produce a design proposal.

## Schema

```yaml
---
name: "OAuth2 token management design"
type: spec
status: draft  # draft | review | approved | superseded
for: project-oauth2-migration
adrs: []
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Descriptive title for the design. |
| `type` | yes | Always `spec`. |
| `status` | yes | Current lifecycle state. |
| `for` | yes | Slug of the project, epic, or story this spec describes. |
| `adrs` | yes | Slugs of ADRs produced from this spec. Empty list if none. |

## Status lifecycle

```
draft  ─────▶  review  ─────▶  approved  ─────▶  superseded
```

- **draft** — being written or revised.
- **review** — ready for others to read.
- **approved** — consensus reached; implementation may start.
- **superseded** — a new spec replaces this one. Link the successor in the body.

Approved specs evolve. Approval is a point-in-time decision, not a freeze. When material changes happen after approval, call them out in the body so a future reader sees what shifted.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `for` | [Project](project.md), [Epic](epic.md), or [Story](story.md) |
| Outgoing | `adrs` | [ADR](adr.md) |

## Body sections

A spec uses seven sections:

1. **Context** — what problem, what constraints, why now.
2. **Current state** — what exists in the codebase today, with concrete file references.
3. **Proposed approach** — the design, grounded in and referring to the current state.
4. **Non-functional requirements** — performance, reliability, security, operability.
5. **Alternatives considered** — what was rejected and why.
6. **Open questions** — what still needs resolution.
7. **Risks** — what could go wrong.

## Example

```markdown
---
name: "OAuth2 token management design"
type: spec
status: review
for: project-oauth2-migration
adrs:
  - adr-001-use-passport-js-for-oauth
---

## Context

The OAuth2 migration needs a single place that issues, stores, and verifies
sessions across multiple identity providers. The current session model is
password-specific; it has to become provider-agnostic without changing the
public session API consumed by downstream services.

## Current State

- `src/auth/session.ts` — current session factory; reads from `users` table
- `src/auth/middleware.ts` — the session verification middleware consumed by
  every authenticated route
- `src/auth/password.ts` — password-specific login path; to be removed by end
  of project
- `src/db/migrations/` — Knex migrations; latest is `0031_add_user_timezone`

## Proposed Approach

Introduce `src/auth/providers/` with one file per identity provider
(`google.ts`, `github.ts`) implementing a shared `Provider` interface. The
session factory accepts an authenticated `ProviderResult` and produces a
session via the existing factory, keeping `session.ts`'s public API unchanged.

Database changes: new `identities` table (user_id, provider, provider_user_id,
created_at) joined to `users`. Users table is unchanged. Migration `0032`.

The callback route `GET /auth/:provider/callback` dispatches on `:provider`
through the `Provider` interface; no provider-specific code in the router.

## Non-Functional Requirements

- Callback latency under 500ms P95
- Session issuance is idempotent; retrying a callback with the same code must
  not duplicate identities
- No OAuth state leaves the server unencrypted

## Alternatives Considered

- Per-provider routes with no abstraction. Rejected: adding a third provider
  would be a duplicative rewrite.
- Third-party auth service (Auth0, Clerk). Rejected: enterprise data-residency
  requirements preclude outsourcing the session layer.

## Open Questions

- Should we support account linking (same user, multiple providers) in this
  project, or defer? Leaning defer; calls out in `out of scope` of the parent
  project.

## Risks

- Existing password users need a migration path when password login is removed.
  Mitigation: parallel run; password path stays available until two providers
  are live.
```

## Useful actions

### Create

Through [spec](../skills/spec.md), which first reads the code it's about to describe. Never write a spec without grounding it in actual files.

### Produce ADRs

Hard-to-reverse decisions inside a spec become ADRs. The spec skill creates the ADR and links it back under `adrs`. See [ADR](adr.md) for the hard-to-reverse test.

### Assess

```
Assess this spec.
```

[Assess](../skills/assess.md) stress-tests the design against the codebase, calls out alternatives the spec missed, and surfaces unstated assumptions.

### Update status

Move to `review` when ready for others. Move to `approved` when consensus is reached. If the design changes materially later, write a new spec and mark the old one `superseded`.

## Tips

- **Read the code first.** A spec that doesn't cite file paths probably hasn't earned its claims.
- **Scope tightly.** `for` points at one work item. Specs that try to describe the whole system are too abstract to be useful.
- **Name alternatives.** At least one rejected option. Without it, the reader can't evaluate the decision.
- **Surface open questions honestly.** Better to flag the uncertainty than to paper over it.
- **A spec that's all open questions is a spike.** Convert it or resolve the questions first.
- **Don't over-spec routine work.** If a grep and a short ADR would settle the design, the spec is ceremony.

## Related

- [ADR](adr.md) — hard-to-reverse decisions that come out of a spec.
- [Project](project.md), [Epic](epic.md), [Story](story.md) — work items a spec is `for`.
- [Spike](spike.md) — use this when the spec would be all open questions.
- [Spec skill](../skills/spec.md) — draft a grounded design.
- [Assess skill](../skills/assess.md) — stress-test a spec.
- [Artifacts overview](README.md)
