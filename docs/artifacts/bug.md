# Bug

**Role:** standalone work item capturing a defect. The software does not behave as specified.

## Why it exists

Bugs are the honest name for behavior that doesn't match intent. They live outside the project-epic-story hierarchy because defects arrive on their own schedule: from production, from support, from an engineer noticing something during unrelated work. Forcing them into the hierarchy hides them; giving them their own artifact surface keeps them visible.

A bug earns its place by being reproducible and observable. Steps to reproduce (even if intermittent), expected versus actual behavior, and honest severity. "It's broken" is a report, not a bug. A report becomes a bug when it can be examined by someone who wasn't there when it happened.

Bugs that can't be reproduced are hypotheses. Say so. The investigating status exists specifically for this: you believe there is a bug, but you haven't confirmed it. Treat the investigation as real work, not as a stale `open` ticket.

## When to create one

- When software behaves differently than specified and a reproduction exists.
- When production reports something broken and the next step is investigation.
- When an engineer notices unintended behavior during unrelated work.
- When a support ticket describes a concrete failure.

## Schema

```yaml
---
name: "OAuth token refresh fails silently on slow connections"
type: bug
status: open  # open | investigating | fix_in_progress | resolved
severity: high  # critical | high | medium | low
parent: null
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Concrete description of the defect. |
| `type` | yes | Always `bug`. |
| `status` | yes | Current lifecycle state. |
| `severity` | yes | `critical`, `high`, `medium`, or `low`. Be honest. |
| `parent` | no | Usually `null`. May reference a related story or epic. |
| `from_discovery` | no | Rarely populated for bugs; included for completeness. |

Bugs live in `docs/development/work-items/` alongside chores and enhancements, distinguished by `type`.

## Severity

- **critical** — production-down, data loss, security breach. Drop everything.
- **high** — significant user-facing impact, no reasonable workaround. Address in this milestone.
- **medium** — user-facing but with a workaround, or reliably affects a minority of users.
- **low** — cosmetic, affects edge cases, or has a trivial workaround.

Severity is not priority. A medium bug affecting an enterprise customer can be prioritized above a high bug affecting nobody in production. Severity is about impact; priority is about when you fix it.

## Status lifecycle

```
open  ─────▶  investigating  ─────▶  fix_in_progress  ─────▶  resolved
```

- **open** — reported but not yet triaged or investigated.
- **investigating** — reproducing, characterizing, finding the cause.
- **fix_in_progress** — root cause known; fix being implemented.
- **resolved** — fix merged and verified.

## Relationships

Bugs are usually standalone. They may reference the story or epic that introduced the defect via `parent`, or leave `parent: null`.

## Example

```markdown
---
name: "OAuth token refresh fails silently on slow connections"
type: bug
status: investigating
severity: high
parent: null
---

## Steps to Reproduce

1. Sign in with Google on a throttled connection (simulated 3G, ~500ms RTT)
2. Let the session sit idle until the access token's 1-hour expiry
3. Navigate to any authenticated route

## Expected

The client refreshes the token silently and loads the requested page.

## Actual

The refresh request times out at 5 seconds. The UI shows the authenticated
state as if signed in, but subsequent API calls fail with 401. No error is
surfaced to the user. The user appears logged in but nothing works.

## Environment

- Version: 2026.4.3
- Browser: Chrome 131, Firefox 133 (both reproduce)
- Network: simulated 3G via Chrome DevTools
- Backend: production and staging both reproduce

## Notes

The 5-second timeout is hardcoded in `src/auth/refresh.ts:42`. On slow
connections it's not long enough; on failure it silently returns the old
token without surfacing the error.
```

## Useful actions

### Create

Through [survey](../skills/survey.md) when a report comes in, or directly during [build](../skills/build.md) when an engineer notices something.

### Investigate

Advance to `investigating` while reproducing and characterizing. When the root cause is found, advance to `fix_in_progress` and implement the fix.

### Fix via build

[Build](../skills/build.md) or the developer agent picks up a bug in `fix_in_progress` much like a story: write a failing test that captures the defect, make it pass, verify.

### Assess

```
Assess this bug.
```

[Assess](../skills/assess.md) can stress-test severity, surface related failure modes, and identify whether the fix should be narrow or structural.

## Tips

- **Reproducible or flagged as unreproduced.** An unreproduced bug is a hypothesis; say so.
- **Expected vs actual.** "It's broken" fails this test. Concrete statements don't.
- **Honest severity.** Don't inflate to get attention; don't minimize to avoid work.
- **Severity is not priority.** Both matter; don't conflate them.
- **Reproduction steps first, hypothesis second.** The steps survive the fix; the hypothesis may not.
- **Write the failing test before the fix.** The test is the bug; the fix makes it pass.

## Related

- [Chore](chore.md) — for maintenance work that isn't a defect.
- [Enhancement](enhancement.md) — for small improvements that aren't defects.
- [Story](story.md) — if the fix is large enough to warrant one, promote it.
- [Survey skill](../skills/survey.md) — triage incoming bugs.
- [Build skill](../skills/build.md) — implement the fix.
- [Artifacts overview](README.md)
