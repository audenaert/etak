# Designing experiments

**Time:** about thirty minutes. **Prerequisite:** [Getting started](getting-started.md) and at least one assumption you'd like to test.

An experiment is not "we built a thing and shipped it and watched the metric." That is a feature release. An experiment is five parts: a specific belief, a method that produces signal on that belief, success criteria written before the test runs, an interpretation guide for mixed results, and pre-committed actions for each outcome. Get any one wrong and you are doing theater.

This tutorial walks through designing one real experiment from beginning to end, with particular attention to the parts that go wrong most often.

## Questions to sit with before you start

1. **What was the last "experiment" your team ran? What did the results say, and what did you do?** Were the success criteria defined in advance? Did the outcome genuinely change what you built? If not, what was the experiment for?
2. **When you're wrong about a user behavior, how quickly do you find out?** What's the shortest feedback loop you have on a belief you're currently holding? The longest?
3. **How much of your current roadmap is resting on untested assumptions?** Pick the three biggest bets. What would it take to falsify each one? Why aren't you doing that?

## The assumption we'll work with

If you have a live assumption in your graph, use it. Otherwise use this one for the tutorial:

*On-call engineers will spend more than thirty seconds configuring alert rules if the configuration UI makes the trade-offs explicit.*

It is high importance (a whole feature rests on engineers engaging with configuration) and low evidence (we haven't watched anyone do it). A classic top-left-quadrant candidate.

## Step 1. Open the experiment

Type:

```
Design an experiment for the assumption about on-call engineers
configuring alert rules.
```

If the assumption isn't already in the graph, Etak will offer to capture it first. Accept, then continue.

Etak moves into **experiment**. It starts by asking two questions that matter: what category of assumption is this (desirability, usability, feasibility, viability, causal?) and what would signal look like if the belief is true?

Answer the second one in concrete terms. Not "engagement increases." *Behavioral*: "an engineer opens the config UI, spends more than thirty seconds, and saves at least one change."

## Step 2. Pick the method

Etak proposes a method based on the assumption type. For this one (a behavioral desirability question on a defined user group), it will probably recommend a **prototype test**: build a clickable mock, observe five to eight on-call engineers, measure engagement against the concrete signal above.

Consider the alternatives Etak offers. For the same assumption, a **fake door test** (ship a "configure rules" link in the product that leads to a waitlist) would give you desirability signal at scale, but less detail on *how* they engage. A **user interview** would give you qualitative texture but no behavioral evidence. An **A/B test** is overkill until you know the thing works at all.

The cheapest test that produces decision-grade signal is usually the right one. For this assumption, the prototype test probably wins.

## Step 3. Write success criteria before you run it

This is the step everyone knows to do and nobody does. Etak will push. Do not negotiate.

Bad: "engineers engage with the UI."
Better: "most engineers spend some time in the UI and make a change."
Right: "6 of 8 observed engineers spend more than thirty seconds in the UI and save at least one configuration change. Less than 3 of 8 = invalidated. 3 to 5 of 8 = inconclusive, run larger test."

Three things make the criteria load-bearing:

- **Specific numbers.** 6 of 8. Not "most." "Most" is the enemy of clear thinking.
- **Explicit invalidation threshold.** What number would convince you the belief is wrong? If there is no number that would convince you, you are not running an experiment. You are performing confidence.
- **An inconclusive band.** Real experiments produce ambiguous middles. Decide in advance what you'll do with them.

Etak will push on all three. Let it.

## Step 4. Pre-commit to actions

Etak now asks for three pre-committed action plans:

- **If validated.** "We advance the configuration feature to ready-for-build, scope it next sprint."
- **If invalidated.** "We shelve the idea that engineers will configure. We explore alternatives: smart defaults with no configuration, or ops-team-driven configuration upstream of on-call."
- **If inconclusive.** "We run a larger prototype test with 15 engineers across two customer segments before deciding."

The discipline here is uncomfortable. You are committing, on paper, to do something you haven't wanted to do yet. That discomfort is the feature. Without the pre-commitment, the results meeting becomes a negotiation. With it, the team has already decided.

One trick if pre-commitment is hard: ask "what would a good version of us, three months from now, be disappointed we didn't do?" That is usually the pre-committed action.

## Step 5. Write the interpretation guide

Good experiments ship with a short note on how to read the results. Edge cases, ambiguities, things to watch for. Etak drafts this and asks you to sharpen it.

For the configuration prototype test, the guide might note:

- Watch for engineers clicking through quickly without engaging — "polite exploration" counts as zero.
- If engineers ask for help during the session, note which step they got stuck on.
- If the behavior changes dramatically between on-call and non-on-call engineers, that is a segmentation finding worth capturing even if the headline result is clear.

## Step 6. Save and run

Review the experiment artifact Etak drafts. The YAML header has method, success criteria, effort estimate, action plans. The body has the protocol. Save it to `docs/discovery/experiments/`.

Now leave Etak and actually run the test. In real life this is where the week or two of work happens. Builds, recruits, sessions, notes. The experiment artifact is where you come back.

## Step 7. Record results honestly

After the test runs:

```
Record results for the alert-rules-config-prototype experiment.
```

Enter the actual numbers. If it was 5 of 8, say so. Don't round up. If 5 of 8 is in the inconclusive band, the outcome is inconclusive, and the pre-committed action is to run the larger test.

This is the moment the tool earns its keep. Etak will notice if the recorded result seems to contradict the pre-committed interpretation and flag it. The flag is not a judgment. It is a question: are you sure you want to interpret 5 of 8 as validation? If so, say why, and update the interpretation guide so the next person reading the graph understands what changed.

## Step 8. Propagate

```
Propagate the results.
```

Etak updates the tested assumption's status. Then it traces:

- **Parent idea.** Does this assumption's result change the status of the idea? A validated critical assumption advances it. An invalidated one may force a pivot.
- **Sibling assumptions.** Does the learning affect how you should rate the other assumptions under the same idea? Often yes — a behavioral observation tells you something about users beyond the specific belief you were testing.
- **Shared assumptions.** If this assumption was shared across ideas, did the test de-risk them all?

You now have a graph that is one experiment smarter than it was an hour ago.

## Step 9. Notice the learning beyond the result

Ask:

```
What did we learn besides the headline result?
```

Experiments almost always produce a secondary observation that is more valuable than the primary one. For the configuration prototype, maybe the headline was validated, but you also noticed engineers kept asking "why is it configured this way by default?" — which is a pointer toward a different, possibly better, idea about defaults rather than configuration.

Etak will offer to capture that observation as a new opportunity or idea, drafted and loosely connected. Accept or reject. Don't lose it.

## Go back to your original questions

- **The last "experiment" that didn't change anything.** What was missing? Almost always it was pre-commitment or specific success criteria. You now have both.
- **How quickly do you find out when you're wrong?** You just shortened one of those feedback loops from "we'll see when we ship in two months" to "we'll see in two weeks."
- **The untested roadmap bets.** Pick one. Design an experiment this week. Even a bad experiment beats no experiment on a high-importance, low-evidence belief.

## Common failure modes

- **Success criteria set after the test runs.** This is not an experiment. It is a press release with data.
- **Pre-committed actions too vague.** "We'll discuss" is not an action. "We'll advance to ready-for-build by end of sprint" is.
- **Running an experiment to confirm something you already believe.** If you cannot imagine being surprised, don't run it. Do something else.
- **Letting inconclusive results become "sort of validated."** The pre-committed action for inconclusive exists specifically to prevent this. Honor it.

## What to try next

- [**The full discovery loop**](full-discovery-loop.md) — see experiment design in its full context.
- [**Critiquing an idea**](critiquing-an-idea.md) — if the experiment invalidates the assumption, critique is where you go to pressure-test the idea that rested on it.
