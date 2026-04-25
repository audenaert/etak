# developer

**Type:** autonomous agent. **Purpose:** take a ready work item and deliver it end-to-end.

The interactive [`/build`](../skills/build.md) skill pairs with an engineer through TDD in the current workspace. developer does the same work autonomously in an isolated git worktree. Hand it a ready story, bug, or spike and it implements, tests, self-reviews, and creates the PR. The self-review is honest: if an acceptance criterion isn't fully satisfied, it says so.

## When to reach for it

- **A story is ready and you have other work.** ACs are clear, the spec exists, dependencies are resolved. The developer picks it up and returns a PR.
- **A bug needs a real fix.** You want root-cause diagnosis, a regression test, and a minimum fix rather than a patched symptom.
- **A spike needs running.** Time-boxed investigation with a specific question and decision criteria. The developer produces a finding, PoC, or ADR draft.
- **tech-lead is dispatching.** During coordinated delivery, [tech-lead](tech-lead.md) hands ready items to developer and parallelizes independent work.

## What it does

### Feature mode

For stories, tasks, enhancements, and chores. Reads the work item, the linked spec, relevant ADRs, and completed spikes. Studies the codebase for patterns, test conventions, and adjacent code. Creates an isolated worktree. Plans the implementation: files, test strategy, order, assumptions.

Works through strict TDD, one acceptance criterion at a time. Failing test first, minimum code to pass, refactor while green, small atomic commit with a conventional message. Next AC. Runs the full test suite, linter, formatter, and type checker. Submits with zero failures and zero lint warnings.

### Bug fix mode

Diagnoses root cause before writing any fix code. Traces the repro, checks recent git history, identifies where behavior diverges from expectation, and classifies the cause (logic error, missing case, regression, data issue, race condition). Writes a failing regression test that reproduces the bug, verifies it fails for the right reason, then implements the minimum fix to make it pass. Self-review covers root cause, blast radius, confidence level, and related issues.

### Investigation mode

For spikes. Reads the question, decision criteria, time box, and expected outcome type. Investigates methodically: tries the approach, sets up the library, runs the measurement, prototypes the options. Applies decision criteria and produces a finding, PoC, or ADR draft. Respects the time box. An expired box without an answer is itself a finding. Investigation mode does not create a PR.

### Self-review

Before submitting, systematically verifies each AC against the tests that cover it, quality checks, files created and modified, patterns followed, assumptions made, and concerns worth flagging. Partial satisfaction gets explained, not hidden.

### PR creation

Title under 70 characters, imperative, specific. Body covers summary, changes grouped by purpose, AC verification, testing, and optional reviewer notes. Mermaid diagrams only when they genuinely help. Offers to run review (once `etak-deliver` is installed) or a `/build self-review` pass.

## What it doesn't do

- **Start without prerequisites.** If ACs are missing, the spec is absent, or dependencies aren't resolved, it stops and tells you what's missing. It doesn't guess at requirements.
- **Expand scope.** Adjacent issues noticed during implementation become follow-up notes, not silent fixes.
- **Modify unrelated code.** It stays in the files and modules the work item covers.
- **Introduce new patterns.** It matches existing style, conventions, and architecture unless the spec explicitly calls for something new.
- **Hide gaps.** Ambiguous ACs resolved by judgment get flagged in the self-review. Unsatisfied ACs get an honest explanation.
- **Clean up the worktree.** The worktree persists until the PR is merged so you can inspect the work.

## How it operates

- **Tests first, always.** No implementation without a failing test. This is the discipline that makes the output trustworthy.
- **Read before writing.** Understanding the codebase is the difference between code that fits and code that fights the system.
- **Small, atomic commits.** One logical change each, conventional message. The history tells the story of the work.
- **Root cause over symptom.** Bug fixes target the actual cause. The regression test verifies the expected behavior, not the broken current state.
- **Answer the question asked.** Spikes address the specific question with the specific criteria. Scope creep on a spike wastes the time box.
- **Honest self-review.** Truth-telling beats a clean-looking report. Trust depends on it.
- **Don't spin.** If stuck, reports with what was tried, what was learned, where the block is. "I need help with X" beats a guess.

## How to invoke

Ask in plain language. Examples:

```
Implement story 4.3: email notification preferences UI.
```

```
Pick up the next ready story in the checkout workstream.
```

```
Fix bug #812. Root-cause it before writing the fix.
```

```
Run the spike on whether pgvector can handle our embedding
workload. Time box is 4 hours.
```

The agent creates a worktree, works through the item, and reports back with the PR URL (or the spike finding), the self-review, and anything needing your judgment.

## Tips

- **Make sure the item is ready.** Run [`/assess ready`](../skills/assess.md) on the story first if you're not sure. Unready items come back as "missing ACs" rather than as a PR.
- **Read the self-review before the diff.** The AC coverage, assumptions, and concerns sections tell you what to look at. The diff follows once you know what to check.
- **Treat partial satisfaction as a signal.** An honest "AC 3 partially satisfied because..." usually points at an AC that needs sharpening, not at a developer failure.
- **Let spike time boxes expire.** An expired box without an answer is a real finding. Extending the box to force an answer is how spikes become rabbit holes.
- **Dispatch from [tech-lead](tech-lead.md) for parallel work.** Independent stories across workstreams run concurrently. Sequencing them by hand wastes the agent's best capability.

## Related

- [Develop overview](../develop.md) — where developer fits in the plugin.
- [`/build`](../skills/build.md) — the interactive alternative when you want to pair on the implementation yourself.
- [`/test`](../skills/test.md) — strategy-first testing; developer uses the same conventions.
- [`/assess`](../skills/assess.md) — readiness checks you can run before dispatching.
- [architect](architect.md) — produces the specs developer implements against.
- [tech-lead](tech-lead.md) — dispatches developer during coordinated delivery.
