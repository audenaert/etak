# ADR

**Role:** architecture decision record. Captures a hard-to-reverse technical decision so a future reader can understand what was chosen, why, and what it commits the project to.

## Why it exists

ADRs exist because code tells you *what* the system does but rarely *why* it does it that way. Six months from now someone will ask why the codebase uses passport.js instead of a custom OAuth layer, or why the session cookie is signed with HMAC instead of encrypted. The ADR is where the answer lives.

The defining quality of an ADR is that the decision is hard to reverse. Swapping the library later would cost weeks. Switching the data model would break every consumer. Choosing vendor A over vendor B closes off options. Reversible decisions (variable names, internal helper shapes, file layout) are not ADR material; they're code review territory.

ADRs are short. They get read, not skimmed. The most valuable section is "consequences": what this commits us to. That's the part people actually care about in retrospect, and the part most ADRs skip.

Once accepted, an ADR is effectively immutable. If the decision changes, you write a *new* ADR that supersedes the old one. This preserves the audit trail. Amending an accepted ADR breaks the record of what was thought at the time.

## When to create one

- When swapping the decision later would cost weeks or more.
- When the decision affects how the team writes code (framework, data model, protocol).
- When the decision closes off options (choosing one vendor over another, one approach over another).
- When a future reader will ask "why did we do it this way?" and the code won't answer.

## When not to create one

- The decision is easily reversed (variable names, internal helper shapes).
- The decision is one of many similar small ones. That's code review territory.
- The answer is obvious from the code or industry practice.

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

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Starts with "ADR-NNN:" where NNN is zero-padded sequence. |
| `type` | yes | Always `adr`. |
| `status` | yes | Current lifecycle state. |
| `for` | yes | Slug of the project this ADR belongs to. |
| `superseded_by` | no | Slug of the later ADR that replaces this one. |

ADRs are numbered sequentially per project. Check `docs/development/adrs/` for the highest existing `adr-NNN-*` filename, use the next integer, zero-padded to three digits.

## Status lifecycle

```
proposed  ─────▶  accepted  ─────▶  deprecated
                  accepted  ─────▶  superseded
```

- **proposed** — drafted, not yet ratified. May be revised freely.
- **accepted** — consensus reached or decision ratified. Effectively immutable.
- **deprecated** — the decision no longer applies but has no replacement. Rare; usually means the feature went away.
- **superseded** — replaced by a later ADR. Set `superseded_by` to the new ADR's slug.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `for` | [Project](project.md) |
| Outgoing | `superseded_by` | another [ADR](adr.md) |
| Incoming | Specs link via `adrs` | this ADR |

## Body sections

An ADR uses four sections:

1. **Context** — the forces that drove the decision.
2. **Decision** — what we chose, in plain language, in one paragraph.
3. **Alternatives considered** — at least two, ideally three. Named, with reasons for rejection.
4. **Consequences** — what this commits to. Good and bad.

## Example

```markdown
---
name: "ADR-001: Use passport.js for OAuth provider abstraction"
type: adr
status: accepted
for: project-oauth2-migration
superseded_by: null
---

## Context

The OAuth2 migration needs to support Google at M1 and GitHub at M2, with SAML
expected in the next project. The team has modest OAuth expertise and tight
time pressure. Whatever we choose becomes the abstraction every provider flows
through; swapping it later would require rewriting every provider integration
and the session hand-off.

## Decision

We will use passport.js as the OAuth provider abstraction. The `passport-google-oauth20`
and `passport-github` strategies handle the first two providers. Passport's
strategy pattern becomes the internal `Provider` interface; the session factory
accepts the authenticated result and issues a session.

## Alternatives Considered

- **Roll our own provider abstraction.** Rejected: OAuth edge cases (PKCE,
  state parameter handling, token refresh) are not where we want to build
  expertise under this deadline.
- **Use a third-party auth service (Auth0, Clerk).** Rejected: enterprise
  data-residency requirements preclude outsourcing the session layer.
- **Use `openid-client` directly, without passport.** Rejected: gives us
  OIDC well but leaves us rebuilding the strategy-per-provider pattern ourselves.

## Consequences

- We commit to passport.js's strategy model and its plugin ecosystem for every
  future provider. SAML via `passport-saml` is the expected path.
- Upgrades to passport become project-scope work; the library is load-bearing.
- Existing team familiarity with Node middleware translates directly; no new
  framework learning curve.
- Passport's callbacks-first API requires wrapping in our async/await conventions;
  we'll add a thin adapter.
```

## Useful actions

### Create

Through [spec](../skills/spec.md) when the spec surfaces a hard-to-reverse decision, or through [plan](../skills/plan.md) when a project-level decision needs recording before detailed design begins.

### Update status

When consensus is reached, advance `proposed → accepted`. Don't linger in `proposed`; decisions benefit from being made.

### Supersede

When the decision changes, write a *new* ADR. Set `superseded_by` on the old ADR pointing at the new one. Don't amend accepted ADRs; that breaks the audit trail.

### Assess

```
Assess this ADR.
```

[Assess](../skills/assess.md) stress-tests the decision from multiple perspectives: operational, security, long-term flexibility, alternative framings.

## Tips

- **Short.** ADRs under a page when possible. If it's a page of design, it's a spec.
- **Decision in one sentence.** "We will use passport.js for OAuth provider abstraction." If you can't force it into one sentence, the decision isn't clear yet.
- **At least two alternatives.** If there was no alternative, it wasn't a decision.
- **Consequences, good and bad.** The most valuable section. Don't skip it.
- **Supersede, don't amend.** Accepted ADRs are immutable. If the decision changes, write a new one.
- **Let the number carry the date.** ADR-NNN ordering tells you the sequence; you don't need a date field.

## Related

- [Spec](spec.md) — where ADRs usually originate.
- [Project](project.md) — what an ADR is `for`.
- [Spec skill](../skills/spec.md) — drafts the spec and invokes ADR creation for hard-to-reverse decisions.
- [Assess skill](../skills/assess.md) — stress-test a decision.
- [Artifacts overview](README.md)
