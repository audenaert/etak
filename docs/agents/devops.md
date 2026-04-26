# devops

**Type:** autonomous agent. **Purpose:** design and implement infrastructure, CI/CD pipelines, containerization, observability, and environment management. Never applies to live infrastructure without explicit approval.

The interactive [`/operate`](../skills/operate.md) skill thinks through blast radius, alerting strategy, and runbook content with the engineer. The devops agent does the building: writing IaC, designing pipelines, wiring observability, creating Dockerfiles. It produces infrastructure code autonomously, then stops at apply for explicit approval.

## When to reach for it

- **A new project needs infrastructure designed and built.** The agent reads the spec (when `etak-develop` is installed) for non-functional requirements and produces IaC, pipelines, and observability.
- **A new service needs containerization.** Multi-stage Dockerfile, Compose for local dev, production deployment config.
- **CI/CD needs setting up or extending.** GitHub Actions (default), GitLab CI, CircleCI — matching whatever the project uses.
- **Observability needs wiring.** Logs, metrics, alerts, dashboards — defined as IaC, in the repo, not console-configured.
- **A new environment is being created.** dev, staging, production — promoted via the same IaC with parameter differences.
- **`tech-lead` (when `etak-develop` is installed) is dispatching infrastructure work alongside developers.**

## What it does

### Default stack

When the project has no existing infrastructure:

| Concern | Default | Alternatives supported |
|---------|---------|------------------------|
| Cloud | AWS | GCP, Azure |
| IaC | CDK (TypeScript) | Terraform, CloudFormation, Pulumi |
| Containers | Docker + Compose (local), ECS Fargate (prod) | EKS, Lambda, EC2 |
| CI/CD | GitHub Actions | GitLab CI, CircleCI, Jenkins |
| Observability | CloudWatch + SNS | DataDog, Grafana/Prometheus, New Relic |

**Always detects existing first.** If the project uses Terraform, the agent writes Terraform. Don't introduce a second IaC tool or CI system without compelling reason.

### Modes

**Infrastructure design.** For a spec or new project: compute, storage, networking, security. Output: an infrastructure section for the spec or a standalone design document.

**IaC implementation.** Writes the infrastructure code matching detected patterns. Stacks separated by lifecycle (stateful vs stateless), environment-parameterized, outputs for endpoints/ARNs, tagged consistently.

**CI/CD pipeline.** Build → test → lint → security scan → deploy. Fast feedback (smoke first, heavy later); aggressive caching; environment-specific configs via secrets.

**Containerization.** Multi-stage Dockerfiles with minimal base images, non-root user, health checks. Compose for local dev with services and `.env.example`.

**Observability.** Structured JSON logging with correlation IDs; redaction at the library level (allowlist of loggable fields); separate audit logs from operational logs; metrics for the four golden signals plus business metrics from the spec; SLO-based alerts (not symptom-based); dashboards as IaC; runbooks for every critical alert.

**Environment management.** Parameterized IaC across dev/staging/prod. Differences explicit and minimal. Drift between environments documented with rationale.

### Confirm before apply

The agent generates infrastructure code freely, but **never applies changes to live infrastructure without explicit user confirmation**. This includes `cdk deploy`, `terraform apply`, `pulumi up`, `aws`/`gcloud`/`az` CLI commands that create/modify/delete resources, Docker commands affecting running services, anything that costs money or changes production state.

The pattern: generate the IaC, show what will be created/changed (including cost implications), wait for explicit approval, apply, report.

For local-only actions (writing files, building images, running tests), the agent proceeds without confirmation.

## What it doesn't do

- **Apply silently.** Never modifies live infrastructure without explicit approval.
- **Write the production application code.** The agent works on infrastructure, pipelines, and configuration. Application code goes to [`etak-develop /build`](../skills/build.md).
- **Diagnose green-the-build CI failures.** That's [release-engineer](release-engineer.md). The agent handles structural pipeline changes (cache, runner pool, secrets), not specific failed builds.
- **Threat-model the infrastructure.** That's [security-lead](security-lead.md) with infrastructure-shaped scope.
- **Run incident response.** When prod is on fire, the runbook is the runbook. The agent writes the runbook; the responder uses it.

## How it operates

- **Detect before defaulting.** A second IaC tool or CI system is almost always wrong.
- **Confirm before applying.** Show what will change, what it costs, wait for approval.
- **Infrastructure is code.** Everything in the repo. No console-only configuration that isn't captured in IaC.
- **Design for operability.** Someone will debug this at 2am. Structured logs, meaningful alerts, clear dashboards, and runbooks make that possible.
- **Least privilege.** IAM minimums. The one place over-engineering (more specific policies) is correct.
- **Tag everything.** Project, environment, managed-by, cost-center. Non-negotiable.
- **Cost awareness.** Mention cost implications when creating resources.
- **Don't gold-plate.** What the application needs today, with clean extension points.

## How to invoke

Ask in plain language. Examples:

```
Set up CI for this project.
```

```
Create a staging environment.
```

```
Add observability to the notification service.
```

```
Containerize this app.
```

```
Write the IaC for the new API.
```

The agent detects the existing stack (or applies defaults), runs the appropriate mode, generates the code, shows what will change with cost implications, and waits for approval before applying anything that affects live infrastructure.

## Tips

- **Run during design when you can.** Infrastructure decisions have long shadows. Designing during spec is cheaper than retrofitting at deploy time.
- **For CI failures, dispatch [release-engineer](release-engineer.md), not devops.** The release-engineer fixes individual builds; devops fixes the pipeline shape that produces them.
- **Pair with [security-lead](security-lead.md) for IAM and network review.** IAM policies and security groups are where infrastructure security lives.
- **Approve incrementally.** When the agent proposes a large IaC change, ask it to apply in stages — stateful resources first, stateless after.
- **Read the runbook before approving the alert.** A new alert without a clear runbook is a future page nobody can handle.

## Related

- [Deliver overview](../deliver.md) — where devops fits.
- [`/operate`](../skills/operate.md) — the interactive alternative.
- [release-engineer](release-engineer.md) — handles individual CI failures; devops handles pipeline architecture.
- [security-lead](security-lead.md) — IAM and network security review.
