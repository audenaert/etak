# Getting started (develop)

**Time:** about thirty minutes. **Prerequisite:** `etak-develop` installed ([develop overview](../develop.md)). A codebase you can write against, ideally one with a web API, some middleware, and a test suite you already trust.

You will walk a ready story through the develop loop: survey the graph, assess the story, ground a spec, record one ADR, plan tests, pair through implementation, self-review, and open the PR. By the end you will have a working mental model of the six develop skills and a PR whose description links back to the story, the spec, the ADR, and, if present, the discovery idea.

## Questions to sit with before you start

Spend two minutes on each. Write your answers down.

1. **The last story you shipped that needed rework in review.** What was missing from the story or the spec that, if it had been present, would have prevented the rework?
2. **When was the last time you wrote an ADR?** Do you have a canonical place to look for "why did we do it this way" on a system you didn't build? What happens when the answer isn't there?
3. **How do you know, before you start coding, that a story is actually ready to build?** Is "ready" a checklist you trust, a gut call, or a status field nobody maintains?

Keep these in mind. The skills will put pressure on all three.

## Scenario

You're an engineer on a platform with a REST API behind rate-limiting middleware. The product has just introduced an enterprise tier, and the story in front of you is to exempt enterprise customers from the default per-IP rate limits.

The story exists. Its ACs are in the graph under `docs/development/stories/`. The codebase has middleware, an auth module, and a config loader. The work is small enough to finish in one PR, and non-trivial enough that the design is worth a pass before you write code.

Open Claude Code at the root of your repo. The examples below assume a rate-limit story; any small-but-not-trivial backend story works.

## Part 1. See the work graph

Type:

```
What's ready to build?
```

The [survey](../skills/survey.md) skill reads `docs/development/` and leads with one or two stories you could actually pick up now. It names why each is a good candidate: small, unblocks downstream work, serves the next milestone. It does not dump the backlog.

Pick the rate-limit exemption story. If survey flags another story as more urgent, hear it out. Push back or accept; the conversation is the value.

**What just happened.** Survey wrote nothing. It reflected the graph back so you could choose where to spend the next thirty minutes. Home base for the develop plugin is not a dashboard; it is a short, opinionated conversation about what matters.

## Part 2. Assess readiness

Before you start, ask:

```
Is this story ready to build?
```

The [assess](../skills/assess.md) skill runs a ready check. It reads the ACs, looks for testable assertions, checks for dependencies, and returns a traffic-light verdict: ready, almost ready, or not ready. Each "not ready" finding names the specific gap.

Expect at least one finding. Typical shapes: an AC that says "enterprise customers are not rate-limited" without naming which endpoint or which identifier the exemption keys off; an AC that doesn't say what happens when a user's tier changes mid-session; a missing mention of the audit trail the security team will ask about.

You have two moves. Fix the findings, by rewriting the ACs with assess's help, or accept the story as-is and note the risk. Both are legitimate. The trap is the third move: skipping the check.

**What just happened.** Readiness got externalized. It is no longer a gut call or a status field nobody trusts; it is a specific list of what's missing, rendered against the artifact you are about to build from. Pay the cost now. It is cheaper than discovering the gap mid-implementation.

## Part 3. Ground a spec

Now ask:

```
How should we build this?
```

The [spec](../skills/spec.md) skill reads code before it writes prose. Watch what it cites. In the rate-limit scenario it will likely surface three files: your rate-limit middleware, your auth module (for reading tier or claim), and the config loader (for any defaults or overrides). Read what it says about each one.

Then read the proposed approach. Two tests:

- Does the design fit the shape of the existing code, or is it a clean-room architecture that ignores the patterns neighbors use?
- Are the alternatives real? Two or three options someone on the team would actually argue for, not one proposal plus two strawmen?

If either test fails, push back. "This proposes new middleware, but the existing middleware already reads the auth context; can we extend it instead?" The spec will reread and revise. That exchange is where the spec earns its weight.

**What just happened.** The design got grounded in the repo you actually have, not a repo the tool invented. A spec that cites `src/middleware/rate-limit.ts:47` and `src/auth/context.ts:23` is harder to be wrong in ways that only surface at PR review. This is the antidote to architecture-astronaut proposals.

## Part 4. Record an ADR for one hard choice

Something in the spec required a real call. Maybe it's where the exemption is evaluated (middleware vs. handler). Maybe it's how enterprise-tier membership is represented (a claim on the JWT, a field on the user, a lookup against a separate service). Maybe it's whether the exemption applies per-endpoint or globally.

Ask:

```
Record an ADR for this decision.
```

The spec skill applies the hard-to-reverse test. Would swapping this choice cost weeks later? Does it affect how the team writes similar code from now on? If yes, it writes an [ADR](../artifacts/adr.md) with four sections: context, decision, alternatives considered, consequences. Read the consequences section carefully. It is the section most ADRs skip and the one future readers care about most.

If the answer to the hard-to-reverse test is no, spec says so and leaves the decision in the spec body. Not every choice is an ADR. Ceremony is not rigor.

**What just happened.** A decision that would otherwise live only in the PR description got written into `docs/development/adrs/` with a stable filename and an immutable record. Six months from now, when someone asks why enterprise exemptions live in middleware instead of handlers, the answer is a file, not a Slack scroll.

## Part 5. Plan tests before code

Ask:

```
Plan tests from the ACs.
```

