# build

**Stance:** collaborative. **Purpose:** the developer inner loop end to end. TDD pairing, self-review, PR, feedback.

Build is where code gets written. Where [plan](plan.md) shapes scope, [spec](spec.md) grounds design, and [assess](assess.md) gates readiness, build picks up a ready story and carries it across the finish line: implementation, self-review, PR, review feedback.

Build is a pairing move. The engineer stays in the driver's seat. Build reads the story and the adjacent code before it types, uses TDD when it earns its keep, and notices when the scope is drifting.

## When to reach for it

- **A ready story is in front of you.** The ACs are testable, the approach is known, and the codebase is open. Time to build.
- **You want a pairing partner on the implementation.** Someone to read adjacent code with you, write tests alongside, commit at AC boundaries.
- **You're ready to push and want a self-review.** Lint, tests, types, AC coverage, clarity scan: the pre-flight checks before the PR.
- **The PR needs a description reviewers can actually use.** A summary, grouped changes, AC checklist, testing notes, areas where you want specific feedback.
- **Reviewers have come back.** Feedback needs triage, grouped changes, specific responses, and a clean re-request for review.

Trigger phrases: *implement this*, *build this story*, *let's pair on*, *TDD this*, *self review*, *am I done*, *ready to push*, *check my work*, *create PR*, *open a PR*, *review feedback*, *address comments*.

## How it behaves

Build reads before it writes. Grep for the modules the story touches, read how similar features are structured, read the tests for neighboring functionality. If the spec says "use the auth module," build verifies that module exists and works the way the spec assumes.

The sharpest move build makes is noticing scope drift. "This is starting to touch four subsystems. Is that the story, or are we growing scope?" That question, asked mid-implementation, prevents the PR that can't be reviewed.

### Preconditions

Build pushes back when the story isn't ready. The story should be at ready status, the ACs should be testable (not "users can sign in" but "given X, when Y, then Z"), and the approach should be known. If any of these are missing, build says "let's assess readiness first" rather than start. It's cheaper than discovering the gap mid-implementation.

### Implement

Build picks up the story, reads the adjacent code, then pairs through the implementation. TDD when it helps: the failing test, the implementation, the refactor. For pure wiring or one-line config, TDD ceremony isn't required. Build uses judgment.

The cadence is one AC at a time. Build to one AC, verify it passes, commit, move on. Atomic commits keep PRs reviewable. If the implementation of AC #3 starts dragging into AC #7's territory, build stops and names it.

Test-specific moves (deriving cases from ACs, choosing layers, fixing flakes) hand off to [test](test.md). Build handles "write a test for this AC"; test handles "plan the test strategy for this story."

### Self-review

Before pushing, build runs the pre-flight checks: lint and static analysis on changed files, focused tests for the changed files then the broader suite for regressions, type checking, AC coverage (scan the diff, confirm each AC has corresponding code and ideally a test), and a clarity scan for long functions, unclear names, missing error handling, committed secrets, or TODO markers without context.

The verdict is specific: ready to push, almost ready (minor gaps addressable in minutes), or not ready (failing tests, missing AC coverage, real issues).

### PR

A good PR description lets reviewers spend their time on design judgment rather than figuring out what changed. Build structures the description: a one-to-three-sentence summary, changes grouped by purpose (not by file), an AC checklist linking the story, testing notes including manual verification when relevant, and notes for reviewers flagging uncertainty or tradeoffs.

Diagrams (Mermaid) go in when they genuinely help (architecture changes, schema changes, state machines), wrapped in `<details>` so they don't dominate. The title runs under 70 characters, imperative mood, specific. "Add Google OAuth login flow" beats "Changes for auth."

### Feedback

When reviewers come back, build fetches and categorizes the comments: action required, question, suggestion, approval. Blocking comments surface first. Changes get grouped by theme (three comments about error handling become one commit, not three). Responses are specific: not "fixed" but "added rate limiting at 10 req/min per IP in middleware, returning 429."

Disagreements don't get posted silently. If build disagrees with a reviewer, it drafts the response and shows the engineer first. The engineer owns the conversation. Build re-requests review once blocking items are addressed.

## How to use it well

**Come in with a ready story.** A story with vague ACs can't be rescued by the inner loop. Run [assess](assess.md) ready first. The feedback is faster and cheaper than discovering the gap in review.

**Use TDD where it earns its keep.** Logic with edge cases: test first. Pure wiring or config: straight implementation is fine. Build uses judgment, not ceremony.

**Commit at AC boundaries.** Atomic commits keep the PR reviewable and make bisecting trivial if a regression shows up. "WIP" commits squashed at the end is a weaker pattern.

**Name scope drift out loud.** The story that grows two extra subsystems is the PR nobody reviews. If you're touching code that wasn't in the story, stop and either extract a follow-up or name the addition in the PR description.

**Don't skip the AC coverage scan.** Self-review's highest-value check is matching the diff against the ACs. A PR that ships with one AC uncovered is the fastest way to waste a review cycle.

**Pair with test on complex stories.** Running [test](test.md) plan before you start often sharpens the ACs before any code lands. The derived cases are the build spec.

## Saving artifacts

Build writes code into the repository and creates the PR. It updates story status as work progresses (in-progress when implementation starts, ready-for-review when the PR opens, done after merge). It does not create new development artifacts. Specs, ADRs, and new stories come from [spec](spec.md) and [plan](plan.md).

See [the develop overview](../develop.md#the-artifacts) for how stories move through status and how build's work links back to the story and, through `from_discovery`, to the validated idea.

## Common failure modes

- **Building before the story is ready.** The inner loop can't rescue a story with vague ACs. Run ready check first.
- **TDD as ceremony.** Not every line needs a test-first flow. Pure wiring or config: straight implementation. Judgment, not dogma.
- **Skipping AC coverage.** The self-review step that catches the missing acceptance criterion is the one most often skipped. Don't.
- **PR-as-dump.** A description that says "see commit messages" forces reviewers to reconstruct the story. Write the summary.
- **Silent disagreement with reviewers.** If you disagree, draft the response and surface it. Don't just change the code grudgingly or ignore the comment.
- **Scope drift.** The PR grew because "while we're in there." Extract follow-ups. Keep the PR to its story.

## Transitions

- The story isn't ready → [**assess**](assess.md) ready
- The approach isn't known → [**spec**](spec.md)
- Test strategy needs deriving from ACs → [**test**](test.md) plan
- Mid-build you discover the plan is wrong → [**plan**](plan.md)
- PR merged and you want the next move → [**survey**](survey.md)
- An ambiguous AC surfaces mid-build → [**assess**](assess.md) refine, or resolve directly

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and artifact hierarchy.
- [test](test.md) — the testing counterpart; build delegates when the question gets deeper than "write a test for this AC."
- [assess](assess.md) — the gate build runs behind.
