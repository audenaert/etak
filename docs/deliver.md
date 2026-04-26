# Deliver

The `etak-deliver` plugin handles the work that turns a green local build into safely running production. Where `etak-develop` builds the change, `etak-deliver` does the parallel things that have to happen *around* the change before and after it ships: code review, story verification, CI/release engineering, security review, infrastructure and operations, and documentation.

These six surfaces are not a sequential pipeline. A team uses some of them on every PR (review, verify), others when the work warrants it (secure for high-risk changes), and others continuously (operate, docs as drift accumulates).

## The six skills

| Skill | Stance | The move it supports |
|-------|--------|----------------------|
| [**review**](skills/review.md) | Rigorous senior colleague | Self-review your own work, run a structured review on someone else's PR, respond to feedback |
| [**verify**](skills/verify.md) | Adversarial QE | Confirm a story is actually done — walk each AC against the implementation, check coverage at the right layer |
| [**ship**](skills/ship.md) | Pragmatic release engineer | Diagnose CI failures, fix them, cut releases, draft notes in the user's frame |
| [**secure**](skills/secure.md) | Adversarial security partner | Walk the attack surface a change opens, identify specific threats, propose tests |
| [**operate**](skills/operate.md) | Structural ops partner | Read existing infrastructure before proposing change, preview blast radius, never modify live infra silently |
| [**docs**](skills/docs.md) | Receptive technical writer | Audit drift between code and docs, generate new docs, update existing pages |

## The rhythm

Deliver does not have a single rhythm. Each skill maps to a moment that recurs naturally in the engineer's work:

- **You finished implementing** → review (self-review before pushing)
- **The PR is open and ACs are listed** → verify
- **CI is red, or the change is ready to release** → ship
- **The change opens new attack surface** → secure
- **The change touches infrastructure or needs observability** → operate
- **The change altered behavior the docs describe** → docs

You usually invoke them in plain language. Describe what you want done; the tool routes.

## How to choose a deliver move

- **You want to know if your code is ready to push** → review (self-review)
- **A PR needs review** → review (PR review)
- **You want to confirm a story is done** → verify
- **CI is broken or you want a release cut** → ship
- **You want to know what could go wrong if an attacker tried** → secure
- **You need infrastructure, monitoring, or a runbook** → operate
- **The docs and the code disagree** → docs

## The six agents

Agents do longer-running autonomous work. You hand them a body of work and review the result.

- [**reviewer**](agents/reviewer.md) — autonomous code review across six dimensions (problem fit, simplicity, critical zones, boundaries, security, tests). Posts findings to the PR.
- [**quality-engineer**](agents/quality-engineer.md) — verifies a story autonomously. Walks each AC, evaluates test coverage at the right layer, optionally writes E2E tests to close gaps, posts verdict on the PR.
- [**release-engineer**](agents/release-engineer.md) — diagnoses and fixes CI failures from logs; cuts releases when ready, drafts notes in the user's frame.
- [**security-lead**](agents/security-lead.md) — autonomous threat modeling, code audit, security test design, and a persistent risk registry.
- [**tech-writer**](agents/tech-writer.md) — autonomous documentation drift correction across API docs, architecture, user docs, and screenshots.
- [**devops**](agents/devops.md) — designs and implements IaC, CI/CD, containerization, observability, and environment management. Never applies to live infrastructure without explicit approval.

Each agent pairs with a skill. The skill is the collaborative version (you in the loop, deciding); the agent is the autonomous version (it runs the work and reports back). Same shape, different interaction model.

## What deliver does NOT do

Deliver works on the change after the design is set. Things that belong elsewhere:

- **Specs, ADRs, scoping, sequencing** → [`etak-develop`](develop.md)
- **Implementation (writing the production code)** → [`etak-develop`](develop.md)
- **Discovery and validation** → `etak-discovery`
- **UX and interaction design** → `etak-design` (planned)

Deliver agents may *invoke* etak-develop's `developer` to make a code change when their work surfaces one (for example, the `quality-engineer` agent finds a missing implementation, or the `reviewer` finds a security issue that needs fixing). That dispatch only happens when `etak-develop` is installed; otherwise the deliver agent surfaces the issue and the engineer decides what to do.

## Standalone install vs combined

Deliver works on its own. You can install only `etak-deliver` and use review, verify, ship, secure, operate, and docs as a focused quality and release toolkit. When `etak-develop` is also installed, the deliver skills and agents pick up additional context: the linked story, the spec, the ADR, the work-item graph. Each cross-plugin reference has a fallback — if `etak-develop` isn't there, the deliver skill or agent reads the diff, the PR description, or asks the engineer interactively.

`etak-discovery` integration is lighter. When discovery context is available, the riskiest assumptions inform what to verify hardest and what threats to model deepest. When it isn't, deliver works directly off the change.

## Future

The deliver plugin currently exposes skills and agents but does not yet maintain its own persistent artifact graph (a risk registry, a release log, a runbook index). A v0.2 may introduce internal artifact writers — the same pattern `etak-develop` uses for stories, specs, and ADRs. For now, where artifacts make sense, the skills and agents write them in conventional locations (`docs/development/security/risk-registry.md`, release notes alongside tags) without enforcing a schema.

## Read next

- [**Skill definitions**](../deliver/skills/) — each skill's full specification.
- [**Agent definitions**](../deliver/agents/) — each agent's full specification.
- [**Develop overview**](develop.md) — the upstream plugin deliver pairs with most often.
