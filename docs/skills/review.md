# review

**Stance:** rigorous senior colleague. **Purpose:** read a diff or PR carefully and surface what matters — problem fit, simplicity, critical zones, architectural boundaries, security, test integrity.

Review is the move where a change gets read carefully by someone who isn't the author. The author has been deep in the problem; the reviewer brings fresh eyes and structural skepticism. Done well, review catches the things the author couldn't see and confirms the things they got right. Done poorly, it produces a wall of nitpicks nobody acts on.

This skill is the collaborative version of review. The autonomous counterpart is the [reviewer agent](../agents/reviewer.md).

## When to reach for it

- **You finished implementing and you're about to push.** Self-review catches the missing AC coverage, the function that grew too long, the assertion that got weakened. Cheaper than the reviewer catching it.
- **You're reviewing someone else's PR.** Walk it across the six dimensions. Lead with what matters; skip what doesn't.
- **Reviewers came back with comments and you're triaging the response.** Group by theme, draft specific responses, surface disagreements rather than swallow them.
- **You want a structured second opinion before pushing a risky change.** Run the dimensions yourself even if no PR exists yet.

Trigger phrases: *review this PR*, *self-review*, *check my code*, *am I done*, *peer review*, *review feedback*, *is this ready*, *code review*.

## How it behaves

Review reads beyond the diff. The diff shows what changed, but boundary and security analysis need surrounding context. So review opens the changed files and reads them in full, traces dependency directions, and sometimes follows imports to where data flows.

The sharpest move review makes is calibrating intensity to the change. A one-line CSS tweak doesn't need a security deep-dive; a new auth endpoint does. A reviewer who treats every PR with maximum scrutiny is functionally indistinguishable from a reviewer who treats none with care.

### The six dimensions

**Problem fit.** Does the implementation address the ACs? Are there ACs not covered, or behaviors implemented that aren't in any AC? Does the approach match the spec? This is the highest-value dimension. A well-crafted, secure, fast implementation that solves the wrong problem is waste.

**Simplicity.** Over-engineering, unnecessary indirection, premature abstraction, cleverness over clarity. The question isn't "is this code complex?" — the question is whether the complexity is *essential* (inherent to the problem) or *accidental* (introduced by the implementation).

**Critical zones.** Areas of inherent complexity that deserve careful human attention: concurrency, transactions, raw SQL, environment-gated behavior, cross-module coupling, new dependencies. Each gets a 1–3 sentence flag explaining why it deserves attention.

**Architectural boundaries.** Interface clarity, single responsibility, dependency direction, abstraction consistency. Dependency direction is the highest-value check — wrong-direction dependencies create coupling that's expensive to fix later.

**Security.** Calibrated to the change. High-risk areas (auth, user input, DB queries, crypto, file system, command execution, new dependencies) get scrutiny; low-risk changes (CSS, internal refactors, test-only) get a quick pass.

**Test integrity.** Coverage (are we testing what we should?), intent (do tests test what they claim?), honesty (over-mocking, weakened assertions, deleted assertions, tautological tests), value (testing behavior, not implementation details).

### Silence is valid

If a dimension is clean, review says so in one line and moves on. Manufactured concerns to seem thorough are worse than a brief "no concerns here." Findings are calibrated by severity — high (must address before merge), medium (should address), low (consider). The verdict is specific, not generic.

## How to use it well

**Self-review before pushing.** The pre-flight that catches the missing AC coverage is cheaper than a review round. The dimensions are your checklist.

**For PR review, lead with severity.** Engineers triage by what blocks merge. A wall of mediums and lows looks the same; a high finding leads.

**Read the surrounding code, not just the diff.** Boundaries and security live in context. The diff alone shows you what changed but not what it touches.

**Calibrate to the change.** Don't apply the same depth to a CSS tweak as to a new auth flow. Calibration is a feature.

**Don't duplicate linting.** If ESLint or ruff would flag it, the project already does. Don't waste review attention on what tooling catches.

**Surface disagreement rather than swallow it.** When you disagree with a reviewer's comment, draft the response and discuss. Silently changing the code or silently ignoring the comment is worse than either alternative.

## What review does NOT do

- **Decide acceptance.** Review surfaces findings. The author and the team decide what to act on.
- **Replace the reviewer agent for autonomous PR review.** When you want findings posted to the PR autonomously, dispatch the [reviewer agent](../agents/reviewer.md).
- **Run automated tests.** Review reads code and tests; it doesn't execute them. CI does that.
- **Refactor.** When review surfaces refactoring opportunities, those are findings, not actions. Refactors live in `etak-develop`'s `build` skill.

## Common failure modes

- **Manufactured concerns.** "Have you considered X?" when X is not actually a concern. Wastes time and erodes trust.
- **Wall-of-nitpicks.** Twenty small style suggestions for one PR. Better: one comment with the pattern, or trust the linter.
- **Ungrounded findings.** "This might have security implications" with no specifics. If the finding isn't grounded in a file and line, it isn't actionable.
- **Reviewer-as-author.** Rewriting the change in comments. The author owns the implementation; the reviewer surfaces concerns.
- **Skipping the AC scan.** The single most valuable check — does the diff cover each AC? — gets skipped when reviewers go directly to nitpicks.

## Transitions

- A finding is a real bug → [**etak-develop /build**](../skills/build.md) (when installed) to fix it
- Review reveals testing gaps → [**verify**](verify.md) for AC-by-AC coverage analysis
- Review surfaces a security concern that needs deeper analysis → [**secure**](secure.md)
- The change touches infrastructure → [**operate**](operate.md) to assess blast radius
- Documentation drift surfaces → [**docs**](docs.md)

## Related

- [Skills index](README.md)
- [Reviewer agent](../agents/reviewer.md) — the autonomous version of this skill.
- [Deliver overview](../deliver.md) — context on how the six deliver skills fit.
- [verify](verify.md) — the AC-driven counterpart; review checks "is this good," verify checks "is this done."
