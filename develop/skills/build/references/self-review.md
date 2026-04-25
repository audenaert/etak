# Self-Review

The private "am I done?" check before pushing. Terminal only — nothing
touches GitHub. The author's tool, so be direct and actionable, not
diplomatic.

## Process

### 1. Gather the changes

- `git diff main...HEAD` for the full change set
- `git log main..HEAD --oneline` for commit history
- Identify changed, new, and deleted files

### 2. Load the story context

Map the branch to a story in `docs/development/stories/`:
- Parse branch name
- Or ask: "Which story is this for?"
- Load the story's ACs if found

### 3. Run automated checks

In parallel where possible:

**Lint / static analysis:**
- Detect the linter (`eslint`, `ruff`, `rubocop`, `golangci-lint`, etc.)
- Run on changed files only
- Report file, line, rule, message

**Tests:**
- Run focused tests for the changed files first
- Then the broader suite for regressions
- Report pass/fail clearly; brief diagnosis on failure

**Type checking** (if applicable):
- `tsc --noEmit`, `mypy`, `pyright`, etc.
- Report only errors in changed files

### 4. AC coverage check

For each AC in the story, scan the diff for code that implements or tests
it:

```
## AC Coverage

✅ AC #1: "Given an unauthenticated user, when they click Sign In..."
   → Covered: src/routes/auth.ts adds the OAuth redirect handler

✅ AC #2: "Given a user who completes OAuth..."
   → Covered: src/routes/auth-callback.ts handles the callback

⚠️ AC #3: "Given a user whose token is expired..."
   → Not clearly covered in this diff. Missing or in another branch?
```

Mark as covered (with evidence), possibly covered (ambiguous), or not
covered. Uncovered ACs are the highest-risk gap — surface them first.

### 5. Code clarity scan

Flag the 2-3 most impactful issues, not every minor style concern:
- Functions over ~30 lines
- Unclear variable/function names
- Missing error handling on external calls
- TODO/FIXME/HACK without context
- Dead code (unreachable branches, unused imports)
- Secrets or credentials (env vars, API keys in code)

### 6. Present the verdict

```
## Self-Review: [branch name]

### Automated Checks
- Lint: ✅ clean (3 files checked)
- Tests: ✅ 12/12 passing
- Types: ✅ no errors

### AC Coverage (story: user-can-sign-in-with-google)
- ✅ 3/4 ACs clearly covered
- ⚠️ 1 AC needs attention: token expiration

### Code Review
- ⚠️ `handleCallback()` is 45 lines — consider extracting token validation
- ✅ No secrets, no dead code, no missing error handling

### Verdict: Almost ready
Address the token expiration AC gap, then push.
```

**Verdicts:**
- **Ready to push** — all checks pass, ACs covered
- **Almost ready** — minor gaps
- **Not ready** — failing tests, missing AC coverage, or real issues

### 7. Offer to fix

- Lint issues → auto-fix if the linter supports it
- Missing coverage → "Scaffold a test for token expiration?"
- Clarity issues → suggest the specific refactor

## Guidelines

- **Direct, not diplomatic.** This is the author's tool.
- **AC coverage is the highest-value check.** A missing AC discovered
  after PR creation wastes everyone's time.
- **Don't duplicate what lint catches.** If eslint already flags it, don't
  also flag it in the clarity scan.
- **Speed matters.** The author is about to push. Keep self-review under
  30 seconds for a typical change.
- **Terminal only.** Nothing touches GitHub.
