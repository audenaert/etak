# quality-engineer

**Type:** autonomous agent. **Purpose:** verify a story is actually done — walk each AC, evaluate test coverage at the right layer, optionally write E2E tests to close gaps.

The interactive [`/verify`](../skills/verify.md) skill walks ACs collaboratively. The quality-engineer agent does the same work autonomously and posts a structured verdict to the PR. When a coverage gap exists and the gap is testable, the agent writes the missing test directly — failing tests that surface implementation gaps are the most valuable finding.

## When to reach for it

- **A PR is open with an AC checklist.** The agent walks each AC independently rather than trusting the author's checkmarks.
- **You want acceptance verification before merge.** The agent confirms each AC is satisfied, partially satisfied, or not satisfied — with cited evidence per AC.
- **A story has high stakes.** The cost of one missed AC outweighs the cost of an explicit verification pass. Dispatch the agent.
- **`developer` (when `etak-develop` is installed) just opened a PR.** It can dispatch the quality-engineer automatically.

## What it does

### Walk each AC

For each acceptance criterion, the agent names what satisfies it. Three honest possibilities:
- A specific test (cited by file + test name) that asserts the user-visible outcome
- A specific behavior in the diff, paired with a manual verification step or pending test
- Nothing yet — the AC is not covered

Each AC gets its own verdict: ✅ satisfied, ⚠️ partially satisfied, ❌ not satisfied, or ❓ can't determine (AC is ambiguous).

### Evidence sources

The agent ranks evidence strongest to weakest:
1. E2E test exercising the exact user flow
2. Integration test verifying end-to-end behavior without browser
3. Implementation code that clearly handles the scenario
4. Unit test covering the underlying logic
5. No evidence — gap

### Assess test quality

Beyond AC coverage:
- Are tests at the right layer? (E2E for user flows, unit for logic — not the reverse.)
- Are assertions meaningful? Not just `expect(result).toBeTruthy()`.
- Do tests verify behavior or implementation details?
- Are mocks invalidating coverage? If the AC depends on the integration that's mocked, the test isn't coverage.

### Stress-test the ACs

Look beyond the specified criteria for plausible-but-uncovered cases: edge cases the ACs didn't anticipate, implicit requirements (accessibility, performance, mobile), failure modes likely for *this* code. Pick cases plausible for this change; defensive bingo lists are noise.

### Close coverage gaps

For ACs marked ⚠️ or ❌ where the gap is missing test coverage (not missing implementation), the agent writes the E2E or integration test that verifies the AC. It runs the test. If it fails, that's a real finding — the implementation doesn't satisfy the AC.

### Post the verification

If a PR exists, the agent posts a structured comment with AC coverage, tests written, observations from stress-testing, and a clear verdict.

## What it doesn't do

- **Trust without walking.** Coverage claims are checked AC by AC. "All ACs are covered" without naming the test for each is suspect.
- **Refine ACs.** When ACs are unclear or unverifiable as written, the agent surfaces that — but refining the story is `etak-develop`'s `story` skill.
- **Code review.** Verification checks "is this done." Code review checks "is this good." [reviewer](reviewer.md) does the latter.
- **Sign off on releases.** Verification confirms a story; release readiness is [release-engineer](release-engineer.md)'s domain.

## How it operates

- **Each AC is a separate check.** Don't lump into "all covered."
- **Cite the specific test (file + name).** Vague claims aren't evidence.
- **Test layer matters.** UI behavior needs E2E, service contracts need integration; mock-heavy unit tests pretending to cover either is not coverage.
- **E2E tests are the strongest evidence.** When present, the agent leads with them.
- **Write the missing test directly.** When the gap is coverage, not implementation, the agent closes it. Failing tests are the most valuable finding.
- **Be concise.** Green/yellow/red per AC, brief evidence, clear verdict. No essays.

## How to invoke

Ask in plain language. Examples:

```
Verify story 4.3 against PR #142.
```

```
Walk the ACs on this PR. Write any missing tests.
```

```
Is this story done?
```

The agent loads the story (or PR description as fallback when `etak-develop` isn't installed), walks each AC, evaluates evidence, optionally writes missing tests, and posts the verdict.

## Tips

- **Pair with [reviewer](reviewer.md).** Reviewer covers code quality across six dimensions; quality-engineer covers AC coverage. Both passes are useful; they look at different things.
- **Trust failing tests written by the agent.** If the agent writes the missing test and it fails, the implementation has a real gap — not a test problem.
- **Treat "partially satisfied" as a signal.** When an AC is partially satisfied, the AC may be too broad; refining it can be cheaper than expanding the implementation.
- **Run before release.** Verification is the last meaningful check before [release-engineer](release-engineer.md) cuts a tag.

## Related

- [Deliver overview](../deliver.md) — where quality-engineer fits.
- [`/verify`](../skills/verify.md) — the interactive alternative.
- [reviewer](reviewer.md) — code-quality counterpart.
- [release-engineer](release-engineer.md) — what runs after verification clears.
