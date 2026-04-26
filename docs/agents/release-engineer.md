# release-engineer

**Type:** autonomous agent. **Purpose:** get changes from green local builds into safely running production — diagnose CI failures, cut releases, draft notes in the user's frame.

The interactive [`/ship`](../skills/ship.md) skill collaborates with the engineer through diagnosis or drafting. The release-engineer agent does the same work autonomously: reads the failing logs, classifies, fixes, verifies; or confirms scope, bumps version, drafts notes, tags.

## When to reach for it

- **CI is red and you have other work.** The agent reads the actual logs (not the GitHub Actions summary), classifies the failure, fixes it, and verifies CI is green before reporting back.
- **You're cutting a release and want it drafted autonomously.** The agent confirms scope, bumps the version, drafts release notes in the user's frame, and stops at the tag command for explicit approval.
- **A flaky test keeps failing across PRs.** The agent traces it to root cause rather than retrying.
- **`developer` (when `etak-develop` is installed) hits a CI failure on a PR.** It can dispatch the release-engineer automatically.

## What it does

### Diagnose & fix mode

When CI is red:

**Read the logs.** Not the run summary — the full log of the failing step.

**Classify the failure:**
- **Real bug** — the diff broke behavior the tests catch
- **Flaky test** — passes on retry, no code change. Trace the root cause; don't retry.
- **Infrastructure flake** — network, image registry, transient cloud resource. Retry once if genuinely transient; flag if recurring.
- **Configuration drift** — env var, secret, image, dependency changed. Reconcile against spec or IaC.

**Fix and verify.** Make a small focused commit. Run the relevant tests locally if possible. Push, watch CI, iterate if a different failure surfaces. Verify CI is genuinely green — not just "the failing job passed once."

### Release mode

When changes are ready to release:

**Confirm scope.** Read commits between last tag and HEAD; group by theme, not by commit. When `etak-develop` is installed, map commits to stories where possible.

**Bump version.** Follow the project's versioning scheme. Don't invent one.

**Draft release notes.** Three sections — what's new (user-visible features), what's fixed (bug fixes that affect users), what's changed (breaking changes, deprecations). Each line in the user's frame. "Search now ranks by relevance, not recency" beats "Updated SearchService.scoreDocuments."

**Tag and publish.** The agent shows the tag command and waits for explicit approval before pushing tags or creating GitHub releases.

## What it doesn't do

- **Write production code.** Bug fixes the agent discovers belong in [`etak-develop /build`](../skills/build.md). The agent diagnoses and fixes pipeline issues; substantive code changes get delegated.
- **Run the deployment.** The agent prepares releases; the actual `kubectl apply` or registry push lives with [devops](devops.md) or with the engineer's authorized hand.
- **Decide what's safe to ship.** Verification ([quality-engineer](quality-engineer.md)) and security ([security-lead](security-lead.md)) run first. The agent's job is to get a verified, reviewed, secure change to production.
- **Restructure pipelines.** When the same step keeps failing across PRs, that's structural — handed to [devops](devops.md).

## How it operates

- **Logs first, guesses never.** The error output has the answer.
- **Classify before fixing.** Real bug, flaky test, infrastructure flake, configuration drift — each has a different fix.
- **Don't retry-until-green.** A flake passing on retry is still a flake. Trace it.
- **Reproduce locally first when possible.** A CI fix you can't reproduce locally is fragile.
- **Write notes for users.** A release note that reads as a commit message is a failed note.
- **Smaller, more frequent releases.** Big-bang tags make rollback hard and notes overwhelming.

## How to invoke

Ask in plain language. Examples:

```
Fix CI on PR #142.
```

```
The build is broken — figure out why and fix it.
```

```
Cut a release. Latest tag is v1.4.2.
```

```
Draft release notes for the work since v2.0.0.
```

The agent fetches logs (for diagnosis) or commit history (for release), runs the appropriate mode, and reports back. For release tagging, it stops at the publish step for explicit approval.

## Tips

- **Trust the classification.** When the agent says "this is a flake, not a real failure," it's read enough log to be confident. Re-running the job won't add information.
- **Run after [quality-engineer](quality-engineer.md) verifies.** Verification confirms the story is done; release-engineer cuts the tag.
- **For recurring infrastructure failures, dispatch [devops](devops.md).** The release-engineer fixes individual builds; devops fixes the pipeline shape that produces them.
- **Don't extend the time box on release notes.** Notes that take longer than the release itself usually mean the release scope is too big.

## Related

- [Deliver overview](../deliver.md) — where release-engineer fits.
- [`/ship`](../skills/ship.md) — the interactive alternative.
- [devops](devops.md) — handles structural pipeline changes; release-engineer handles individual failures.
- [quality-engineer](quality-engineer.md) — runs before release-engineer in the typical flow.
