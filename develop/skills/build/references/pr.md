# Create PR

A good PR description lets reviewers spend their time on design judgment,
not figuring out what changed. Write it from the reader's perspective:
someone seeing this change for the first time.

## Process

### 1. Analyze the change

- `git log main..HEAD --oneline` — commit history
- `git diff main...HEAD --stat` — files changed
- `git diff main...HEAD` — the full diff
- Story in `docs/development/stories/` if the branch maps to one
- Parent epic/project for broader context

Understand:
- What changed at a high level?
- Why? (story context, bug fix, refactor)
- What's the scope?

### 2. Generate the description

```markdown
## Summary

[1-3 sentences: what this PR does and why]

## Changes

- [Key changes grouped logically — skip trivial stuff like import reordering]

## Acceptance Criteria

- [ ] AC #1: ...
- [ ] AC #2: ...

## Testing

[How this was tested — suites, manual steps]

## Notes for Reviewers

[Optional: design decisions, tradeoffs, areas of uncertainty,
where specific feedback is wanted]
```

Keep it concise. The diff shows details; the description tells the story.

### 3. Generate diagrams (if valuable)

| Change type | Diagram value |
|-------------|---------------|
| Multi-layer feature (models, services, API, UI) | High — architecture |
| Schema change | High — ER diagram |
| Workflow/state change | High — state machine or sequence |
| Refactor/structural change | Medium — before/after |
| Simple bugfix, config change | Low — skip |

Wrap in collapsible `<details>` sections. Ask before including:
> "An architecture diagram would help reviewers here. Want me to include
> one?"

### 4. Create or update the PR

**New PR:**
```bash
gh pr create --title "[concise title]" --body "$(cat <<'EOF'
[description]
EOF
)"
```

Title: under 70 characters, imperative, specific. "Add Google OAuth login
flow" beats "Changes for auth."

**Existing PR:**
```bash
gh pr edit {pr_number} --body "$(cat <<'EOF'
[updated description]
EOF
)"
```

### 5. Report back

Show the PR URL and a brief summary of what was included.

## Guidelines

- **The description is for reviewers, not the author.** Write from the
  perspective of someone seeing this change for the first time.
- **Lead with why.** The diff shows what. The description explains why.
- **Group by purpose, not by file.** "Added OAuth callback handling"
  beats "Modified auth.ts, router.ts, and config.ts."
- **Diagrams should clarify, not decorate.** If a diagram doesn't make
  the review faster, skip it.
- **Include the AC checklist** when the branch maps to a story. Reviewers
  can verify coverage without looking up the story.
- **Title is what shows up everywhere** — notification emails, Slack,
  git log. Keep it short and specific.
