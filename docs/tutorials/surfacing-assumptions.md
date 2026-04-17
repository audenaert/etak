# Surfacing assumptions

**Time:** twenty to thirty minutes. **Prerequisite:** [Getting started](getting-started.md).

This tutorial goes deep on one move: finding what an idea is actually betting on, especially the beliefs that feel too obvious to name. That "feel too obvious to name" part is where most expensive mistakes live.

## Questions to sit with before you start

1. **Think about the last idea you championed that didn't work.** Looking back, which belief was wrong? When did you form that belief, and on what evidence? If you'd been forced to write it down before you started building, would you have noticed it was thin?
2. **In your current work, what is the deepest thing you believe about your users that you have no direct evidence for?** Not a feature hypothesis. Something about who they are or what they want that shapes every idea you generate. How would you test it? Why haven't you?
3. **When you disagree with a colleague about a product decision, how often is the disagreement really about a hidden assumption that neither of you has surfaced?** What would change about the argument if the assumption were written on a wall between you?

## The idea we'll work with

Pick a live idea in your own work, or use this one for the tutorial:

*An in-app onboarding checklist that guides new users through five key actions in their first session.*

Before you ask Etak anything, write down — in your own words — three or four assumptions you think this idea rests on. Don't overthink it. Just list them. You'll compare to what the tool surfaces.

## Step 1. Open the sound

Type:

```
I want to surface assumptions for an idea: an in-app onboarding
checklist that guides new users through five key actions in
their first session.
```

Etak will enter **sound** and ask a couple of framing questions: who the user is, what "key actions" means here, what the success metric on the underlying opportunity is. Answer briefly. You don't need to have the answers polished; the point of the exercise is to pressure-test what you think you know.

## Step 2. Read the first pass

Etak will propose assumptions in categories. A typical pass produces six to ten. For the checklist idea, expect things like:

- **Desirability.** New users want a guided path rather than open exploration.
- **Desirability.** The five chosen actions are actually the five that predict success.
- **Usability.** A checklist UI doesn't feel infantilizing to the target user.
- **Feasibility.** We can reliably detect completion of each action.
- **Viability.** Guided completion correlates with retention at day 30.
- **Causal.** The guide causes retention — it doesn't just select for users who were going to retain anyway.

Read each one. Compare to the list you wrote before. The comparison is the lesson.

Two patterns to watch for:

- **Beliefs you held but didn't name.** "The five chosen actions are the right five" is an assumption most PMs hold implicitly. Once stated, it becomes a specific, testable thing. You probably skipped it on your pre-list.
- **Beliefs the tool proposed that you don't share.** Push back. "I don't think the checklist-feels-infantilizing thing is real — we've seen users respond well to structure in this segment." The tool will ask what evidence you have. If you have some, tighten the assumption. If you don't, keep it in with a lower importance rating.

## Step 3. Rate importance honestly

For each assumption, Etak will ask for **importance**: if this belief is wrong, how badly does the idea fail?

Three rough calibration points:

- **High.** If this is wrong, the idea cannot be saved. The whole feature is a waste.
- **Medium.** If this is wrong, the feature still ships but the impact is significantly smaller.
- **Low.** If this is wrong, we tune the feature and move on.

"Everything is high" is the most common failure mode. Push back against yourself. If three assumptions are all rated high, ask: if only one could fail, which would hurt the most? That is your real high. Demote the others.

## Step 4. Rate evidence honestly

Then **evidence**: how much do we actually know?

- **High.** Multiple independent data sources, including recent behavioral evidence, all point this direction.
- **Medium.** Some direct evidence, or strong indirect evidence. One good study, a clear analog from a similar product.
- **Low.** Hunches, a couple of conversations, analyst intuition. No behavioral data.

The seductive lie is medium. Most beliefs are low-evidence until someone explicitly checks. "We've talked to some users about this" is low. "We ran a five-person study last quarter and four responded this way" is medium.

## Step 5. Look at the risk quadrant

Ask:

```
Show me the risk view.
```

Etak displays the assumptions arranged by importance against evidence. The top-left quadrant — high importance, low evidence — is your attention. Those are the beliefs you are betting the outcome on without evidence.

If the quadrant is crowded, you have a risky idea. That's fine — some ideas are worth a risky bet, but you should be choosing it consciously. If the quadrant has one clear entry, that is the single test that most de-risks the idea.

If the quadrant is empty, two possibilities: the idea is well-understood and you can proceed, or (more common) you under-rated importance or over-rated evidence somewhere. Push on both columns.

## Step 6. Watch for shared assumptions

Ask:

```
Are any of these assumptions shared across ideas?
```

Etak scans the graph. If the checklist idea lives alongside, say, a "setup wizard" idea and an "interactive tutorial" idea, they probably share desirability assumptions about new users wanting structure. A shared assumption is worth more than the sum of its parts — one test informs all three ideas.

If the graph finds nothing shared, it may be because the space is still thin. It may also mean you're missing a connection. Look manually. Does another idea depend on "new users want a guided path"? Link it.

## Step 7. Decide what to do

At this point you have three honest options. Pick one explicitly.

1. **Test.** The riskiest assumption is worth a cheap experiment. Move to [designing experiments](designing-experiments.md).
2. **Pivot.** The assumptions are so shaky that a different idea would be better. Go back to [explore](../skills/explore.md) and generate alternatives from the same opportunity.
3. **Proceed.** The assumptions are acceptable risk given the strategic context. You've seen the risks clearly, you've chosen to take them, and the graph records that choice.

"Proceed" is a legitimate option. Etak is not a guilt machine. Well-surfaced assumptions that you consciously choose to take on are the whole point. What you want to avoid is proceeding because you didn't notice the assumptions.

## Go back to your original questions

Look at the list you wrote before you started, next to the list Etak produced, next to the risk quadrant.

- **The last failed idea.** Would this exercise, done before you started, have surfaced the belief that turned out to be wrong? Probably yes, if you were honest about evidence.
- **The deepest unexamined belief.** Is it in the graph now? If not, add it as an assumption with low evidence. You just made it testable.
- **The disagreement with a colleague.** Is there an assumption in the graph that would locate the disagreement precisely? If not, ask the colleague what they think is wrong with the idea. Translate their objection into an assumption they think is false. Add it. You now have a productive argument instead of a stuck one.

## What to try next

- [**Designing experiments**](designing-experiments.md) — turn the top-left-quadrant assumption into a test you can actually run.
- [**Critiquing an idea**](critiquing-an-idea.md) — a different angle on the same idea. Sound looks inward at assumptions. Critique looks from outside.
- Try the sound move on an idea a colleague is pushing. The same move works on other people's ideas, and the conversation that results is often more productive than debating the idea directly.
