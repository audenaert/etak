# Ready Check

A readiness check is a gate: is this work item well-defined enough to begin?
It's a quality check, not a bureaucratic hurdle. A small, obvious story with
three testable ACs is ready even if it doesn't have formal tasks.

## Running the check

1. **Load the target** — the work item, its parent, its children if any,
   linked specs/ADRs/spikes, and any discovery link.
2. **Run the type-appropriate checklist** (below).
3. **Produce a traffic-light verdict** with specific gaps named.
4. **Offer to fix** what's fixable — vague ACs rewritten, missing tasks
   created, status promoted.

## Story readiness

| Check | What to verify | Weight |
|-------|---------------|--------|
| **Acceptance criteria** | Exist, testable, cover happy path + key edge cases | Critical |
| **Scope clarity** | Clear what's in and out | Critical |
| **Implementability** | A developer could pick this up and know what to build | Critical |
| **Dependencies resolved** | No unresolved blockers; prerequisites done or in flight | Critical |
| **Testable language** | No weasel words: "properly", "correctly", "appropriate", "good" | High |
| **Context sufficient** | Links to parent, spec, or discovery | Medium |
| **Tasks identified** | Either present or the story is small enough to not need them | Low |
| **Spikes resolved** | Related spikes complete and findings incorporated | High |

## Epic readiness

| Check | What to verify | Weight |
|-------|---------------|--------|
| **Stories identified** | Child stories exist and cover the epic's scope | Critical |
| **Scope defined** | Clear in/out boundaries | Critical |
| **Story readiness** | What proportion of child stories are themselves ready? | High |
| **Workstream assigned** | Epic belongs to a workstream (if project has workstreams) | Medium |
| **Milestone assigned** | Epic targets a milestone | Medium |

## Project readiness

| Check | What to verify | Weight |
|-------|---------------|--------|
| **Breakdown exists** | Workstreams and/or epics defined | Critical |
| **Milestones defined** | At least M1 with demo criteria | Critical |
| **Spec exists** | Technical design documented (for non-trivial projects) | High |
| **Key decisions made** | No blocking ADRs stuck in `proposed` | High |
| **Risks identified** | Pre-mortem has been run, or top risks noted | Medium |
| **Dependencies mapped** | External and cross-workstream dependencies identified | Medium |

## Verdicts

- 🟢 **READY** — all critical checks pass, no high-weight issues
- 🟡 **ALMOST READY** — critical checks pass, high-weight gaps that can be
  resolved quickly
- 🔴 **NOT READY** — one or more critical checks fail

## Common readiness failures

- **Vague acceptance criteria.** The single most common failure. Train your
  eye for: "properly", "correctly", "as expected", "appropriate", "good user
  experience."
- **Missing alternatives in a spec.** A design with one approach named is a
  memo, not a decision.
- **Open spike past time-box.** Either extract a finding, accept a partial
  answer, or consciously extend — don't ignore it.
- **AC avoidance because "it's obvious."** If it's obvious, writing it takes
  30 seconds. If it's not obvious, you just saved three days.

## After the check

If all critical checks pass and the status is `draft`, offer to update the
status to `ready`. Don't update silently.

If the verdict is 🔴 or 🟡, list the fixes as concrete next actions:
- "Rewrite AC #3 to specify what the user sees on error"
- "Wait for `spike-evaluate-oauth-libs` to complete, or accept the risk"
- "Want me to generate a test plan via `/test`?"
