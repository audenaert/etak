# test

**Stance:** rigorous. **Purpose:** strategy-first testing. Plan from ACs, write at the right layer, run and interpret, debug flakes and ordering issues.

Test is the testing partner in the inner loop. Where [build](build.md) pairs on implementation end-to-end, test goes deeper on the testing question: what needs covering, at what layer, and why. It reads the conventions in the codebase before it writes a single test. Your tests should look like they belong.

Test covers the developer inner loop. Adversarial integrity review (are these tests honest, do they match intent, do they add value) is QE territory and will live in the planned `deliver` plugin.

## When to reach for it

- **Planning tests from ACs.** A story has acceptance criteria and you want the test specification before writing code.
- **Writing tests for a specific change.** The story or change is in front of you and you need the cases, at the right layer, in the project's style.
- **Running and interpreting.** Tests are failing and you want to know whether the code is wrong, the test is wrong, or the suite is flaky.
- **Debugging test pathologies.** A test passes alone and fails together. A test passes locally and fails in CI. Something is intermittent and nobody trusts the suite.

Trigger phrases: *write tests*, *test this*, *TDD*, *test plan*, *plan tests from ACs*, *run tests*, *flaky test*, *debug test*, *ordering dependency*, *passes alone but fails together*, *what tests do we need*.

## How it behaves

Test reads the test conventions first: framework (Jest, Vitest, pytest, RSpec, Go test, Cargo test), file organization (`__tests__/`, `*.test.ts`, `tests/`, `spec/`), patterns (naming, setup, teardown), infrastructure (test DBs, fixtures, containers), and existing suites (smoke, integration-only, feature markers).

If the project uses `describe`/`it`, test doesn't write `test()`. If fixtures live in `conftest.py`, test doesn't invent a new pattern. The sharpest move is asking what could break and whether this test would catch it. A test that passes forever regardless of code changes is noise.

### Plan: derive tests from ACs

When a story has acceptance criteria, those ACs are the test specification. For each AC, test derives one or more concrete cases and marks them: happy path, edge case, or gap when the AC is ambiguous. It recommends the layer (unit, integration, API, E2E) and adds supplementary cases for error paths, boundary conditions, and regression guards.

Gaps don't get guessed. An ambiguous AC is a refinement finding, handed back to [assess](assess.md) or resolved directly with the engineer.

### Write at the right layer

Unit for logic, integration for boundaries, API for contracts, E2E for critical user flows. Each layer has a cost. Test picks the cheapest layer that actually exercises the thing you care about. TDD discipline when it adds value: write the test, verify it fails for the right reason, then implement.

### Run and interpret

Run the new tests. Run the broader suite to check regressions. Test interprets failures honestly: the code is wrong (fix the code), the test is wrong (fix the test), or the failure is intermittent (move to debug mode). The report names what passed, what failed, and what needs attention.

### Debug pathologies

Tests that fail intermittently, depend on ordering, or pass alone but fail together are bugs in the test suite, not the code. Test diagnoses against the common pathologies: flakiness (race conditions, timing assumptions, leaky state), ordering dependency (test B needs test A's side effects), environment sensitivity (local vs CI), mock drift (the mock no longer matches the real dependency).

## How to use it well

**Plan before you code.** Running a plan pass on the ACs before implementation starts often surfaces ambiguities that refinement missed. That's cheaper than discovering them mid-build. Pair this with [build](build.md) on stories with complex ACs.

**Ask for strategy before tests.** "Write tests for this" skips the strategy step. A three-to-five-line strategy (what layer, what behaviors matter, what edges), then buy-in, then the tests. One round of strategy saves three rounds of rewrites.

**Pick the cheapest honest layer.** Business logic tested via E2E is slow and imprecise. Integration tests pretending to be unit tests miss the boundary they were meant to exercise. Match the layer to what you actually need to verify.

**Fix flakes, don't retry them.** A test that retries to pass is debt. Test triages the pathology to its root. Delete the test if the pathology can't be fixed and the coverage isn't worth the noise.

**Gaps are a refinement problem, not a test-writing problem.** If you can't derive a case from an AC, that's a signal the AC needs work. Don't invent what the feature should do.

## Saving artifacts

Test writes test files into the codebase using the project's existing conventions. Test plans (derived from ACs) are notes in the conversation or, when helpful, appended to the story body. Test does not create new artifact types in the development graph; it works inside the existing code and story structure.

See [the develop overview](../develop.md#the-artifacts) for how stories carry ACs that test reads.

## Common failure modes

- **Skipping strategy.** Writing tests without understanding what needs testing. Produces either over-test (everything, including trivial code) or under-test (happy path only).
- **Wrong layer.** Testing business logic via E2E because unit tests felt tedious. The suite gets slow and the logic isn't covered precisely.
- **Testing the implementation.** Tests that break whenever internals change, not whenever behavior changes. Fragile suite, expensive refactoring.
- **Ignoring flakes.** Retrying a flaky test until it passes is debt. Fix the pathology or delete the test.
- **ACs guessed, not derived.** If you can't find the AC, don't invent a test case for what you assume the feature should do. Ask.
- **Writing tests that don't belong.** Ignoring the project's conventions produces tests that future readers can't easily follow. Read the neighbors first.

## Transitions

- Plan surfaces a gap (ambiguous AC) → [**assess**](assess.md) refinement, or resolve with the engineer
- Strategy and cases are clear → [**build**](build.md) for the implementation pass
- A flake turns out to be a real bug → [**build**](build.md) to fix the underlying code
- Coverage question grows into a full design question → [**spec**](spec.md)
- Debug reveals the wrong test strategy entirely → back to plan within test

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and artifact hierarchy.
- [build](build.md) — the inner-loop companion; test handles the testing moves build delegates.
- [assess](assess.md) — where AC gaps get refined when test surfaces them.
