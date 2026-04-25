# Stress-test

Stress-testing examines an artifact from perspectives the engineer hasn't
considered. It surfaces blind spots, challenges hidden assumptions, and —
when done well — produces specific, actionable fixes rather than vague
concerns.

Two modes:

- **Implementation mode** — specs, stories, epics, ACs. "Is this solid
  enough to build from? What will bite us?"
- **Risk mode** — projects and plans. "Assume this failed. Why?" (See also
  the pre-mortem reference in `/plan`.)

## Running the session

### 1. Load context

- The target artifact (full content)
- Connected artifacts, one level up and down
- Related findings, prior critiques, existing assumptions
- **The relevant codebase** — implementation-grounded critique is far more
  valuable than theoretical concern

### 2. Pick personas or frameworks

**Implementation mode — pick 2-3 personas:**

| Persona | Lens | Best for |
|---------|------|----------|
| **Architect** | Scalability, maintainability, coupling, boundaries | Specs, epics |
| **QE** | Testability, edge cases, AC gaps, regression risk | Stories, ACs, specs |
| **DevOps / SRE** | Deployability, observability, rollback, failure modes | Specs, projects |
| **Security** | Attack surface, input validation, auth/authz, data exposure | Stories with user input, API specs |
| **On-call engineer** | "It's 2am and this broke — can I fix it?" Debuggability, error messages, logging | Specs, error handling |
| **New team member** | Clarity, onboarding cost, implicit knowledge | All — catches tribal knowledge |
| **End user** | Does this solve the problem? Is it confusing? | Stories, ACs |

**Frameworks (pick 1-2):**
- Edge cases — empty states, boundary values, concurrent access, errors
- Scale stress — does it hold at 10x users, 10x data?
- Regulatory / ethical — compliance, legal, ethical considerations

**Risk mode** — skip personas. Frame for the engineer:
> "It's [realistic timeframe] from now. This project failed. What went wrong?"

Failure categories to probe (spend time on the ones most relevant):
- Technical — hardest problems, unfamiliar tech, performance
- Scope — is MVP actually minimal? Where will scope grow?
- Integration — where workstreams sync, what if dependencies slip
- Product — what if nobody uses it, were discovery assumptions real
- Organizational — key person unavailable, unconsulted decision-maker

### 3. Run the critique

For each persona or framework, produce a focused list:

- **The critique** — 3-5 specific points
- **Severity** — blocker / concern / suggestion (implementation mode) or
  likelihood × impact (risk mode)
- **Theme tag** — feasibility, testability, security, usability, etc.

Persona critiques should feel real: an architect and a QE notice different
things. Write from the perspective.

### 4. Watch for diminishing returns

If a new round mostly restates earlier findings, stop. Don't fill rounds
for ceremony. When all major themes are covered, synthesize.

### 5. Synthesize

Consolidated summary, ordered by severity. Include **strengths** — things
that held up under scrutiny. For each finding, quote the problematic part
and propose a specific fix.

### 6. Offer to fix

Refinement folds in here. Common fixes:

- Vague ACs rewritten:
  > ❌ "Errors are handled appropriately"
  > ✅ "When upload fails due to file size, user sees 'File exceeds 10MB
  > limit' and the file input resets"
- Implementation leakage removed:
  > ❌ "The API returns 200"
  > ✅ "User sees their profile after signing in"
- Missing alternatives added to a spec
- Missing NFR targets specified ("must be fast" → "p95 under 200ms")

### 7. Suggest next steps

- **Implementation + blockers** → "These need fixing before build. Apply?"
- **Implementation + clean** → "Solid — want to run a ready check?"
- **Risk + high risks** → "These need mitigation. Spawn spikes for the
  technical unknowns?"

## Guidelines

- **Be specific, not performatively negative.** Every point should be
  actionable.
- **Ground in the codebase.** "This won't work because `src/auth.ts:45` does
  X" beats "this might have security implications."
- **Strengths matter.** If something is genuinely strong, say so.
- **Don't critique what the engineer can't change.** Focus on choices and
  assumptions within their control.
- **One round is usually enough.** If a second round finds as many issues
  as the first, the artifact needs a rewrite, not more refinement.
- **Calibrate to maturity.** Draft specs don't need production-readiness
  standards.
