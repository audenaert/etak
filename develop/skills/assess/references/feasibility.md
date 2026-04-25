# Feasibility

Feasibility is a *grounded* assessment: what exists, what must change, where
the risks are. **Read the code before writing the assessment.** Without it,
every dimension below is a guess.

## Running the assessment

### 1. Load the target

The work item, its parent and siblings, any linked spec, any discovery
context. You want the shape of the thing and the shape of what surrounds it.

### 2. Read the codebase

Use Glob, Grep, and Read to understand:

**What exists today:**
- Which files, modules, components are relevant?
- What patterns does the codebase use for similar functionality?
- What abstractions can this work build on?
- What tests already exist for related functionality?

**What must change:**
- Which specific files need modification?
- Schema changes? API contract changes?
- Tests that need updating?

**What doesn't exist yet:**
- New modules, components, infrastructure?
- Dependencies to add?
- Configuration or tooling to stand up?

### 3. Assess across dimensions

**Effort** — scope of change.
- How many files modified vs. created?
- Well-trodden path (existing patterns) or novel ground?
- Where's the boilerplate vs. where's the real work?

**Risk** — where could things go wrong.
- Fragile, poorly-tested, or poorly-understood code paths?
- Performance-sensitive paths being modified?
- Shared components where changes could ripple?
- External dependencies with uncertain behavior?
- For each risk: severity + what mitigates it (spike, feature flag,
  incremental rollout).

**Dependencies** — what must happen first or in parallel.
- Internal: other work items, shared modules in flight
- External: services, APIs, libraries
- Infrastructure: migrations, deployment changes
- Knowledge: decisions someone needs to make first

**Complexity hotspots** — where the hard part is.
- Name the 1-3 genuinely-difficult areas vs. the stuff that's straightforward.
- For each hotspot: what makes it hard, is there a simpler alternative?
- Explain the *shape* of the complexity, not just the rating.

### 4. Look for a stepping stone

A stepping stone is a smaller piece of work that's **independently valuable
AND makes the larger work cheaper later**.

Examples:
- "Adding the schema + API endpoint now (useful for the admin dashboard)
  makes the user-facing feature much simpler later."
- "Refactoring auth middleware into a plugin architecture is valuable on
  its own (cleaner code, easier testing) AND it's the prerequisite for
  multi-provider support."

The stepping stone must be independently justifiable — if it's only useful
as a prerequisite, it's a dependency, not a stepping stone.

If no natural stepping stone exists, say so. **Don't manufacture one.**

### 5. Present the assessment

```
## Feasibility: [work item]

### Summary
[One paragraph: verdict + key finding]

### Codebase Landscape
- Files involved: [main ones + brief notes]
- Existing patterns to follow: [what precedent exists]
- New ground: [what has no precedent]

### Effort
[Specific file/module references]

### Risks
1. [Risk] — severity: H/M/L — mitigation: [specific suggestion]

### Dependencies
- [Dep] — status: [available / needed / uncertain]

### Complexity Hotspots
1. [Area] — why hard: [specific] — alternative: [if any]

### Stepping Stone
[If yes: what, value on its own, how it reduces future effort.
 If no: "No natural stepping stone — proceed directly."]

### Recommendation
[Proceed / modify / spike first / reconsider / start with stepping stone]
```

## Guidelines

- **Be specific.** "This is complex" is useless. "The middleware at
  `src/middleware/auth.ts:45-120` uses a pattern that doesn't support multiple
  providers — significant refactor needed" is useful.
- **Distinguish effort from risk.** Something can be high-effort + low-risk
  (lots of straightforward work) or low-effort + high-risk (one small change
  on a critical path).
- **Don't inflate risk to seem thorough.** If something is straightforward,
  say so. "Follows the pattern in `src/routes/users.ts` — low effort, low
  risk" is a valid finding.
- **If the work item is too vague to assess, say so** and explain what's
  missing. Don't guess.
- **Lead with surprises.** If the code reveals the planned approach won't
  work, that's the top of the report, not a footnote.
