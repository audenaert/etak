# secure

**Stance:** adversarial security partner. **Purpose:** walk the attack surface a change opens and surface what an adversary would actually do.

Secure is the move where someone asks: *if a real attacker had time and motivation, how would they exploit this?* It's grounded in the specific change, the specific data, the specific trust boundary — not in generic OWASP categories.

This skill is the collaborative version. The autonomous counterpart is the [security-lead agent](../agents/security-lead.md), which goes deeper: full audits, persistent risk registry, security test suites.

## When to reach for it

- **A change opens new attack surface.** A new endpoint, a new service-to-service call, a new file write, a new place that reads from disk or network — anywhere untrusted data crosses a boundary.
- **You're modeling threats during design.** Before implementation, when changes are cheap, walk the design. The earliest valuable threat model happens before the spec ships.
- **A reviewer flagged security concerns and you want a deeper read.** The reviewer agent does fast scans; secure does the deeper analysis when the change warrants it.
- **You want to know what could go wrong.** Specifically — not generically.

Trigger phrases: *threat model this*, *security review*, *is this safe*, *what could go wrong*, *audit this for security*, *attack surface*, *risk audit*.

## How it behaves

Secure walks the attack surface in three moves: enumerate what the change opens, identify specific threats grounded in the change, and propose tests for the threats worth testing. Generic OWASP threats are not threats; threats grounded in *this* change, with *this* data, in *this* trust boundary, are.

The sharpest move secure makes is distinguishing the threats that follow from this specific diff from the threats that would apply to any web service. A threat list that could be cut-and-pasted into any review is not a threat model.

### Walk the attack surface

For the change, enumerate what it *opens*:
- **New trust boundaries.** New endpoints, new service calls, new file writes, new disk/network reads.
- **New data flows.** What data crosses what boundary, in what shape, with what authorization.
- **New authority.** Who can now do what they couldn't before. Privileged operations, admin paths, escalation routes.
- **New external dependencies.** New libraries, new services, new keys, new secrets.

If nothing in the diff opens new surface — pure refactor, internal renaming — secure says so. Not every change needs a threat model.

### Identify specific threats

For each new surface, walk threats grounded in the change. STRIDE is a checklist of categories — Spoofing, Tampering, Repudiation, Information disclosure, Denial of service, Elevation of privilege — but each threat is specific to *this* change.

For each threat:
- **What an attacker would do.** Concrete: "send a malformed JSON body that the parser doesn't handle, crashing the service" — not "attackers might exploit input handling."
- **What it would cost.** Data leak? Outage? Financial loss? User trust? Compliance failure?
- **What mitigates it.** Specific: input validation library X with rule Y, rate limit at Z, secret in vault not env.
- **Severity.** Critical (must mitigate before merge), High (mitigate before release), Moderate (track and address), Low (note, may accept).

If the threat is hypothetical for this codebase ("we don't accept unauthenticated traffic at this layer"), secure says so and moves on. Don't pad the model with threats that don't apply.

### Propose security tests

Threats that are real but not currently tested deserve tests. For each Critical or High threat:
- **What to test.** The specific behavior — "rejects malformed JSON with 400, doesn't crash."
- **At what layer.** Unit (parser-level), integration (endpoint-level), or E2E (full attack chain).
- **How.** Existing test framework; existing patterns. Don't introduce a new fuzzing tool unless it's earned.

When test depth gets serious — fuzzing, penetration testing, full security audit — secure names it as separate work, not as something this skill ships.

## How to use it well

**Run it during design when you can.** Threats caught at the spec stage are cheaper than threats caught at the PR stage. The earliest valuable threat model is before implementation.

**Be specific about the attacker's action.** "An attacker might exploit this" is useless. "An attacker who registers a new user can submit a sortBy parameter that runs arbitrary SQL" is actionable.

**Match severity to the realistic adversary.** A critical vulnerability that requires physical server access is less urgent than a medium vulnerability exploitable by any authenticated user. Severity = impact × likelihood.

**Hand off mitigation.** Secure proposes mitigations; [`etak-develop /build`](../skills/build.md) (when installed) implements them. Secure proposes tests; [`etak-develop /test`](../skills/test.md) writes them.

**For deeper work, dispatch the agent.** When you need a full audit, persistent risk registry maintenance, or a security test suite, the [security-lead agent](../agents/security-lead.md) takes that on.

## What secure does NOT do

- **Write the production code.** Mitigations get implemented in [`etak-develop /build`](../skills/build.md). Hand off; don't write the auth code.
- **Run a generic OWASP scan.** Generic scans go through `operate` or external tooling. Secure does threat modeling grounded in the change.
- **Compliance attestation.** SOC 2, HIPAA, PCI-DSS are formal frameworks with their own requirements. Secure flags what looks like a gap; the formal work belongs elsewhere.
- **Maintain a risk registry.** That's the [security-lead agent](../agents/security-lead.md). Durable risk tracking is autonomous work. The skill produces threats and proposed mitigations for *this* change.

## Common failure modes

- **Threat-list bingo.** Listing OWASP categories as if they were threats. Cause: the categories are right there. Fix: each item must be specific to the change; cut the rest.
- **Threats without context.** "SQL injection possible" without naming where, on which input, with which mitigation absent. Cause: thinking in categories not concretes. Fix: walk the actual code path.
- **Severity inflation.** Marking everything Critical to show rigor. Cause: defensive overrating. Fix: Critical is reserved for "must mitigate before merge"; Moderate is a real status, not a softer Critical.
- **Hypothetical attackers.** Modeling threats from APT-style adversaries on a hobby project. Cause: threat model unmoored from the actual product. Fix: who would actually attack this, what would they want, what's the realistic scenario.
- **No mitigation, no severity.** Threats listed without "what would fix this." Cause: surfacing risk without owning the resolution. Fix: every threat names a mitigation, even if the mitigation is "accept the risk because X."

## Transitions

- A threat is actually a testable belief → [**etak-discovery /sound**](sound.md) to surface it as an assumption
- Mitigation needs implementation → [**etak-develop /build**](../skills/build.md)
- Threat needs a regression test → [**etak-develop /test**](../skills/test.md)
- Recurring threat pattern across PRs → [**security-lead agent**](../agents/security-lead.md) for a deeper audit
- Infrastructure threats surface → [**operate**](operate.md) for IaC and IAM review

## Related

- [Skills index](README.md)
- [Security-lead agent](../agents/security-lead.md) — autonomous threat modeling, code audit, and risk-registry maintenance.
- [Deliver overview](../deliver.md) — how secure fits.
- [review](review.md) — fast security scan as one of six dimensions; secure goes deeper when warranted.
