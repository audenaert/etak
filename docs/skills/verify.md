# verify

**Stance:** rigorous QE pairing with the engineer. **Purpose:** confirm a story is actually done — not "the code compiles" but "the user-visible behavior the story promised exists, is tested, and survives the edge cases."

Verify is the move where someone asks the question that cuts through optimism: *if the implementation broke tomorrow, would any test fail?* Review checks "is this good." Verify checks "is this done." They are different passes, both useful.

This skill is the collaborative version of verification. The autonomous counterpart is the [quality-engineer agent](../agents/quality-engineer.md).

## When to reach for it

- **The story is implemented and you're about to mark it done.** Verify walks each AC and names what satisfies it. The honest answer is sometimes "almost done."
- **A PR is open with an AC checklist.** Verify confirms each item independently rather than trusting the author's checkmarks.
- **You suspect coverage isn't where the AC lives.** A unit test passing for behavior that needs an integration test is false confidence. Verify catches that.
- **A high-stakes story is shipping.** The cost of one missed AC outweighs the cost of an explicit verification pass.

Trigger phrases: *verify this story*, *is this done*, *check AC coverage*, *did we test it*, *walk the ACs*, *story verification*, *ready to close*.

## How it behaves

Verify assumes nothing about coverage. It walks each acceptance criterion in turn and names what satisfies it — a specific test (file + name), a specific behavior in the diff, or nothing yet. Each AC is a separate check. "All covered" without naming the test for each AC is suspect.

The sharpest move verify makes is noticing the gap between "the test passes" and "the AC holds." A test that exercises a code path without asserting the user-visible outcome is not coverage. A test that mocks the thing the AC actually depends on is not coverage either.

### The three moves

**Walk the ACs.** For each acceptance criterion, three honest possibilities: a specific test (cite file + test name), a specific behavior in the diff (paired with a manual verification step or pending test), or nothing yet (the AC is not covered, say so).

**Check the test layer.** Tests at the wrong layer give false confidence. UI behavior needs E2E or component tests, not unit. Service contracts need integration, not mock-heavy unit. Verify mentally mutates the implementation and asks: would the test catch it? If the answer is no, the test isn't coverage.

**Identify gaps.** Output a structured report: ACs satisfied (with evidence), ACs partially covered (what's covered and what's not), ACs not covered (what's missing and what kind of test would cover it), concerns beyond the ACs (edge cases, race conditions, ops concerns the story didn't anticipate).

The verdict: **done**, **almost done** (specific gaps), or **not done** (material gaps). Specifics over labels.

## How to use it well

**Run verify after build, not as part of build.** The author's coverage assessment is biased. Verify is the second pass that asks the structurally skeptical questions.

**Be specific about the test.** "AC #2 — verified by `notification-flow.spec.ts:12`" is coverage. "AC #2 — covered by tests" is hand-waving.

**Notice when the test mocks the AC dependency.** If the AC is "emails are sent on item change" and the test mocks the email service, the mock invalidates the test as coverage of that AC. Name what's mocked.

**Pick edge cases plausibly.** Defensive lists of every conceivable edge case are noise. Pick the ones plausible for this code: empty input, boundary values, concurrent calls, network failures — whichever apply.

**When verify finds a coverage gap, hand off.** Verify identifies; `etak-develop`'s `test` writes. The handoff is the point.

## What verify does NOT do

- **Write tests.** When the gap is missing test coverage, hand off to [`etak-develop`'s `test` skill](../skills/test.md) (when installed). The autonomous [quality-engineer agent](../agents/quality-engineer.md) *can* write tests; the verify skill identifies and delegates.
- **Refine ACs.** If ACs are unclear or unverifiable as written, surface that — but refining the story is `etak-develop`'s `story` skill.
- **Code review.** Verify checks "is this done." Code review checks "is this good." They are different passes; both run, but [`review`](review.md) handles the second.
- **Sign off on releases.** Verify confirms a story; release readiness requires [`ship`](ship.md).

## Common failure modes

- **Coverage on trust.** Accepting "all ACs are covered" without walking each one. Cause: the report sounds confident. Fix: name the test for each AC; if you can't, the AC isn't covered.
- **Wrong-layer comfort.** A passing unit test for behavior that needs an integration test. Cause: the test exists, so it must count. Fix: ask whether the test asserts the user-visible outcome at the right layer.
- **Mock-it-and-pass.** The test mocks the thing the AC actually depends on. Cause: the mock makes the test fast. Fix: name what's mocked; if the AC depends on it, the mock invalidates the test as coverage.
- **Edge-case bingo.** Listing every conceivable edge case without judgment. Cause: defensive verification. Fix: name the cases plausible for *this* change; skip the rest.
- **Verification as gatekeeping.** Refusing to mark anything done because something might be wrong. Cause: confusing rigor with paranoia. Fix: be specific about what's missing; if nothing is, say done.

## Transitions

- An AC has no test → [**etak-develop /test**](../skills/test.md) (when installed) to write one
- An AC is unverifiable as written → [**etak-develop /story**](../skills/build.md) to refine it
- Verification reveals a missing assumption → [**etak-discovery /sound**](sound.md) to surface it
- All ACs satisfied → [**ship**](ship.md) for release readiness
- Verification surfaces a security concern → [**secure**](secure.md)

## Related

- [Skills index](README.md)
- [Quality-engineer agent](../agents/quality-engineer.md) — the autonomous version of this skill.
- [Deliver overview](../deliver.md) — how the six deliver skills relate.
- [review](review.md) — the structurally adjacent move; review checks code quality, verify checks AC coverage.
