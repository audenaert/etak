# Run Tests

Running tests intelligently means understanding what to run, why, and what
the results mean. Not just a command runner.

## Process

### 1. Detect the runner

Auto-detect from project config:
- `package.json` scripts → npm test, jest, vitest, playwright test
- `pyproject.toml` / `pytest.ini` → pytest
- `Gemfile` / `.rspec` → rspec, rails test
- `go.mod` → go test
- `Cargo.toml` → cargo test
- `Makefile` / `justfile` → custom commands

Check for suite configuration (tags, markers, projects, directories).

### 2. Determine the request

- **Nothing** — smart default: run tests for changed files (via git diff)
- **"focused"** or **"for [file/feature]"** — tests relevant to specific
  changes
- **"smoke"** / **"full"** / **"[suite-name]"** — named suite
- **"failing"** / **"failed"** — re-run only previously failing tests
- **Specific file or pattern** — run exactly that

### 3. Execute with clear output

- Show the command being run so the user can repeat it
- Set reasonable timeouts
- Capture or stream output

### 4. Interpret results

**All passing:**
- "All N tests pass. [Time taken]"
- If focused: offer to run the full suite for regressions

**Some failing — categorize each:**
- **Assertion failure** — code does something different from what the test
  expects. Usually a real issue.
- **Error / exception** — code throws unexpectedly. Check setup, inputs,
  edge cases.
- **Timeout** — infinite loop, unresolved promise, missing mock for a slow
  resource.
- **Setup failure** — test infrastructure problem (DB not running, missing
  fixture, env var). Not a code bug.

**Many failures — triage:**
Find the root cause. Many failures often share one underlying issue.
Identify it, don't list every failure.

### 5. Suggest next steps

- All pass, focused run → "Run the full suite?"
- Assertion failures → "The test expects X, code does Y — test wrong or
  code wrong?"
- Setup failures → "Looks like the test DB isn't running" or "Missing env
  var X"
- Intermittent failure → hand off to debug mode

## Organizing test suites

### Standard taxonomy

| Suite | Contents | When to run | Speed target |
|-------|----------|-------------|-------------|
| **Smoke** | Critical path only | Every push, pre-commit | < 30 seconds |
| **Feature** | Tests grouped by capability | During feature work | < 2 minutes each |
| **Integration** | Cross-boundary tests | CI on every PR | < 5 minutes |
| **E2E** | Browser tests | CI on every PR | < 10 minutes |
| **Full** | Everything | Before merge to main | Whatever it takes |

Not every project needs all of these. Propose what fits the size and
speed.

### Implement grouping

- **Jest/Vitest**: `--testPathPattern`, projects config, custom runners
- **Pytest**: markers (`@pytest.mark.smoke`) and `-m smoke`
- **RSpec**: tags (`:smoke`) and `--tag smoke`
- **Go**: build tags or `-run` patterns
- **Playwright**: projects in config or tag-based filtering

## Guidelines

- **Speed matters.** Run the smallest useful set first.
- **Show the command.** Users should be able to re-run it themselves.
- **Diagnosis over dump.** Extract the meaningful part: which test, what
  assertion, why.
- **Smoke is sacred.** If it takes more than 30 seconds, it's too big.
- **Feature suites map to thinking, not code structure.** "Auth tests" and
  "payment tests" beat "controller tests" and "model tests."
- **Don't create suites nobody will use.** If the full suite takes 45
  seconds, just run everything.
