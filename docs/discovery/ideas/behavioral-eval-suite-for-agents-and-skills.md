---
name: "Behavioral eval suite for Etak agents and skills"
type: idea
status: draft
addresses:
  - hmw-prevent-agent-skill-behavioral-regressions
---

## Description

A test harness — `tests/` in the etak repo — that exercises each agent and skill against canonical scenarios and asserts on behavioral expectations. When a prompt is edited, the suite catches drift before the change ships to users.

Two layers of assertion:

- **Structural** — directly checkable from generated artifacts: write paths conform to the schema, frontmatter parses and matches the expected shape, required fields are present, link slugs resolve, ADR numbering is sequential, opportunities are prefixed `hmw-`, etc.
- **Behavioral** — checkable from the agent's response or generated content: does the architect surface a path conflict when the brief prescribes a non-conforming path? Does the developer agent refuse to start when ACs are missing? Does the spec skill push back when the engineer skips codebase reading?

Both are scenario-driven: a fixture provides the scenario inputs (a sample brief, a sample story, a synthesized codebase state); the agent or skill runs against it; the suite asserts on the outputs.

## Why this could work

- The ground we're protecting is small (~10 agents, ~20 skills) and stable in shape — most edits are wording refinements, not surface-area expansions.
- Path conformance is mechanically checkable against `_internal/core/references/schemas.md` — the schema is already the source of truth.
- Frontmatter is YAML; pyyaml or similar parses and asserts directly.
- Refusal and branching behavior can be tested by designing prompts that should trigger specific paths and asserting on the agent's output.
- The recent incident (PR #85 on forge-app) is itself a test scenario: a brief that prescribes the wrong path should produce a path-conflict surface, not silent compliance. That assertion is mechanical.

## How users would experience it

The "user" here is the Etak maintainer and contributors. Their experience:

- Open a PR that edits an agent or skill prompt.
- CI runs the eval suite against the changed surface area.
- Failures show: which scenario tripped, which assertion fired, what the agent produced versus what was expected.
- Green CI gives confidence to merge. Red CI surfaces drift before it reaches a dogfooder.

Downstream, dogfooders benefit indirectly: regressions that would have shipped don't.

## Open questions

1. **Sandboxing agent runs for tests.** Real Claude API calls? Cached/recorded responses? A subset of scenarios that exercise real models, the rest using deterministic stubs? Cost and flakiness both matter.
2. **Keeping scenarios honest as the surface evolves.** The suite's value depends on scenarios staying realistic. How do we prevent scenarios from rotting into "exercises that pass" rather than "exercises that probe"?
3. **Golden-output vs structural assertions.** Pure structural assertions (path conformance, frontmatter shape) are stable. Golden-output assertions (the architect produces this exact spec for this brief) are brittle. Where's the right line?
4. **Cost of running in CI.** Real API calls aren't free. Are we OK paying per-PR? Do we batch runs nightly instead?
5. **First scenarios.** What's the smallest set that would have caught the path-conformance regression? The substrate-pivot brief is one obvious starting fixture.
6. **Coverage map.** Which assertions cover which behavioral surfaces? Without that map, we'll have false confidence.

## Adjacent concerns

- This is partly a positioning move: Etak claims to be a tool for serious product/engineering work. A behavioral eval suite is the kind of artifact that demonstrates the claim is honest. It's also defensible at YC-time.
- The suite scope creeps if we're not careful. Limit to agent + skill prompt files; do not try to test the underlying SDK or the harness itself.
