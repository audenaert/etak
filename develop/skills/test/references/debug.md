# Debug Tests

Test pathologies are tests that fail for reasons other than the code being
wrong. These are the most frustrating test failures — the code works, but
the tests don't reliably prove it. Almost always, a "flaky" test has a
deterministic cause you haven't found yet.

## Process

### 1. Characterize the failure

Ask (or observe):
- Does it fail every time or intermittently?
- Fails in CI but passes locally (or vice versa)?
- Fails only when run with other tests?
- Did it start recently, or has it always been unreliable?

### 2. Classify the pathology

| Pathology | Symptoms | Common causes |
|-----------|----------|---------------|
| **Ordering dependency** | Passes alone, fails when run after specific other tests | Shared mutable state (globals, DB rows, singletons, class vars) not cleaned up |
| **Flakiness** | Fails intermittently, no obvious pattern | Timing issues, race conditions, non-deterministic data, external services |
| **Environment sensitivity** | Passes locally, fails in CI (or vice versa) | OS, timezone, locale, env vars, DB version, language version, file paths |
| **Mock dependency** | Test passes but doesn't test real behavior | SUT heavily stubbed, mocks return what assertions expect regardless of input |
| **State leakage** | Tests interfere with each other | DB not rolled back, temp files, in-memory caches, env var mutation |
| **Timing sensitivity** | Fails under load or on slow machines | Hardcoded sleeps, tight timeouts, race between async ops |

### 3. Investigate

**Ordering dependency — bisect:**
1. Run the failing test in isolation. Passes? Then it depends on side
   effects.
2. Bisect the suite — run the failing test with the first half, then
   quarter, etc., to identify which test pollutes state.

**Flakiness — find the non-determinism:**
1. Run the test 10-20 times in a loop. What's the failure rate?
2. Look for: `setTimeout`, `sleep`, polling, `Date.now()`, `Math.random()`,
   network calls, file system, spawned processes.

**Environment sensitivity — compare envs:**
1. Diff the environments: OS, versions, env vars, timezone, locale.
2. Check path assumptions (`/tmp/`, Windows vs Unix).
3. Check timezone-sensitive operations.

**Mock dependency — assess coverage:**
1. Identify every mock. For each: is it mocking a collaborator (OK) or the
   SUT (problem)?
2. Remove mocks one at a time. Does the test still pass?
3. Check if mocks return realistic data or just what assertions expect.

**State leakage — find the leak:**
1. Run tests in reverse order. Different tests fail?
2. Check teardown: DB rolled back? Temp files deleted? Globals reset?
3. Look for shared mutable state.

### 4. Present the diagnosis

```
## Test Pathology Diagnosis

**Test:** `test/services/payment.test.ts:45 "processes refund correctly"`
**Pathology:** Ordering dependency
**Pattern:** Passes alone, fails when run after `checkout.test.ts`

**Root cause:** `checkout.test.ts:78` sets
`process.env.PAYMENT_MODE = 'sandbox'` but doesn't restore it. The refund
test assumes `PAYMENT_MODE` is unset.

**Fix:**
- Add `afterEach` in `checkout.test.ts` to restore `PAYMENT_MODE`
- Or: make the refund test set the env var it needs explicitly

**Prevention:** Consider a helper that snapshots/restores env vars
automatically.
```

### 5. Offer to fix

For each diagnosed pathology, offer to implement the fix:
- Add cleanup code
- Make tests deterministic (mock time, fixed seeds, proper async waiting)
- Replace problematic mocks with real implementations
- Add environment isolation

## Guidelines

- **Start with the simplest hypothesis.** Most "flaky" tests have a
  deterministic cause. You just haven't found it yet.
- **Bisect mechanically.** Don't guess — narrow it down systematically.
- **Mock dependency is the sneakiest.** The test passes, so nobody notices
  it's not testing anything. Check mocking when coverage looks too clean.
- **Prevention beats cure.** After fixing a pathology, suggest how to
  prevent recurrence: helpers, CI config, conventions.
- **Some flakes are real bugs.** If the test is correct and the code has a
  race condition, that's a bug in the code, not a test pathology.
