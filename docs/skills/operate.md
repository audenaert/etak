# operate

**Stance:** structural ops partner. **Purpose:** read what exists before proposing change; preview blast radius before applying; never modify live infrastructure silently.

Operate is the move where someone thinks carefully about infrastructure, pipelines, observability, and environments. It's pragmatic about what's already deployed (read first; propose changes that compose with reality) and structurally cautious about what's risky (preview the plan; explicit approval before apply).

This skill is the collaborative version. The autonomous counterpart is the [devops agent](../agents/devops.md), which takes longer-running infrastructure work end-to-end.

## When to reach for it

- **A new endpoint is going to prod.** What does success look like, and how would we know if it stopped? That's the dashboard. Operate makes sure observability is wired up before launch.
- **A new resource needs IaC.** Operate reads existing modules first, then proposes changes that match the project's patterns.
- **Infrastructure is changing in a way that needs preview.** Plan before apply, blast radius named, explicit approval before anything mutates live state.
- **A runbook needs writing.** When a new alert fires, the responder needs steps to diagnose and recover. Operate writes the runbook entry.
- **Environments are drifting.** Differences that should be in tfvars are in shell scripts no one remembers. Operate reconciles.

Trigger phrases: *set up CI*, *fix the pipeline*, *add observability*, *containerize this*, *deploy this*, *infrastructure*, *monitoring*, *alerts*, *runbook*, *environment config*.

## How it behaves

Operate reads what exists before proposing change. Existing IaC modules, existing dashboards, existing alerts, existing pipelines — these are the patterns the change should compose with. A proposal that ignores what's deployed is a proposal that introduces drift.

The sharpest move operate makes is noticing when a proposed change is "what should exist" rather than "what does." Infrastructure work is built on what's actually deployed, not what the diagram says.

### The four operating areas

**Infrastructure as code.** For new resources, modify or extend existing IaC. For changes to running resources, generate the plan first (`terraform plan`, `cdk diff`, `pulumi preview`) and review it before applying. Name the blast radius — will this change in-place mutate something stateful? Cause downtime? Is the change reversible? Apply only with explicit approval.

**CI/CD pipelines.** For new pipelines, model on existing ones. For broken pipelines, hand off to [`ship`](ship.md) — operate's role is structural fixes (resource limits, runner config, caching strategy), not green-the-build. Pipelines should be reproducible (same inputs, same outputs), fail visibly (no silent skips), and be readable (six-deep matrix builds belong in a refactor).

**Observability.** New endpoints get observability before they go live. Metrics (latency, throughput, error rate at minimum, plus business metrics if the spec calls for them); structured logs with request/trace ID for correlation, no secrets; tracing if the system has multiple services; SLO-based alerting (not symptom-based — "p99 latency > target for 5min" beats "any error in last 5min"); dashboards linked from the runbook.

**Environments and runbooks.** Environments promoted via the same IaC, not hand-edited. Differences live in tfvars or equivalent, not in shell scripts. Runbooks are living documents — when an alert fires, the runbook names the steps to diagnose and recover. Secrets live in a vault, not in `.env` files committed to git.

## How to use it well

**Read existing modules before proposing new ones.** If the project uses Terraform, write Terraform. If it uses CDK, write CDK. Don't introduce a second IaC tool without compelling reason.

**Preview before apply, every time.** `terraform plan`, `cdk diff`, `pulumi preview` — show what would change before applying. Get explicit approval. Never modify live infrastructure silently.

**Wire observability before launch.** A new endpoint shipping without a dashboard is an endpoint nobody can debug at 2am. Ask the right question: "What does success look like, and how would we know if it stopped?"

**Alert on SLO breaches.** "Any error in the last 5 minutes" alerts get muted. "p99 latency > 500ms for 5 minutes" alerts mean something. Calibrate.

**Write the runbook when you write the alert.** A new alert without a runbook entry is a future page nobody knows how to handle.

## What operate does NOT do

- **Write the production application code.** Operate works on infrastructure, pipelines, and configuration. Application changes go to [`etak-develop /build`](../skills/build.md).
- **Diagnose green-the-build CI failures.** That's [`ship`](ship.md). Operate handles structural pipeline changes (cache, runner pool, secrets), not specific failed builds.
- **Threat-model the infrastructure.** That's [`secure`](secure.md) with infrastructure-shaped scope. Operate flags concerns; secure runs the model.
- **Run incident response.** When prod is on fire, the runbook is the runbook. Operate writes the runbook; the responder uses it.

## Common failure modes

- **Apply without preview.** Modifying live infra without generating the plan and showing it. Cause: speed temptation. Fix: always plan, always show, always get explicit approval.
- **Greenfield design.** Proposing IaC that ignores what's already deployed. Cause: didn't read existing modules. Fix: read first, propose changes that compose.
- **Symptom alerts.** "Any error" alerts that fire constantly and get muted. Cause: alerting on what's easy, not what matters. Fix: alert on SLO breaches and known bad patterns; everything else is a dashboard, not an alert.
- **Runbook drift.** Runbook describes a service shape that no longer exists. Cause: ops change without doc update. Fix: when you change the system, you change the runbook.
- **Manual environment drift.** Hand-edits in dev/staging/prod that don't go back into IaC. Cause: "I'll fix it later." Fix: every change goes through IaC, even the small ones.
- **Secrets-in-env.** Secrets committed to a `.env`. Cause: convenience. Fix: any secret in a file that could end up in git is an issue, not a config.

## Transitions

- Recurring CI flake → [**ship**](ship.md) for diagnosis, then operate for structural pipeline changes
- Infrastructure change exposes new attack surface → [**secure**](secure.md) before applying
- New endpoint without observability → propose adding it before merge ("This endpoint is going to prod with no dashboard. Want to wire one up?")
- Runbook drift → [**docs**](docs.md) to sync
- Long-running infra build → [**devops agent**](../agents/devops.md) for autonomous IaC implementation

## Related

- [Skills index](README.md)
- [Devops agent](../agents/devops.md) — the autonomous version of this skill.
- [Deliver overview](../deliver.md) — how operate fits.
- [ship](ship.md) — handles individual CI failures; operate handles pipeline architecture.
