# Develop Foundations

**For:** readers who want to understand why `etak-develop` works the way it does, and how it differs from a generic AI coding assistant. **Length:** about fifteen minutes. **Required?** No. The [develop overview](../develop.md) stands on its own. But if you have used tools like Copilot, Cursor's agent mode, or a bare-prompt Claude Code session and wondered why the output feels closer to generation than to engineering, this is the longer answer.

Most AI coding tools optimize for generation speed. `etak-develop` optimizes for decision quality and traceability. The differences are not cosmetic. They rest on specific commitments about how software gets built well, and those commitments shape every skill, every agent, and every artifact the plugin produces.

**On this page.** The basic claim · grounded in material · mixed-initiative, not full-auto · traceability as a product feature · story and task are different things · the six-skill rhythm · strategy before code · honest self-review · why this matters now · where to go next.

## The basic claim

Software engineering is a decision-making discipline. Not a typing discipline. Not a code-generation discipline. The hard part of shipping good software is not producing syntactically correct lines; it is deciding what to build, where to put it, which tradeoffs to accept, and which invariants to protect. The code is the residue of those decisions.

Most AI coding tools invert this. They treat generation as the product and the decisions as a prompt problem. Ask for a function, receive a function. Ask for a feature, receive a feature. The tool's job ends when the code compiles.

`etak-develop` is built on the opposite premise. The tool's job is to help you make better decisions, to record them so they can be traced, and to turn them into code that reflects what you actually chose. Generation is the cheap part. The thinking is the work.

## Grounded in material

The single most important rule in the `etak-develop` core guidelines is to ground everything in the codebase. Before writing a spec, read the code. Before estimating, understand what exists. Before proposing an architecture, check what is already there. A spec that says "use the auth module" without knowing whether that module is `src/auth.ts` or `services/auth/` is useless, and worse than useless, because it gives false confidence.

Prompt-first tools do not work this way. They treat the prompt as the world. What the engineer types becomes the ground truth, and the code the tool generates floats free of the repository it is supposedly serving. When the generated code collides with the actual codebase (different module layout, different patterns, different assumptions about error handling) the engineer has to reconcile the two by hand.

`etak-develop` reverses the flow. The tool reads the codebase first. Specs cite actual files and actual functions. ADRs describe decisions the codebase will live with, not decisions about a hypothetical codebase the engineer might write later. When a skill cannot find the code it needs, it says so directly: "I can write this spec at the conceptual level, but you will need to fill in the current state, or point me at the files you want me to read." The tool refuses to invent what it cannot verify.

This connects to a theme Donald Schön developed in *The Reflective Practitioner*: expertise works through what he called **back-talk of the materials**. The practitioner makes a move, the situation responds, the practitioner adjusts. You cannot reflect against nothing. A codebase is material in exactly Schön's sense. It has grain, it has existing patterns, it has decisions already made. Design that ignores the grain will fight it. Design that reads the grain first is cheaper to implement, cheaper to maintain, and more likely to survive contact with the next engineer to touch the file.

## Mixed-initiative, not full-auto

Eric Horvitz's 1999 paper on mixed-initiative user interfaces is cited in Etak's discovery foundations. It applies equally to development. The core idea: in a good collaboration between a human and an AI, both parties can take initiative. The system proposes. The human decides. Neither side is passive, and neither side unilaterally commits the other.

Most AI coding tools sit at one of two extremes. Prompt-in/code-out tools put the human entirely in charge: the tool waits for instruction and produces output. Full-auto agent modes go the other way: the tool picks a task, writes the code, runs the tests, and opens a pull request with minimal human involvement. The first is often too dumb; the second is often too confident.

`etak-develop` is deliberately mixed. The interactive skills (plan, spec, assess, survey, test, build) are the default. They pair with the engineer. They propose structure, surface gaps, draft artifacts, but they show their work before committing anything to the graph. The engineer accepts, rejects, or revises. Pushback is part of the collaboration: if a story has no testable acceptance criteria, the tool says so. If a spec proposes a design ungrounded in the actual codebase, the tool names that. If an estimate is being made without looking at the code, the tool flags it.

The three agents (architect, tech-lead, developer) are autonomous where autonomy is appropriate. An architect agent can read a codebase deeply, consider alternatives, and draft a multi-part technical design while the engineer works on something else. A developer agent can pick up a ready story, work through it with TDD, and open a pull request. But autonomy is earned by the state of the work, not granted by default. A story without acceptance criteria is not ready for a developer agent; a project without a spec is not ready for a tech-lead agent. The handoff is deliberate.

Ben Shneiderman's framing in *Human-Centered AI* captures the design goal: not a choice between high automation and high human control, but both at once. A tool that automates everything takes the thinking away. A tool that automates nothing leaves you doing work a machine could do better. The interesting design problem is to give you both, and it has to be solved case by case.

## Traceability as a product feature

