# Process Feedback

You help the author process PR review comments — understand, implement,
respond. The goal is to close the feedback loop quickly and thoroughly.

## Process

### 1. Fetch review comments

```bash
gh api repos/{owner}/{repo}/pulls/{pr_number}/reviews
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments
gh pr view {pr_number} --comments
```

### 2. Parse and categorize

For each comment:

**Type:**
- **Action required** — reviewer wants a change
- **Question** — reviewer wants clarification
- **Suggestion** — non-blocking improvement
- **Approval** — positive feedback, no action

**Scope:**
- **Specific** — points to a line/file with concrete change
- **General** — architectural/design feedback spanning the PR

**Priority:**
- **Blocking** — reviewer requested changes (CHANGES_REQUESTED)
- **Non-blocking** — suggestions, nits, questions

### 3. Present the summary

```
## PR Feedback: #{pr_number}

### Action Required (3)
1. `src/routes/auth.ts:34` — @reviewer1: "Add rate limiting"
2. `src/services/token.ts:12` — @reviewer1: "Throw on invalid, don't return null"
3. General — @reviewer2: "Missing OAuth timeout handling"

### Questions (1)
4. `src/middleware/auth.ts:22` — @reviewer1: "Why advisory lock?"

### Suggestions (2)
5. `src/routes/auth.ts:45` — @reviewer2: "nit: extract into a helper"
6. `src/config/oauth.ts:8` — @reviewer2: "make this per-environment?"

### Approved
- @reviewer3: no comments
```

Ask: "Address all action-required items, or pick specific ones?"

### 4. Implement changes

For each change:
1. Read the comment and the code it references
2. Read surrounding context
3. Implement the change
4. Run relevant tests

Group related changes into a single commit: three comments about error
handling → one commit ("fix: add error handling for OAuth timeout,
invalid tokens, and rate limiting").

### 5. Respond to questions

For questions requiring answers (not code changes):
- Draft a response based on code context
- Show the draft to the engineer for approval before posting
- Post:
```bash
gh api repos/{owner}/{repo}/pulls/{pr_number}/comments/{comment_id}/replies \
  -f body="[response]"
```

### 6. Respond to reviewers

After implementing, reply to each addressed comment. Keep it short but
specific:
- Code changes: "Fixed in [sha] — added rate limiting at 10 req/min per IP"
- Questions: the answer
- Suggestions taken: "Good call — done in [sha]"
- Suggestions declined: explain briefly (get engineer's approval)

"Fixed" alone tells the reviewer nothing. Say what changed.

### 7. Push and re-request review

```bash
git push
```

If all blocking comments are addressed:
```bash
gh pr edit {pr_number} --add-reviewer {reviewer}
```

Report: "Addressed N comments, pushed M commits. Re-request review from
@reviewer1?"

## Guidelines

- **Address blocking feedback first.** Non-blocking can wait or be
  declined with a reason.
- **Don't argue with reviewers in code.** If you disagree, draft a
  response explaining your reasoning and let the engineer decide whether
  to post it. The engineer owns the conversation.
- **Group related fixes.** Three comments on error handling → one commit.
- **Run tests after every change.** Review feedback often touches edge
  cases — don't introduce regressions.
- **Be specific in responses.** "Fixed" is useless; "Added rate limiting
  at 10 req/min per IP, 429 on excess" is useful.
- **Questions are opportunities.** A "why?" often means the code needs a
  comment. Consider adding the explanation to the code, not just the PR
  thread.
