# security-lead

**Type:** autonomous agent. **Purpose:** thorough security analysis — threat modeling, code audit, security test design, persistent risk registry.

The interactive [`/secure`](../skills/secure.md) skill walks the attack surface of a specific change with the engineer. The security-lead agent does deeper, autonomous work: full-system threat models, codebase audits, security test suites, and a living risk registry that tracks identified risks across the project.

## When to reach for it

- **A new authentication system is being designed.** A full threat model, not a fast scan.
- **You're auditing a subsystem.** Deep read of authentication, authorization, input handling, data protection, logging, dependencies, configuration.
- **You're building a security test suite.** The agent designs and implements tests grounded in identified threats.
- **You need a risk registry.** Identified risks, mitigations, acceptance decisions tracked over time.
- **A PR touches high-risk areas significantly.** The [reviewer](reviewer.md) does a fast security scan; security-lead does the deeper read when the change warrants it.

## When to use this agent vs the reviewer's security dimension

| Situation | Use |
|-----------|-----|
| PR with a new API endpoint | [reviewer](reviewer.md) — fast security check as part of 6-dimension review |
| New authentication system being designed | **security-lead** — full threat model |
| Periodic security audit of the codebase | **security-lead** — systematic review |
| PR touching auth, payments, or user data | **Both** — reviewer for the PR, security-lead if the change is significant |
| Building a security test suite | **security-lead** |
| Maintaining the risk registry | **security-lead** |

## What it does

### Mode: design review (threat modeling)

For a spec or design before implementation. The agent reads the spec and relevant code, identifies threat actors, analyzes attack surface (data entry points, auth/authz boundaries, data storage and transmission, external integrations, error handling), identifies risks with concrete attack scenarios, recommends mitigations, and adds entries to the risk registry.

### Mode: code audit

For a subsystem or feature area. The agent systematically reviews:

- **Authentication.** Protected routes actually protected; token lifecycle; session management; password hashing.
- **Authorization.** Permission checks at every access point (not just UI); horizontal and vertical privilege escalation paths; consistent across API, direct DB, background jobs.
- **Input handling.** Validation; parameterized queries; output encoding; command injection; path traversal; file upload safety.
- **Data protection.** Encryption at rest and in transit; PII handling; secrets management; logging confidentiality.
- **Dependencies.** Known vulnerabilities; outdated; concerning maintenance.
- **Configuration.** Debug mode; CORS; security headers; rate limiting.

### Mode: security tests

For testing identified threats. The agent designs and implements tests for authentication (access without auth → 401, expired/invalid tokens → 401), authorization (user A → user B's resources → 403, regular → admin endpoints → 403), input validation (SQL injection, XSS, path traversal, oversized inputs), data protection (sensitive fields not in responses unless authorized, passwords as hashes), log confidentiality (no raw values in logs, auth headers stripped, error responses don't leak internal details). Each test documents what it guards against and links to a registry entry.

### Mode: risk registry management

The agent maintains a persistent registry at `docs/development/security/risk-registry.md` (when `etak-develop` is installed) or `docs/security/risk-registry.md` (standalone). Each risk has a category, severity, status, affected areas, attack scenario, mitigation, and last-reviewed date. Resolved risks move to the resolved section; accepted risks require explicit rationale.

## What it doesn't do

- **Generic OWASP scans.** Threat models are grounded in *this* code, *this* data, *this* trust boundary.
- **Compliance attestation.** SOC 2, HIPAA, PCI-DSS are formal frameworks with their own requirements. The agent flags what looks like a gap; the formal work belongs elsewhere.
- **Implement mitigations.** Mitigations get implemented in [`etak-develop /build`](../skills/build.md). The agent surfaces and tracks; the engineer or developer agent fixes.
- **Block on everything.** Critical and high need immediate attention; medium and low can be tracked and addressed on schedule.

## How it operates

- **Concrete over theoretical.** "The `user_id` parameter on line 45 of `api/reviews.ts` is interpolated into a SQL query without parameterization" beats "there might be injection risks."
- **Attack scenarios are the test.** If you can't describe how an attacker would exploit a finding, reconsider whether it's a real risk or a theoretical concern.
- **Severity is impact × likelihood.** A critical issue requiring physical server access is less urgent than a medium issue exploitable by any authenticated user.
- **The risk registry is institutional memory.** It prevents forgetting accepted risks or assuming mitigations are still in place.
- **Security tests outlast reviews.** A review is a point-in-time assessment; tests guard continuously.
- **Defense in depth.** Layer controls — if input validation fails, parameterized queries catch SQL injection.
- **Calibrate scope.** A TODO app doesn't need the same scrutiny as a payment system.

## How to invoke

Ask in plain language. Examples:

```
Threat model the notification system spec.
```

```
Audit the authentication system.
```

```
Build security tests for the payment flow.
```

```
Show the active risk registry.
```

```
Deep security review of PR #142.
```

The agent routes to the appropriate mode (design review, code audit, security tests, registry management, or deep PR review), runs the analysis, and reports back with findings and registry updates.

## Tips

- **Run threat modeling at the spec stage when you can.** Threats caught before implementation are cheaper than threats caught at PR.
- **For routine PR security checks, use [reviewer](reviewer.md).** Security-lead is the deeper read; reviewer is the fast scan as part of the six-dimension review.
- **The registry is for the team.** Accepted risks with explicit rationale prevent the team from re-litigating them every quarter.
- **Pair with [devops](devops.md) for IAM and network review.** Some threats live in the application; some live in the infrastructure.
- **Don't dispatch on every PR.** Calibrate to risk. Most PRs don't open new attack surface.

## Related

- [Deliver overview](../deliver.md) — where security-lead fits.
- [`/secure`](../skills/secure.md) — the interactive alternative for change-scoped threat modeling.
- [reviewer](reviewer.md) — fast security scan as one of six dimensions.
- [devops](devops.md) — infrastructure security review counterpart.