Every development artifact in `etak-develop` can carry a `from_discovery` field. The field is a link back to the validated discovery idea that motivated the work. It is one-way: development points back at discovery, discovery never points forward at execution. The asymmetry is intentional. Discovery does not need to know which projects picked up which ideas; development needs to know why each project exists.

The hierarchy is explicit. Projects contain epics. Epics contain stories. Stories can contain tasks. The chain is not bureaucracy. It is the answer to a question that every real engineering organization has to answer at some point: "Why was this built?"

Compliance auditors ask this question. New team members ask it on their first week. The engineer who has to modify the code two years later asks it, usually while reading a git log that stops at a commit message that says "fix auth." Most AI coding tools do not leave an answerable trail. The code appears, the prompt that produced it disappears into a chat log, and the reasoning vanishes with the session.

`etak-develop` treats the trail as a product feature. The artifact graph is durable. The links are typed. When someone asks "why was this change made?", the answer is not hidden in a Slack thread or lost in a prompt history. It is a markdown file with a `from_discovery` link pointing at the opportunity, the idea, and the experiment that validated it. That is cheap when you create it and expensive to reconstruct after the fact, which is why it has to be a default rather than an afterthought.

## Story and task are different things

One of the deliberate departures from standard project management tooling: `etak-develop` distinguishes a story from a task and treats that distinction as load-bearing.

A **story** is user-frame. "As a *persona*, I want *capability*, so that *outcome*." It describes change from the perspective of the person who will experience it. Acceptance criteria are the observable conditions under which the story is done.

A **task** is engineering-frame. It describes a technical change that enables or supports a story or epic. Migrations, feature flags, telemetry, internal refactors. Tasks are optional. When a story is a single obvious pull request, tasks add noise. When a story depends on non-user-visible work, or when a technical change cuts across multiple stories, tasks earn their place.

This is a conscious rejection of the Jira default that collapses user and engineering concerns into the same ticket. Jira tickets are frame-agnostic: a ticket can be a user request, a bug, an internal refactor, a deployment task, or all four at once. The frame-agnosticism is presented as flexibility. In practice, it means nobody can tell whether a given ticket is supposed to produce a user-visible change, and the reviews that might have caught a mismatched frame never happen.

Keeping user-frame and engineering-frame separate costs something. It requires the team to know, for each artifact, which frame they are in. It pays for itself in the conversations it prevents: the ones where a product manager thought they were specifying a user outcome and an engineer thought they were receiving a work order, and the thing that shipped was neither.

## The six-skill rhythm

Development in `etak-develop` is organized around six interactive skills: plan, spec, assess, survey, test, build. They form a loop, not a waterfall.

**Plan** scopes validated ideas, decomposes them into workstreams and epics, and sequences milestones. **Spec** produces grounded technical design against the actual codebase. **Assess** stress-tests readiness, feasibility, and sizing before committing to build. **Build** runs the inner loop: TDD pairing, implementation, self-review, pull request. **Test** handles strategy, layer selection, and flake debugging. **Survey** is the home base: navigate the work graph, see what is ready, blocked, or stalled.

The rhythm is: plan, spec, assess, build (with test), back to plan for the next slice. You scope and sequence. You ground the design. You stress-test before committing. You implement with care. When one slice lands, you look at the graph again and pick the next move.

The rhythm holds at every scale. A single story moves through the same loop as a multi-quarter initiative, just with fewer artifacts. The skill you reach for is determined by the state of the work, not by a predetermined project phase. Generic code assistants do not model scope this way. They have no opinion about whether you are planning or building or assessing; they respond to whatever prompt you type. The result is often a mismatch between what the engineer needs and what the tool provides: a request for a quick implementation during planning, a request for architectural critique during the final push to ship.

Naming the state is how `etak-develop` picks the right move. The core guidelines put it plainly: "You have a validated idea that needs structure, plan. You have a story and need to decide how to build it, spec. You have a story or spec and want to know if it holds up, assess." The engineer describes the move they want to make, in plain language, and the tool routes. No skill names to memorize.

## Strategy before code

The `test` skill is described as "rigorous." The description is doing work. Rigor in testing is not about quantity; it is about knowing what you are testing and why. `etak-develop` pushes test plans before test code, and test code before production code.

The rhythm is standard test-driven development in its original form: write the failing test, make it pass, refactor, repeat. Kent Beck's formulation of TDD in the late 1990s anticipated most of what modern engineering organizations now take as given about unit testing, and the inner loop in `etak-develop`'s `build` skill reflects that heritage directly. The interesting layer on top is the strategy step. Before the TDD inner loop starts, the `test` skill produces a plan: which acceptance criteria map to which test layers, where integration tests earn their keep, which flakes are tolerable and which are not, which fixtures are shared and which are scoped.

The strategy step is what most autonomous coding agents skip. They can write tests, often many of them, often at the wrong layer. A unit test for a function that is only reachable through integration code is testing the wrong thing. A suite of end-to-end tests for logic that could be covered cheaply at the unit level burns CI time and obscures what is actually broken when something fails. Tests without strategy are noise with intent.

