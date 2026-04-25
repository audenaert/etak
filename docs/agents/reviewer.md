# reviewer

**Type:** autonomous agent. **Purpose:** read a PR or diff and post structured findings across six dimensions.

The interactive [`/review`](../skills/review.md) skill pairs with the engineer through a review. The reviewer agent does the same work autonomously — fetches the diff, reads the surrounding code, runs the dimensions, posts the findings to the PR. Same six dimensions, same calibration, autonomous report.

## When to reach for it

- **You created a PR and want a review pass before requesting human review.** The reviewer agent runs the dimensions, posts findings, and you address them before anyone else looks.
- **You're triaging a backlog of PRs.** Dispatch the reviewer to walk each one and surface what matters.
- **A PR touches multiple subsystems.** The reviewer reads each subsystem's surrounding code; running the dimensions across that surface area would be tedious to drive turn by turn.
- **`developer` (when `etak-develop` is installed) just opened a PR.** It dispatches the reviewer automatically.

## What it does

### Six dimensions

The reviewer walks the diff across the same six dimensions as the [`/review`](../skills/review.md) skill: problem fit, simplicity, critical zones, architectural boundaries, security, test integrity. Each finding includes:

- A dimension prefix (🎯 problem fit, ✂️ simplicity, 🔒/🗄️/🔌/⚙️/🔗/📦 critical zones, 🏗️ boundaries, 🔐 security, 🧪 tests)
- Severity (🔴 high — must address, 🟡 medium — should address, 🟢 low — consider)
- Specific location (file, line)
- Clear concern with a suggested fix

### Read beyond the diff

The reviewer reads each changed file in full, not just the changed lines. Boundary and security analysis need surrounding context. The diff alone shows you what changed; the surrounding code shows you what it touches.

### Calibrate to the change

A one-line CSS tweak doesn't get a security deep-dive. A new auth endpoint does. The reviewer assesses risk surface first and applies depth proportional to risk.

### Post findings

By default, the reviewer posts a summary comment on the PR with batched inline comments for specific findings. With `--dry-run`, it displays in terminal only.

### Suggest next steps

Based on findings:
- High-severity → "These need fixing before merge. Want me to delegate to `developer`?" (when `etak-develop` is installed)
- Test integrity issues → "Want to dispatch `quality-engineer` for deeper acceptance verification?"
- Clean → "Ready for human review."

## What it doesn't do

- **Manufacture concerns.** Silence is valid. If a dimension is clean, the agent says so in one line and moves on.
- **Duplicate linting.** If ESLint or ruff would flag it, the project already does.
- **Refactor the code.** Findings are findings; refactors live in [`etak-develop /build`](../skills/build.md).
- **Run automated tests.** CI does that. The reviewer reads tests and asserts honesty; it doesn't execute them.
- **Decide acceptance.** Findings inform; the human decides what blocks merge.

## How it operates

- **Ground findings in specifics.** "This might have security implications" is useless. "The user-supplied `sortBy` parameter at line 45 is interpolated into a SQL query without sanitization" is actionable.
- **Calibrate intensity.** Match review depth to the change's risk surface.
- **Lead with severity.** Engineers triage by what blocks merge. A wall of mediums looks the same; high findings lead.
- **Celebrate clean dimensions.** Saying "no concerns here" briefly builds trust.
- **Escalate to security-lead.** When the security dimension surfaces concerns in high-risk areas (auth, payments, user data, crypto), the reviewer can dispatch [security-lead](security-lead.md) for deeper analysis. Only for genuinely high-risk changes; most PRs don't need a full security review.

## How to invoke

Ask in plain language. Examples:

```
Review PR #142.
```

```
Run a code review on this branch.
```

```
Review my changes — dry run, don't post.
```

The agent fetches the diff, reads the surrounding code, walks the dimensions, and reports back. By default it posts to the PR; pass `--dry-run` for terminal-only output.

## Tips

- **Review before requesting human review.** The reviewer agent finds what's catchable mechanically. Human reviewers focus on design judgment, which is harder to automate.
- **For security-sensitive changes, dispatch [security-lead](security-lead.md) directly.** The reviewer's security dimension is calibrated for fast scans. Deeper analysis (full threat model, attack chain mapping) belongs to security-lead.
- **For acceptance verification, dispatch [quality-engineer](quality-engineer.md).** Reviewer checks "is this good." Quality-engineer checks "is this done."

## Related

- [Deliver overview](../deliver.md) — where reviewer fits.
- [`/review`](../skills/review.md) — the interactive alternative.
- [security-lead](security-lead.md) — deeper security analysis for high-risk PRs.
- [quality-engineer](quality-engineer.md) — AC-by-AC verification, which complements reviewer's "is this good" with "is this done."
