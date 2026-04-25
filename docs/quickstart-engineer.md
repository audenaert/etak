# Quick start for engineers

**For:** a senior engineer, tech lead, or staff IC who just installed `etak-develop` and is deciding whether to trust it with real work.

**Question this page answers:** what does this actually do that a bare Claude Code session or a code-completion tool doesn't?

## The short answer

AI coding tools fail in predictable ways. Code generated without reading the repo's actual shape. Specs that drift from what the codebase needs. Stories that land in the queue without the context that would make them buildable. Autonomous agents that declare done when they're not. Architecture decisions that never get written down and have to be relitigated the next time they come up.

`etak-develop` is organized around those failure modes. It reads the codebase before it writes prose. It refuses to rubber-stamp stories with vague ACs. It pairs through implementation instead of throwing a patch over the wall. It records the decisions worth remembering. You stay in the driver's seat. The tool surfaces what it finds and shows its work before anything lands.

## What it gives you

A typed artifact graph under `docs/development/`. The ones you'll touch most:

- [**Story**](artifacts/story.md) — user-frame increment with testable ACs. The primary unit of work.
- [**Spec**](artifacts/spec.md) — grounded technical design for a story, epic, or project. Cites actual files.
- [**ADR**](artifacts/adr.md) — hard-to-reverse decision with alternatives and consequences. Immutable once accepted.
- [**Spike**](artifacts/spike.md) — time-boxed investigation when the design hinges on an unknown.

Everything is markdown with YAML frontmatter. Grep it, edit it, review it in a PR. The graph is the audit trail, and it's the input the skills read to answer "what's ready?" and "what's missing?"

## The five minutes

Open a Claude Code session in the repo where the work lives. Run these four moves in order.

### 1. Survey the work graph (30 seconds)

```
What's ready to build?
```

[Survey](skills/survey.md) reads `docs/development/` and leads with the one or two stories you could start now, not a dump of the backlog. It names why each is a good pick: small, unblocks other work, serves the next milestone. Pick one.

Survey writes nothing. It reflects the graph back so you can decide.

### 2. Check readiness before committing (90 seconds)

```
Is this story ready to build?
```

[Assess](skills/assess.md) runs a ready check against a Definition of Ready. It flags vague ACs ("users can sign in" is not testable), unresolved dependencies, missing spec anchors, and returns a traffic-light verdict with a specific list of what's missing.

If the story is not ready, assess offers the follow-up: rewrite the AC, draft the missing spec, kick a spike. Readiness debt is cheaper to pay now than mid-implementation.

### 3. Get a grounded spec (2 minutes)

```
How should we build this?
```

[Spec](skills/spec.md) greps the modules the story touches, reads the top of each relevant file, and checks how similar features are implemented today. Then it proposes a design that references real file paths, names at least two real alternatives, and flags non-functional requirements honestly.

When a decision inside the spec is hard to reverse, spec writes an [ADR](artifacts/adr.md) and links it from the spec. The audit trail holds without ceremony.

### 4. Pair through implementation (remaining time)

```
Let's build this.
```

[Build](skills/build.md) picks up the ready story and pairs through it. TDD where it earns its keep: failing test, implementation, refactor. Atomic commits at AC boundaries. If the implementation starts to drift into a second subsystem, build names it rather than letting the PR grow past what anyone can review.

At the end, build runs a self-review: lint, tests, types, AC coverage, clarity scan. The verdict is specific. If an AC isn't covered, it says so.

In practice, you probably don't get through all four in five minutes. The first two tell you enough about how the tool behaves: whether the readiness check finds the gaps you'd find, whether survey's recommendation matches the one you'd make.

## Why this makes your team faster

Three second-order effects. None of them are typing speed.

**Fewer built-then-rewritten features.** When a story arrives ready, with ACs that survive a readiness check and a spec that referenced real files, the PR that comes out of it is closer to the first try. The rewrites happen on paper, where they're cheap.

**Fewer surprises in PR review.** Reviewers get a spec that read the code, a PR description that maps changes to ACs, and a self-review that reports honestly on what's covered. The review spends its time on design judgment rather than on reconstructing what the change is supposed to do.

**An architecture trail that compounds.** Six months from now, when someone asks why the codebase uses passport.js instead of a custom OAuth layer, the answer is an ADR with the forces, the alternatives, and the consequences. The next migration reads the old ADRs before proposing the new one. Context does not vanish with the session.

## Where to go from here

- **[Develop overview](develop.md)** — the six skills, three agents, and how they compose.
- **[Develop foundations](context/develop-foundations.md)** — the longer argument: mixed-initiative design, grounding in material, traceability as a product feature. About fifteen minutes. Read it when you want to know why the tool behaves this way, not just how to use it.
- **[Skill reference](skills/)** — each skill's full spec. You don't need to learn skill names to use the tool; the reference is for when you want to know exactly what a skill does before reaching for it.
- **[Artifacts reference](artifacts/)** — the shape of each artifact, its schema, and its lifecycle.
- **[Tutorials](tutorials/)** — runnable walkthroughs against realistic scenarios.

## A warning

The tool is only as honest as the artifacts it reads. A story marked `ready` when the ACs are actually mushy produces a build pass that inherits the mush. An ADR never written is a decision that will be relitigated. A spec approved without reading the code is a spec that lies to the next engineer who touches it.

`etak-develop` reduces the cost of keeping the graph honest. It does not do it for you. If the team treats the artifacts as ceremony, the tool produces ceremony at scale. If the team treats them as the actual record of what was decided and why, the tool compounds every decision into leverage for the next one.

That's the trade. Keep the graph honest, get a faster build loop and an architecture trail you can trust.
