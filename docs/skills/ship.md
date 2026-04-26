# ship

**Stance:** pragmatic release engineer. **Purpose:** get changes from green local builds into safely running production — diagnose CI failures, cut releases, draft notes that are actually useful.

Ship is the unglamorous middle work that prevents shipping disasters. It's the difference between "the code is done" and "the code is in production with the right people knowing about it."

This skill is the collaborative version. The autonomous counterpart is the [release-engineer agent](../agents/release-engineer.md).

## When to reach for it

- **CI is red and you want it green again.** Ship reads the actual logs (not the GitHub Actions summary) and traces the failure to its root cause.
- **The build's been flaky and you're tired of retrying.** Ship pushes back on retry-until-green and traces the flake to its root cause.
- **You're cutting a release.** Ship confirms scope, bumps the version, drafts release notes in the user's frame.
- **Release notes need writing.** A note that reads as a commit message is a failed note. Ship writes for users.

Trigger phrases: *fix CI*, *CI is red*, *diagnose this build*, *why did the deploy fail*, *cut a release*, *release notes*, *tag a version*, *get to green*.

## How it behaves

Ship reads the actual job output, not the run summary. The first real error is the one that matters; the fifty downstream cascade errors are noise. After reading, ship classifies the failure — real bug, flaky test, infrastructure flake, or configuration drift — because the fix differs by category. Misdiagnosis wastes hours.

The sharpest move ship makes is distinguishing infrastructure flakes from real test failures from configuration drift. A test that fails locally too is a real bug. A test that passes on retry, with no code change, is flaky and deserves root-cause analysis (not retries). A test that fails because an env var changed is configuration drift; reconcile it.

### Diagnose

Ship reads:
- **What ran.** The exact command, the exact environment.
- **What failed.** The first real error, not the cascade.
- **Why.** Code change broke a test? Test was already flaky? Environment changed (image bump, secret rotation, dependency update)? Resource limit (timeout, memory)?

Categorization matters because each category has a different fix. Ship doesn't guess the category — it reads enough to classify confidently.

### Fix

For each category:
- **Real bug:** make a small focused commit. The PR with the fix is its own review-worthy change.
- **Flaky test:** trace to root cause. Common causes: shared state, time-based assertions, network calls in unit tests, resource leak. Fix the cause; if you can't fix today, mark `.skip` with an issue link, not retried.
- **Infrastructure flake:** retry once. If it recurs, it's a pipeline problem, not a build problem — escalate to [`operate`](operate.md).
- **Configuration drift:** reconcile actual configuration with what the spec or IaC says. Update one to match the other; don't leave them out of sync.

After the fix, verify CI is genuinely green — not just "the failing job passed once." Run it twice if the test was flaky.

### Release

Ship confirms scope (commits between last tag and HEAD, grouped by theme), bumps the version following the project's scheme (don't invent one), and drafts notes:
- **What's new** (user-visible features)
- **What's fixed** (bug fixes that affect users)
- **What's changed** (breaking changes, deprecations)

Each line in the user's frame, not the engineering frame. "Search now ranks by relevance, not recency" beats "Updated SearchService.scoreDocuments." If a commit doesn't change anything user-visible, ship omits it.

## How to use it well

**Read the logs, don't guess.** The answer is almost always in the error output. Don't try fixes until you've read the failing step's full log.

**Reproduce locally first when possible.** A CI fix you can't reproduce locally is fragile. The next CI failure of the same shape will not be obvious.

**Don't retry until green.** A flake passing on retry is still a flake. Retries are the temptation that compounds. Trace it once, fix it once.

**Smaller, more frequent releases.** A big-bang tag with months of changes makes notes overwhelming and rollback messy. Smaller releases are easier to write, easier to test, and easier to undo.

**Write notes for users.** A release note that reads as a commit message is a failed note. The reader of the notes shouldn't need to read the diff to understand what changed.

## What ship does NOT do

- **Write the production code.** Bug fixes ship discovers belong in [`etak-develop /build`](../skills/build.md). Ship diagnoses and fixes pipeline issues; substantive code changes get delegated.
- **Run the deployment.** Ship prepares the release; the actual `kubectl apply`, `terraform apply`, or registry push lives with [`operate`](operate.md) or with the engineer's authorized hand.
- **Decide what's safe to ship.** Verification ([`verify`](verify.md)) and security review ([`secure`](secure.md)) run first. Ship's job is to get a verified, reviewed, secure change to production.
- **Restructure pipelines.** When the same step keeps timing out across PRs, that's a structural pipeline problem. Hand to [`operate`](operate.md).

## Common failure modes

- **Retry-until-green.** The flake passes on retry, so it gets retried. Same flake recurs across PRs. Cause: short-term path of least resistance. Fix: trace to root cause once.
- **Diagnose by summary.** Reading the GitHub Actions overview, not the actual log. Cause: the overview is right there. Fix: open the failing job's log and read the lines around the first error.
- **Mute-and-skip.** Flaky test gets `.skip` and no follow-up. Cause: shipping pressure. Fix: every skip is paired with an issue link; review the skipped list at each release.
- **Big-bang release.** Months of changes in one tag. Cause: deferring the release work. Fix: smaller, more frequent releases — release notes get easier and rollback gets cleaner.
- **Notes-as-changelog.** Release notes that are commit messages. Cause: copy-paste from git log. Fix: write notes in the user's frame.

## Transitions

- A failing test reveals a real bug → [**etak-develop /build**](../skills/build.md) to fix it
- Recurring infrastructure flake → [**operate**](operate.md) for structural pipeline review
- Release scope reveals a security concern → [**secure**](secure.md) before tagging
- Release notes drift from docs → [**docs**](docs.md) to sync
- Story isn't done → [**verify**](verify.md) before shipping

## Related

- [Skills index](README.md)
- [Release-engineer agent](../agents/release-engineer.md) — the autonomous version of this skill.
- [Deliver overview](../deliver.md) — how ship fits in the deliver plugin.
- [operate](operate.md) — the structural counterpart; ship handles individual failures, operate handles pipeline architecture.
