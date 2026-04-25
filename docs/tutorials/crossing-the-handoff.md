# Crossing from discovery to development

**Time:** about thirty minutes. **Prerequisite:** [the full discovery loop](full-discovery-loop.md), or a real validated idea of your own sitting in `docs/discovery/`.

This tutorial picks up where discovery finishes. You have a validated idea. The team nods. Engineering asks "so what are we building?" and the idea starts to blur before anyone opens an editor. This is the handoff, and it is where discovery discipline gets tested.

You will take one validated idea and turn it into a scoped project with a first ready story, a grounded spec, an ADR, and a `from_discovery` link that closes the loop. All of it under `docs/development/`. None of it fabricated.

## Questions to sit with before you start

Spend a few minutes on each. Your answers are what the tool is going to put pressure on.

1. **How do validated learnings currently flow from discovery into engineering on your team?** What makes it across and what gets lost? Does the thing you decided to build still look like the thing you decided to build a week later?
2. **What does "ready to build" actually mean to the engineers on your team?** Is it a checklist, a feeling, a ritual? Who decides? What happens when someone pulls a story that wasn't actually ready?
3. **When a shipped feature underperforms, can you trace back to the assumption you made?** Or does the trail go cold at the PR? If compliance asked why a specific change landed, what would you show them?

Write your answers. You will come back to them.

## Scenario

You ran the full discovery loop on the B2B observability scenario. You have:

- An **objective**: reduce alert fatigue on high-volume services.
- An **opportunity**: *how might we* help on-call engineers distinguish signal from noise at 3am.
- A **validated idea**: adaptive alert thresholds that learn from user response patterns.
- Seven assumptions, one tested through interviews, five ranked by risk.
- An experiment result that sharpened the direction: engineers will not interact with a rating UI, but their silence and acknowledgement behavior is trainable signal.

The idea is now `status: validated`. It is not shipped. Nothing has been built. The next move is to cross it into development without losing what discovery taught you.

Open Claude Code in the same repo you ran discovery in.

## Part 1. Scope it

Say:

```
I want to take the adaptive-thresholds idea into development.
Help me scope it.
```

Etak recognizes this as a plan move and enters the scope lens. It reads `docs/discovery/ideas/idea-adaptive-alert-thresholds.md` and the assumptions under it. It asks the questions that shape scope: what's in, what's explicitly out, what artifact type this actually is.

Plan drafts the project shell. Name, demo sentence, goals, constraints, non-goals. Pay attention to non-goals. "No user-facing rating UI" is worth more than four goals. It encodes the learning from the experiment that would otherwise evaporate.

**What just happened.** You forced a boundary on something that was still fuzzy. Discovery settled *what* to build; plan just settled *how big*. The non-goals list carries the experiment's finding forward into execution where it can actually constrain decisions.

## Part 2. Decompose into epics and stories

Stay in plan. Say:

```
Decompose it. What are the natural seams and what's the thinnest first slice?
```

Plan runs the decompose move. It looks for real system boundaries, not team boundaries. For adaptive thresholds, the natural seams are usually three: ingestion of response signal (acknowledge, silence, escalate), the learning model itself, and the surface where suggested thresholds appear to an engineer editing an alert rule.

Plan proposes three workstreams, one per seam, each with an interface contract. It proposes two or three epics under the project: one for signal ingestion, one for the suggestion surface, one that spans the model if the model is non-trivial.

Then plan asks what M1 proves. The honest answer is not "all three working." M1 is the thinnest path that shows the architecture holds: a single service, one acknowledgement signal captured, one suggested threshold surfaced in the UI read-only, no learning yet. If your first M1 proposal includes the learning model, plan pushes back. The learning is the risky middle; prove the pipes first.

Plan drafts a first epic ("capture response signal for acknowledged alerts") and a first story under it ("engineer's alert acknowledgement records a typed event against the rule"). Save the drafts.

**What just happened.** You went from one validated idea to a project with three workstreams, two or three epics, one first story. The story is in `draft`, not `ready`. Draft is honest; you haven't grounded the design yet.

## Part 3. Link forward and backward