`etak-develop` treats the strategy as the thinking work and the code as the output. When an engineer starts with "write some tests for this module," the tool asks what the module is for, how it is used, which invariants matter, and which failures would be most expensive. The tests that follow are the ones that answer those questions. The ones that would not answer them do not get written.

## Honest self-review

The `developer` agent ends its work with a self-review. The self-review reports whether each acceptance criterion was satisfied. When a criterion was not satisfied, the agent says so.

This sounds obvious. In practice, most autonomous coding agents optimize for the "done" signal. They were rewarded during training for completing tasks. The incentives run toward declaring victory, and the declarations are often disconnected from whether the code actually does what was asked. An agent that says "implemented and tested" when the implementation only passes two of three acceptance criteria is worse than an agent that says nothing, because the claim of completeness takes a reviewer out of the loop.

`etak-develop`'s developer agent is tuned in the opposite direction. The acceptance criteria are specific. The self-review is explicit. When a criterion is not met, the agent names which one, explains why, and surfaces what would be required to meet it. The output is accurate state, not reassuring state. The engineer reviewing the pull request can act on what they read rather than having to re-derive the real state of the work from first principles.

This is the same posture that runs through the interactive skills. The plan skill flags milestones that are not demo-able. The spec skill pushes back on designs ungrounded in the codebase. The assess skill refuses to rubber-stamp stories with vague acceptance criteria. The tool reports what it sees rather than what the engineer hoped it would see. That is a small thing when everything is going well and a large thing when something is about to go wrong.

## Why this matters now

Code generation got cheap quickly. Between 2022 and 2026, the marginal cost of producing a plausible-looking function collapsed. The scarce resource in software engineering is no longer typing; it is judgment. Which function to write. Where to put it. How it should fail. Which tests tell you whether it works. Which decisions, once made, become expensive to reverse.

The tools that dominated the first wave of AI-assisted development were built for the old scarcity. They made typing faster. They assumed the judgment was happening somewhere else, probably in the engineer's head, probably before the prompt. Now that the typing is the easy part, the tools that only made typing faster have less left to offer.

`etak-develop` is a bet on the new scarcity. The plugin spends compute on reading the codebase before writing. It spends interaction time on assessing readiness before committing. It spends artifact surface on traceability, so decisions can be reviewed later. It accepts that the engineer will reject its proposals sometimes and treats the pushback as part of the collaboration, not a failure of prediction. The goal is not to maximize the lines of code produced per unit of engineer time. The goal is to maximize the decisions made well per unit of engineer attention.

That is a different shape of product. It is slower in some places where speed was never the point, and faster in some places where the old tools did not compete.

## What this means in practice

The ideas above show up in specific product decisions:

The `spec` skill reads the codebase before proposing a design, which is the grounded-in-material principle made concrete.

Every development artifact carries a `from_discovery` field when the work originated from validated discovery, which is traceability made into a default.

Stories have acceptance criteria and the `assess` skill refuses to pretend otherwise, which is the story-versus-task distinction enforced at the point it matters most.

The `developer` agent's self-review reports acceptance criteria honestly, which is the mixed-initiative principle extended to reporting. The agent takes initiative in implementation and defers to the engineer on what counts as done.

The six skills form a loop that holds at every scale, which is the rhythm principle generalized across the hierarchy from story to project.

## Where to go next

If the ideas above are new to you and you want to go deeper, a few sources are worth the time. Some overlap with the discovery foundations; the arguments apply on both sides of the plugin boundary.

Donald Schön, [*The Reflective Practitioner*](https://www.amazon.com/Reflective-Practitioner-Professionals-Think-Action/dp/0465068782) (1983). The back-talk-of-materials argument, made about architecture and design professions but applicable to any discipline that involves shaping material toward a purpose. Dense but rewarding.

Eric Horvitz, ["Principles of Mixed-Initiative User Interfaces"](https://erichorvitz.com/chi99horvitz.pdf) (1999). The clearest statement of how a tool and a human can share control without either one becoming passive. Short and still current.

Ben Shneiderman, [*Human-Centered AI*](https://www.amazon.com/Human-Centered-AI-Ben-Shneiderman/dp/0192845292) (2022). The modern synthesis. High automation and high human control together, which is what a development tool built on this philosophy is trying to achieve.

Kent Beck, [*Test-Driven Development: By Example*](https://www.amazon.com/Test-Driven-Development-Kent-Beck/dp/0321146530) (2002). The original book-length argument for the inner loop that `build` and `test` are built around. The specific examples have aged; the rhythm has not.

None of these will teach you how to use `etak-develop`. They will teach you why it is built the way it is.

## Related

- [**Develop overview**](../develop.md) — what the plugin does, skill by skill.
- [**Foundations**](foundations.md) — the companion document for the discovery plugin. The research lineage is shared.
- [**About the name**](about-the-name.md) — the navigation metaphor that orients the vocabulary of the whole product.
