# Delivery Model

Delivery is the work between "the code is written" and "the system is running, observable, secure, and documented." It is not a single phase — it's six phases that run in parallel and reinforce each other.

## The six phases

| Phase | Skill | Agent | The move |
|-------|-------|-------|----------|
| Review | `review` | `reviewer` | Read the diff against the spec and the codebase; surface what's risky, redundant, or missed |
| Verify | `verify` | `quality-engineer` | Confirm the story is actually done — ACs covered, test strategy sound, edge cases real |
| Ship | `ship` | `release-engineer` | Get to green CI, cut the release, fix what broke |
| Secure | `secure` | `security-lead` | Threat-model the change; record risks; write the security tests |
| Operate | `operate` | `devops` | Infrastructure as code, pipelines, observability, environments |
| Document | `docs` | `tech-writer` | Keep docs in sync with what the code actually does |

The phases overlap. A reviewer may surface a security concern; the security-lead may surface a missing test; the quality-engineer may surface an undocumented assumption. Each phase is an entry point and a hand-off, not a step in a queue.

## Skill ↔ agent pairing

Every phase has both an interactive **skill** and an autonomous **agent**. They are not duplicates — they have different roles:

- **Skills** pair with you in the conversation. You describe a move, the skill helps you make it. It reads, suggests, walks through, asks. The artifact (a fixed PR, a refined story, a passing test) is shaped together.
- **Agents** run on their own, do many things, return a report. You point one at a PR or a service and walk away. It posts findings to the PR, updates files, opens issues. It then summarizes what it did.

Reach for a skill when the work needs your judgment in the loop. Reach for an agent when the work has a defined output and you want to come back to it.

## Cross-plugin context

Delivery work is downstream of development and discovery. Most of what makes a delivery decision *informed* lives in artifacts those plugins produce.

- **From etak-develop:** stories (`docs/development/stories/`), specs (`docs/development/specs/`), ADRs (`docs/development/adrs/`), tasks. The branch and PR description usually name the parent story.
- **From etak-discovery:** assumptions linked via `from_discovery` on the story. Riskier assumptions point at riskier verification.

## Standalone install

Deliver works without its companions. When the upstream graphs are not installed, each skill and agent falls back to reading the codebase directly:

- The diff names what changed.
- The branch name and PR description name the intent.
- The existing tests name what's already verified.
- The CI logs name what failed.

The fallback is never silent. Each skill and agent that depends on upstream context states what it can do without it, and what it loses.

## What delivery does NOT do

- **Pre-implementation design.** That's etak-develop's `spec` and `architect`. Delivery reviews the design after it's been implemented; it does not author it.
- **Story writing.** That's etak-develop's `story` skill. Verify reads stories; it does not refine them.
- **Idea exploration.** That's etak-discovery. Delivery may surface concerns that point back to a missed assumption — but it doesn't run a discovery loop.

When delivery work uncovers a gap upstream, the right move is to name it and hand back. The graph stays clean if each plugin owns what it owns.
