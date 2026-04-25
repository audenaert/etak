# The full discovery loop

**Time:** about an hour if you move, longer if you think. **Prerequisite:** [Getting started](getting-started.md).

This tutorial runs a complete discovery cycle on a realistic scenario. It covers all six user-facing skills, the product-researcher agent, and the part most teams skip: propagating experiment results back into the graph so the next decision has better evidence than the last one.

## Questions to sit with before you start

These are the questions the tutorial is going to put pressure on. Spend a few minutes with each before you start typing.

1. **How do you align ideas you have strong conviction about to business outcomes?** When a senior leader or your own gut says "we should build X," how do you tell whether it's actually serving the outcome you're responsible for? What do you do when it isn't?
2. **How do you refine stakeholder requests into purposeful features without becoming an order-taker?** When sales comes to you with a specific feature request tied to a specific deal, what is the actual conversation you want to have? What do you wish you could show them?
3. **How do you make sure your AI-accelerated team is shipping the right things, not just more things?** A team that builds twice as fast but builds the wrong thing is a team shipping twice as much waste. What does "right thing" mean in your context, and how do you verify you are building it before you build it?

Write something down. You will look at this again at the end.

## Scenario

You are the PM for a B2B observability platform. An enterprise customer has asked for "AI alerting." A founder has opinions. Sales is excited. Engineering is skeptical. You have one week before the roadmap review.

This is the situation the tutorial handles. Adapt the specifics to your own context.

## Part 1. Orient before you generate

Before you brainstorm anything, figure out where you are. Open Claude Code and say:

```
Catch me up on discovery.
```

On a fresh graph, Etak will tell you there's nothing yet and offer to start with an objective. On an existing graph, it will give you a prioritized picture of the state: stale experiments, high-risk assumptions that haven't been tested, ideas with no assumptions under them, opportunities that have gone quiet. This is the **orient** skill doing the work of triage.

Now introduce the situation:

```
Sales is pushing an "AI alerting" request from a specific enterprise
customer. I have a week. Help me figure out what we're actually
being asked to solve.
```

Etak will press on the framing. *Which customer? What did they say in their own words? What problem are they trying to solve by asking for AI alerting?* This is the part a good PM does anyway, but in a meeting with a noisy Slack thread open. Having the tool ask the questions makes it easier to answer them honestly.

By the end of this step you should have one or two candidate opportunities framed as *how might we* questions, not a feature request. Something like:

- *How might we* reduce alert fatigue on high-volume services?
- *How might we* help on-call engineers distinguish signal from noise at 3am?

Notice these are not "AI alerting." They are the job "AI alerting" was being asked to do.

## Part 2. Bring in external context

You have internal signals. Before you generate ideas, bring in what the outside world already knows. Say:

```
Research the observability alerting competitive landscape.
```

This invokes the [**product-researcher agent**](../agents/product-researcher.md). It runs autonomously for a few minutes and comes back with a structured brief: direct competitors, adjacent players, what they actually do under the "AI alerting" label, what users say works, what has failed, what enabling technology has shifted recently.

Read the brief. Notice what you didn't know. The most common useful finding at this step is that "AI alerting" is a term that means four different things and the customer probably meant one of them.

Come back into an interactive session:

```
Based on that research, let's brainstorm opportunities and ideas.
```

Etak moves into **explore**. It proposes additional opportunities informed by the competitive work, and for each it can produce a spread of candidate ideas. You are not picking one yet. You are widening the space so the one you do pick is picked from a real set of alternatives.

Save two or three opportunities and four to six ideas. Don't overthink the drafts. Draft status exists on purpose.

## Part 3. Sound the ideas

Pick the idea that feels most promising. For the scenario, say it's *adaptive alert thresholds that learn from user response patterns*. Say:

```
What are we assuming about this idea?
```

Etak is in **sound** now. Let it work. Expect between six and ten assumptions across categories: desirability, usability, feasibility, viability, causal.

This is where the tutorial earns its keep. Watch for two specific things.

**Hidden beliefs.** The tool will surface something you believed but had never stated. For the scenario, it might be "users are willing to train the system by explicitly rating alerts." You assumed it. You would have built for it. It is also probably wrong. Mark its importance high and its evidence low.

**Shared assumptions.** If you have multiple ideas, Etak may note that a single assumption underlies several of them. These are high-leverage. One experiment can inform three ideas.

Save the assumptions. You should now have an idea with seven or eight testable beliefs attached, each rated on importance and evidence.

See the [surfacing assumptions tutorial](surfacing-assumptions.md) if any of them feel stuck.

## Part 4. Critique from outside

You have looked inward at what the idea rests on. Now look at it from outside. Say:

```
Critique this idea.
```

Etak moves into **critique**. It will run several rounds from different perspectives: a skeptical user persona, a cost-conscious finance lens, an adversarial competitor, a second-order-effects framework. Each round produces specific findings with severity ratings.

Two things to watch.