Open the project file plan just wrote. Look at the frontmatter. The `from_discovery` field is populated with `idea-adaptive-alert-thresholds`. This is not decoration.

Run:

```
Show me the trace from this story back to the objective.
```

Etak follows the chain: story → epic → project → idea (via `from_discovery`) → opportunity → objective. Six links. Every edge is a file on disk.

This is load-bearing. Three reasons.

For **compliance**, the chain is the answer to "why did this change ship?" The auditor does not want your Slack history. They want a traceable path from production change to validated need. `from_discovery` is that path.

For **team onboarding**, a new engineer reading the story sees the objective at the end of the chain and knows what the work is actually for. Without the link, they see ACs and guess.

For **the team itself**, the link is the record that discovery happened at all. Teams that skip it end up relearning the same invalidated assumptions six months later.

Notice what the link does not do. Discovery never reaches forward. The idea in `docs/discovery/` does not know about the project. Development owns traceability. This is deliberate: an engineering team can rescope, split, or abandon a project without needing write access to the discovery graph.

**What just happened.** You wired the handoff. The one-way link is small enough to skip and important enough to skip being the tell for a team that is not doing discovery seriously.

## Part 4. Ground the first spec

The story is in draft. The implementation path is not yet obvious, because even a simple event capture raises real questions: what table, what schema, what happens when the alert rule changes after the event is recorded, how does this interact with the existing alerting service. Say:

```
Write a spec for the acknowledgement-event story.
```

Etak enters the spec skill. Watch what happens next. Before drafting a single sentence of design, spec reads the codebase. It greps for the alerting modules. It reads the top of the relevant files. It notes the existing event-recording patterns. It checks the testing conventions neighbors use. You will see it cite specific paths.

When the draft appears, go straight to the Current State section. Read the citations. This section is where the spec earns its weight. Every path listed is a real file in your repo. Every pattern named is one you could open and verify. A spec without this section is an opinion piece dressed up as design.

Then read the Alternatives Considered section. Spec will name at least two alternatives it rejected. For acknowledgement events: a new `alert_events` table versus extending the existing `audit_log`, or dispatching events through the existing notification bus versus a direct write. If the alternatives are strawmen, push back. Real alternatives are the ones an engineer on the team would actually argue for.

Save the spec as draft. The `for` field points at the story. The story's body gets a pointer back to the spec.

**What just happened.** The design is now grounded in material. This is the antidote to architecture astronauts: a design document that cannot be written without reading code will not propose things the code cannot cheaply support. The Current State section is the single most useful thing in the spec, and it exists because spec refused to draft without grounding first.

## Part 5. Record the hard-to-reverse decision

While writing the spec, one question came up that the spec itself cannot settle: should the per-user response pattern live in a streaming aggregate (maintained online, read-on-write) or a batch aggregate (computed periodically, read from a materialized view)? Both are viable. Both shape how the rest of the system gets built. Changing later costs weeks.

This is an ADR. Say:

```
Record this as an ADR.
```

Spec applies the hard-to-reverse test and agrees. It drafts an ADR capturing the forces (latency of the suggestion surface, operational cost of streaming infrastructure, the observability team's existing batch patterns, the size of the response-signal firehose), the decision (batch with five-minute refresh, specifically because it lets you reuse the existing aggregation pipeline), and the consequences (suggestions trail real behavior by up to five minutes; the suggestion UI must show a freshness timestamp so engineers know what they're looking at).

The ADR lands in `docs/development/adrs/`. It is linked from the spec's `adrs` field. It is effectively immutable. If the team later wants streaming, they write a new ADR that supersedes this one.

**What just happened.** You captured a real decision at the moment it was made, with the alternatives on the table at the time, in a place a future engineer will actually find. ADRs amended in place lose their audit value. Write new, supersede old.

## Part 6. Assess readiness before commit

The first story is in draft. The spec is in draft. Before you pull the story into an iteration and hand it to the developer agent, check readiness. Say:

```
Is this story ready to build?
```

Etak enters the assess skill and runs the ready lens against the first story. It checks the ACs for testability. It checks that the persona-goal-outcome frame actually names a user, not "the system." It verifies that the parent epic exists and that the spec covers the design question the story leaves open. It checks the workstream and milestone assignments.

