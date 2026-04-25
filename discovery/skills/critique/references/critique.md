# Critique — Conversation Guide

This is the operational guide for running a critique session on discovery artifacts
(opportunities, ideas, objectives). The goal is to make thinking more rigorous, not
to shut ideas down.

## The Core Move

Look at the artifact through eyes that aren't the PM's. Every idea looks good to its
creator — your job is to adopt perspectives that would see it differently, apply it
critically, and report back what you find. That includes reporting what holds up.

## Persona-Based Critique

### Grounding Perspectives

Always consider these three. They represent the forces every product idea must
navigate:

- **Target end-user** — "Does this actually solve my problem? Would I use this?
  What would confuse me? What am I doing today instead?" Get specific about their
  context, constraints, and motivations.

- **Adjacent stakeholder** — "How does this affect me?" Think about people who
  don't use the feature directly but are impacted: managers who see the output,
  support teams who field questions, partners who integrate with it.

- **Resource-constrained builder** — "Can we actually pull this off? What's the
  hidden complexity? What are we underestimating? What will be painful to maintain?"

### Context-Specific Personas

Generate 3-5 personas based on the artifact's domain. These should represent
meaningfully different perspectives, not slight variations of the same user.

Good persona selection surfaces real tension. Look for:
- The reluctant adopter (someone who'd resist this change)
- The edge-case user (atypical usage patterns, extreme needs)
- The person who pays but doesn't use (or uses but doesn't pay)
- The expert who's seen similar attempts fail
- The downstream consumer of this feature's output

When critiquing from a persona, commit to the perspective. Write from their
viewpoint. Lead with what they'd actually think, feel, or struggle with — not
an abstract analysis of "what users might experience."

## Framework-Based Critique

Select 2-4 frameworks relevant to the artifact. Not every framework applies to
every artifact — choose the ones that would reveal the most.

### Assumption Audit

What must be true for this to work? List the critical dependencies. This overlaps
with sounding but comes at it from outside — "what does a skeptic see that the
team doesn't?" rather than "what are we betting on?"

### Second-Order Effects

If this succeeds, what happens next? What new problems emerge? Success often
creates its own challenges — scaling issues, support burden, user expectations
that expand beyond what was built, competitive attention drawn to your approach.

### Inversion

What would we do if we wanted this to fail? What would guarantee it doesn't
work? This reframe often reveals vulnerabilities that forward-looking analysis
misses. If "confuse users about what this does" is a failure recipe, ask how
close the current design comes to that.

### Competitive Response

If a competitor saw this, what would they do? Copy it, undercut it, make it
irrelevant with a different approach? How defensible is this? What's the moat?

### Edge Cases

What happens with extreme users (power users, brand-new users, users with
accessibility needs)? With unexpected data? With scale? With abuse? Edge cases
reveal how robust the thinking is.

## Running a Critique Session

### Setting Up

Load context before you start: read the target artifact, its connected artifacts
(one level up and one level down), and any existing assumptions or previous
critiques. Understand what you're examining and what the PM already knows about
its weaknesses.

Propose your persona and framework selections to the PM. Let them adjust — they
know their domain and may suggest better perspectives. This also signals what
the session will cover so there are no surprises.

### During Rounds

Each round uses one persona or one framework and produces one critique artifact.
For each round, produce:
- **3-5 specific findings** — concrete, not vague. "Users might not like it" is
  useless. "First-time users won't understand what 'semantic similarity' means
  in the results header" is actionable. Each finding has a `name` (the claim, one
  sentence) and a `body` (the reasoning).
- **Kind for each finding** — `risk` (something that could go wrong) or `strength`
  (something that holds up under scrutiny). Both belong in a balanced round.
- **Severity for each risk** — `fatal` (the idea cannot be saved in its current
  form), `serious` (must be addressed before the idea advances), `moderate` (real
  problem worth addressing, but the idea survives it), or `minor` (worth noting,
  unlikely to determine the outcome). Strengths do not have severity.
- **Theme tag** — feasibility, desirability, viability, usability, or a
  domain-specific tag.

### Tracking Coverage

After each round, mentally inventory which themes you've covered. When a new
round mostly restates concerns already raised in earlier rounds, that's
diminishing returns. Name it: "We're starting to circle back to the same
concerns. I think we've covered the main angles — want to synthesize, or is
there a specific perspective you'd still like to hear from?"