**Findings should be specific.** "This might not work" is not a critique. "On-call engineers at 3am do not want to interact with a rating UI; they want to silence the alert and sleep" is. Push the tool for specifics if it drifts vague.

**Strengths matter too.** Ideas that survive serious critique get stronger, not weaker. A critique session that finds three solid reasons the idea holds up under pressure is as valuable as one that finds three risks. Both inform the next decision.

When new rounds produce diminishing returns, stop. Propagate each finding to a resolution. Risk findings that articulate testable beliefs get promoted to assumption artifacts — the originating critique becomes the assumption's provenance, and the finding is marked `addressed` with the new assumption slug recorded as its resolution. Other findings fold into the idea body, point toward alternative ideas worth capturing in [explore](../skills/explore.md), or get dismissed with a reason. The audit trail is the point: every finding ends with an outcome, and the graph remembers what came from where.

See the [critiquing an idea tutorial](critiquing-an-idea.md) for more on running useful critique sessions.

## Part 5. Prioritize what to test

Now converge. Say:

```
Prioritize these assumptions. I have about three days of research
bandwidth and one engineer I can borrow for a half-day.
```

The constraint is the key. **Prioritize** is doing two things: ranking by risk (high importance + low evidence) and filtering by feasibility under your actual constraint. A perfect assumption to test that requires six weeks of setup is not the right assumption to test first.

The output should be a ranked list with a clear recommendation: "Test assumption X this week with method Y. It is the single highest-leverage move given your constraints."

If the recommendation surprises you, pay attention. If the ranking confirms what you already thought, also pay attention — the tool just gave you a crisper way to explain the call in the roadmap review.

## Part 6. Design and run the experiment

```
Design an experiment for the top assumption.
```

Etak moves into **experiment**. It will:

1. Pick a method appropriate to the assumption type. A desirability assumption might call for user interviews; a usability one for a prototype test; a causal one for a time-bounded A/B.
2. Draft specific success criteria. Not "users find it useful." Concrete: "at least 6 of 8 interviewed engineers describe a situation in the last month where adaptive thresholds would have reduced their pain, unprompted."
3. Force you to pre-commit to action plans for each outcome: validated, invalidated, inconclusive.

This last step is where most teams fail in real life. Without pre-commitment, the results meeting becomes a negotiation. With it, the team has already agreed what the results mean.

Save the experiment. Its status is `planned`.

Now go run it. In a real cycle, this is a day to a week of actual work — interviews, a prototype, a data pull. When you're back:

```
Record results for the adaptive-thresholds experiment.
```

Enter what happened. Etak will help you interpret against the pre-committed criteria. It will resist post-hoc rationalization. If results were 4 of 8 instead of 6 of 8, and 6 of 8 was the bar, the answer is not "close enough." The answer is inconclusive, and the pre-committed action for inconclusive is what you do next.

## Part 7. Propagate the learning

This is the step most teams skip, and it is the step that makes the graph compound. Say:

```
Propagate the results.
```

Etak updates the tested assumption's status (validated, invalidated, inconclusive). Then it traces the implications:

- **For the parent idea.** A validated critical assumption advances the idea toward ready-for-build. An invalidated one may mean the idea needs to pivot or be shelved.
- **For sibling ideas that shared the assumption.** One experiment may have just de-risked or invalidated three ideas at once.
- **For the opportunity.** If the evidence shifts, the opportunity may need reframing.

You should end this step with a clear answer to: *what is the highest-value next move, given what we just learned?* Etak's recommendation is based on reading the updated graph. Your job is to decide whether to take it or push back.

## Part 8. Come back to your original questions

The three questions from the top of this tutorial.

**How do you align ideas you have strong conviction about to business outcomes?** You now have a structure where every idea traces to an opportunity that traces to an objective. The chain either holds or it doesn't. When it doesn't, you have a specific thing to talk about: the missing link between this idea and the outcome you promised your CEO.

**How do you refine stakeholder requests without becoming an order-taker?** The request came in as "AI alerting." It is now sitting in the graph as an idea with seven assumptions, one of which you tested, and a ranked list of the riskier ones you haven't. That is a different conversation with the sales lead. "Here's what we would need to believe for this to be worth building" is a better answer than "yes" or "no."

**How do you make sure your AI-accelerated team is shipping the right things?** The graph is the instrument. If the team is shipping fast but the assumptions never get tested, the graph shows it. If assumptions get tested but the learning never flows back, the graph shows that too. The constraint on quality is no longer "are we moving fast enough?" It is "are we learning as fast as we're building?" That is the question worth answering.

## What to do with this

Do the loop again on a real initiative this week. The first pass feels like overhead. The third pass feels like you're finally seeing what was always there.

When you have a validated idea you want to actually build, run [**crossing the handoff**](crossing-the-handoff.md) next. It picks up from the adaptive-thresholds idea you just produced and walks it into `docs/development/` as a scoped project with a ready story, a grounded spec, and an ADR. The handoff is where discovery discipline either holds or quietly evaporates.