Assess will probably return "almost ready" with specific gaps. Common ones for this scenario:

- One AC restates the title. Not testable. Needs rewriting as a given-when-then.
- The edge case "engineer acknowledges an alert that has since been deleted" is missing. The spec has a view on it, but the story doesn't.
- The question "does acknowledgement from a mobile push notification count the same as acknowledgement from the web UI?" is unresolved. This is a spike, not an AC rewrite.

Let assess offer follow-ups. For the vague AC, it suggests a rewrite; approve it and the story is updated. For the missing edge case, it adds an AC. For the spike, it drafts a spike artifact with a one-day time box and a specific question: "is mobile-push acknowledgement distinguishable from web-UI acknowledgement in the existing event stream?" That spike blocks the story from moving to `ready`.

Do not skip the spike to unblock the story. A story promoted to `ready` over an unresolved design question is how teams ship features that re-open as bugs. Assess externalizes "ready to build" so it is a check, not a feeling.

**What just happened.** You turned a vague sense of readiness into a verdict with specific follow-ups. Two of them were cheap fixes you accepted. One was a spike. The story is honest about its state.

## Part 7. Look at what you built

```
Show me the development graph for adaptive thresholds.
```

You should see a connected tree, all in `docs/development/`:

```
project-adaptive-alert-thresholds  (scoping)
  from_discovery: idea-adaptive-alert-thresholds
  workstreams:
    - workstream-signal-ingestion
    - workstream-suggestion-surface
    - workstream-learning-model
  milestones:
    - milestone-m1-read-only-suggestion
  children:
    epic-acknowledgement-signal-capture  (draft)
      stories:
        story-acknowledgement-records-typed-event  (draft, blocked on spike)
          spec: spec-acknowledgement-event-capture  (draft)
            adrs:
              - adr-001-batch-aggregate-for-response-patterns
      spike-mobile-push-acknowledgement-distinguishable  (open)
    epic-suggestion-surface  (draft)
    epic-learning-model  (draft)
```

Open the project file. The `from_discovery` link is there. Follow it. You land in the idea. Follow the idea's `addresses`. You land in the opportunity. Follow the opportunity. You land in the objective. The chain holds.

Open the story. Its `from_discovery` field is also populated. This is not redundant with the project's link. Stories can originate directly from validated ideas without passing through a project, and development-side queries on "what did this idea produce" need to find every artifact that traces back.

Open the spec. Read the Current State section again. Every path cited is real. The design is grounded.

## Go back to your original questions

Answer them again, in the language of the artifacts you just built.

**How do validated learnings flow from discovery into engineering on your team?** They flow through one link, `from_discovery`, set at plan time on the artifact that picks up the work. The experiment finding about the rating UI became a non-goal on the project. The validated assumption became a constraint the scoping respected. The invalidated belief became a warning in the project body. Learning did not evaporate; it got written into the artifact that the engineering team reads.

**What does "ready to build" mean on your team?** It now means the output of the assess skill's ready lens: testable ACs, a parent epic that exists, a spec where the design isn't obvious, no unresolved blocking questions. A story that does not pass the check stays in `draft`. The check is a file, not a feeling, and anyone can run it.

**When a shipped feature underperforms, can you trace back to the assumption you made?** Yes. From PR to story, story to spec, spec to ADR, story to `from_discovery` idea, idea to opportunity, opportunity to objective. Six links. The trail does not go cold at the PR. That is the compliance chain and the team-learning chain, and they are the same chain.

## What to try next

- The **develop getting-started tutorial** takes the `ready` story you just produced into implementation: build with TDD, self-review, PR.
- Run this tutorial on a real validated idea of your own. The first pass feels like overhead. By the third idea you cross, the shape is habitual and the handoff stops being the place work quietly decays.
- The **pm-engineer-partnership pattern** in `docs/patterns/` covers the conversation side of the handoff: what to say in the meeting where you walk the engineering lead through the project and the first story. The artifacts do half the work; the conversation does the other half.