Do NOT keep generating criticism to fill rounds. Three thorough rounds that
surface real issues beats six rounds that repeat themselves.

### Calibrating to Maturity

Match your rigor to the artifact's stage. A draft idea in early exploration
should get "is this worth pursuing?" critique, not "will this scale to a million
users?" critique. Production-readiness standards applied to brainstorm-stage
thinking kills ideas that deserve development time.

Ask yourself: what decisions is this artifact being used to make right now?
Critique should serve those decisions, not hypothetical future ones.

## Synthesizing

After rounds are complete, consolidate:

- **Organized by severity** — fatal concerns first, then serious, then moderate,
  then minor
- **Include strengths** — what held up under scrutiny. This matters. It tells
  the PM what they can be confident about, and it makes the critique credible
  rather than reflexively negative.
- **Theme patterns** — if multiple rounds flagged feasibility concerns, that's
  a pattern worth naming explicitly
- **Assumptions surfaced** — critique often reveals assumptions the PM hasn't
  examined. Offer to promote them: "This critique surfaced three beliefs we
  haven't tested. Want me to promote those findings to assumption artifacts so
  we can track and test them?"

## Persisting the Round

A round is not done until it is written. Each round produces one critique artifact
in `docs/discovery/critiques/`. Rounds in the same session share a `session` slug
so they can be queried together later.

Call the internal `critique` skill to write the artifact. Pass the round's stance
(persona or framework), the perspective text, the target (`about_idea` or
`about_opportunity` — exactly one), the findings list, and the session slug if one
exists. Always show the artifact before writing.

The session-level `summary` field is your read on what the round found. Populate it
as part of synthesis — it is what someone reviewing the critique later will read
first.

## Promoting a Finding to an Assumption

When a risk finding articulates a testable belief, offer to promote it. The flow:

1. Confirm with the PM which findings to promote.
2. Call the internal `assumption` skill to create the assumption, passing the
   finding text, the originating critique slug, and the finding's name. The
   assumption's body records its origin.
3. Call the internal `critique` skill to update the originating finding —
   `status: addressed`, with `resolution` set to a sentence naming the new
   assumption slug.

This two-step move keeps both sides of the link in sync. The assumption knows where
it came from; the critique finding knows where it went.

Strengths do not graduate into assumptions. If a strength is worth tracking forward,
it belongs in the idea body, not as a new artifact.

## Moves for Specific Situations

### The PM pushes back with good reasoning

Acknowledge it. Update your assessment. A critique point that gets a strong
counterargument isn't a failure — it's the PM demonstrating they've thought it
through. Note it as a strength: "I raised X, and your reasoning about Y is
convincing. That concern is addressed."

### The PM gets defensive

Back off the adversarial stance. Lead with strengths for a round. Then return
to concerns from a different angle — often a persona perspective feels less
confrontational than a framework-based "what's wrong here."

### Everything looks fatal

Distinguish between "this specific approach has problems" and "this problem
space isn't worth pursuing." Usually the opportunity is sound but the idea
needs rethinking. Name that clearly: "The opportunity is real — users do
struggle with X. But this particular approach has significant feasibility
risks. Want to brainstorm alternative solutions?"

### Everything looks minor

That's a good signal if the critique was genuinely rigorous. Say so: "This
held up well. The concerns are manageable and none threaten the core approach.
I'd focus on [the most important minor point] and move forward."

## Signals That You're On Track

- Critique points are specific enough that the PM knows what to do about them
- You're finding genuine blind spots, not manufacturing objections
- Strengths are identified alongside weaknesses
- The PM says "I hadn't thought about that" at least once
- Rounds are getting shorter as you run out of new angles

## Signals That Something Is Off

- Every point is "fatal" — you're probably being too harsh or applying the
  wrong standard. Recalibrate to the artifact's maturity level
- Every point is "minor" — you may be staying in safe territory. Push into
  the uncomfortable questions: what if the core premise is wrong?
- The PM is disengaging — the critique may feel like an attack rather than
  a collaboration. Reset the stance, lead with strengths
- You're on round five and still finding new concerns — either the artifact
  has deep problems (name that) or you're generating noise (stop)

## Where This Leads

After critique, the natural next moves are:
- **Sound** — go deeper on assumptions the critique surfaced
- **Explore** — brainstorm alternatives if fatal concerns suggest rethinking
- **Prioritize** — rank the surfaced assumptions for testing
- Or simply move forward with confidence if the critique confirmed the approach
