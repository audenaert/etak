# Write Tests

Writing tests is strategy-first. Understand what needs testing, pick the
layer, present the strategy, get buy-in, then write. One round of strategy
saves three rounds of rewrites.

## Process

### 1. Detect the testing landscape

Before writing anything:
- **Frameworks** — Jest/Vitest/Mocha (JS), pytest/unittest (Py), RSpec, Go
  test, Cargo test
- **File organization** — where do tests live?
- **Patterns** — naming, structure, setup, assertion style
- **Infrastructure** — test DBs, Docker, fixtures, browser setup
- **Existing suites** — smoke, integration-only tags, feature markers

Read the test config and a few existing test files. Your tests should look
like they belong in this codebase.

### 2. Develop and present a strategy

Before writing, present 3-5 lines:
- What layer(s) and why
- What behaviors are being tested
- What's being mocked and what isn't
- The test cases you plan to write

Get buy-in. *Then* write.

### 3. Write with TDD discipline

**a. Write the test first**
- Follow existing conventions exactly
- Name tests as behavior descriptions ("returns empty array when no reviews
  are scheduled")
- One logical assertion per test
- Each test independent — no ordering dependencies

**b. Run the test**
- Verify it fails *for the right reason* (not a setup error)
- If it passes immediately, you're testing the wrong thing

**c. Implement until it passes**

**d. Refactor**

### 4. Layer-specific guidance

**Unit tests:**
- Test behavior, not implementation
- Don't test private methods — test through the public interface
- Mock at boundaries (external services, DBs), not internals
- Skip trivial getters, simple delegations, framework behavior

**Integration tests:**
- Use real (or realistic) dependencies — don't mock the boundary you're
  testing
- Do mock external services you don't control
- Test error paths — integration tests earn their keep on failure modes
- Tag/organize for separate execution (slower than unit)

**API tests:**
- Real HTTP requests
- Assert status codes, response shapes, headers
- Test auth comprehensively (valid, missing, wrong role)
- Be specific about response shapes — don't just check status 200

**E2E tests:**
- Use the project's Page Object pattern (or whatever exists)
- Resilient selectors: `data-testid` > aria-label > semantic HTML > never
  CSS classes
- Proper async waiting — never hardcoded sleeps
- Each test protects a critical user flow — don't over-test with E2E
- Independent — each runs in isolation

### 5. Run and verify

Run the new tests. Run the broader suite for regressions. Report results
clearly.

## Guidelines

- **Strategy first.** Don't jump to code. Strategy, buy-in, then write.
- **Match the project.** Your tests should look like they belong. Naming,
  structure, assertion style, mocking patterns.
- **Right layer for the job.** Unit for logic, integration for boundaries,
  API for contracts, E2E for critical flows.
- **Each test earns its place.** Especially at higher layers. Justify each.
- **ACs are test specifications.** When a story has ACs, they're the
  primary source of cases.
- **Don't over-test.** Trivial code doesn't need tests. Same behavior at
  three layers is waste.
- **Test behavior, not implementation.** Tests that break when internals
  change (not behavior) are fragile.