The [test](../skills/test.md) skill reads the ACs and derives concrete cases. For each case, it names the layer: unit, integration, API, or E2E. Read the plan before writing anything.

Two questions to ask:

- Is each AC actually covered, including the unhappy paths (tier downgrade, missing claim, misconfigured exemption list)?
- Is each case at the cheapest honest layer? A unit test that exercises the middleware's tier check is cheaper and more precise than an E2E test that hits the whole stack to verify the same behavior.

If the test skill flags an AC as ambiguous, that is a refinement finding. Hand it back to assess, or resolve it directly. Don't invent a case for what you assume the feature should do.

**What just happened.** The test plan became the executable version of the story. When you write the tests, you are writing the ACs in a form the suite will enforce forever. The moment to catch "this AC is ambiguous" is now, not when the review comment lands.

## Part 6. Pair through implementation

Now:

```
Let's build this.
```

The [build](../skills/build.md) skill picks up the ready story, reads the adjacent code, and pairs through the implementation. For logic with edge cases, the inner loop is TDD: write the failing test, watch it fail for the right reason, implement, watch it pass, commit at the AC boundary, move on.

TDD here is not dogma. It is how the tool stays honest. A test that is written and seen to fail is a test that was run before the implementation existed. A test written after the code it covers will often pass for the wrong reason. The red-green cycle is the evidence that the coverage is real.

For pure wiring (registering a middleware, reading a config key) TDD ceremony is not required. Build uses judgment. If build starts to drift into a second subsystem that wasn't in the story, expect it to name the drift: "this is starting to touch the billing module; is that the story, or are we growing scope?" Trust that signal.

**What just happened.** Each AC landed as its own commit, backed by a test written first. The PR you are about to open has atomic commits, a suite that passed after each one, and zero scope drift. That combination is what makes the PR reviewable in an hour instead of a day.

## Part 7. Self-review

Before pushing:

```
Self review.
```

Build runs the pre-flight checks: lint on changed files, focused tests, broader suite for regressions, type checking, AC coverage scan, and a clarity pass on long functions, missing error handling, committed secrets, or unexplained TODOs.

The verdict is specific. "Ready to push," "almost ready" (minor gaps addressable in minutes), or "not ready" (failing tests, missing AC coverage, real issues).

Read the "not done" signal carefully. It is more honest than the "done" signal. A tool that has been fine-tuned to agree with you will find a way to say done. A tool that says "AC #3 has no corresponding test" when AC #3 has no corresponding test is giving you the report you wanted before you pushed. Address the gaps. If the gap is the story rather than the code, go back to assess and refine the AC.

**What just happened.** The "done" declaration got externalized, against the same ACs you assessed at the start. The feedback loop between story and PR closed in the place where it is cheapest.

## Part 8. Open the PR

When self-review is clean:

```
Open a PR.
```

Build creates the PR with a description structured for a reviewer who was not in the room. One-to-three-sentence summary. Changes grouped by purpose, not by file. An AC checklist that links back to the story. Testing notes including any manual verification. Questions for the reviewer where you are genuinely uncertain.

Read the description. Notice what it links to. The story at the top. The spec that grounded the design. The ADR that captured the hard choice. If the story has a `from_discovery` field, the validated discovery idea upstream of it. That chain of links is the answer, six months from now, to "why is this in production?"

Push. The PR is up.

## What just happened

You walked a ready story through seven moves: survey, assess, spec, ADR, test plan, build, self-review, PR. Five skills did work on the artifact graph (assess, spec, test, build; survey only read it). One skill produced a record that will outlive the PR (the ADR).

Five things to notice:

- **"Ready to build" got externalized.** It is no longer a status field or a gut call. It is a specific list of findings you addressed or accepted.
- **The spec cited real files.** The design fits the shape of the codebase because the skill read the codebase before proposing anything. The alternatives were real.
- **TDD was evidence, not ceremony.** The failing test before the implementation was the thing that proved the test covers the behavior.
- **Self-review named the gaps honestly.** The tool's "not done" signal is sharper than its "done" signal. Trust that asymmetry.
- **The PR links backward through the full trail.** Story, spec, ADR, discovery idea. That trail answers questions no PR description alone can answer.

## Go back to your original questions

The three questions from the top of this page. Answer them again, in the language of the artifacts you just touched.

- **What was missing from the last story that needed rework?** Whatever it was, assess would have flagged some shape of it. Gaps in ACs, missing spec anchors, unresolved dependencies. The finding would have let you address it on paper.
- **When was the last time you wrote an ADR?** You just wrote one. It took five minutes. It will save a meeting in six months.
- **How do you know a story is ready?** You run the readiness check and read what it surfaces. "Ready" is a verdict with findings, not a status anyone has to trust.

That is the delta `etak-develop` is trying to produce.

## What to try next

- **[Develop overview](../develop.md)** — the six skills and how they compose.
- **[Survey](../skills/survey.md)**, **[assess](../skills/assess.md)**, **[spec](../skills/spec.md)**, **[test](../skills/test.md)**, **[build](../skills/build.md)** — each skill's full spec. Read the one that surprised you most during this walkthrough.
- **[Story](../artifacts/story.md)**, **[spec](../artifacts/spec.md)**, **[ADR](../artifacts/adr.md)** — the artifact shapes you just produced. The YAML frontmatter is the whole data model.
- Run the flow above on a real story sitting in your backlog right now. That is when the tool starts to pay for itself.
