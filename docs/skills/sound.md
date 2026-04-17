# sound

**Stance:** exploratory, curious, not adversarial. **Purpose:** surface the beliefs an idea is resting on.

The name comes from nautical practice. Before committing to a course, sailors *sound* the water — they lower a weighted line to learn what's beneath the surface. In discovery, the same move applies. An idea looks solid on the surface. The PM can explain it, the team is excited, the slides are clean. Sounding reveals what it's resting on: the beliefs about users, about causation, about feasibility that are so embedded they've become invisible.

When those beliefs are stated back to the PM explicitly, the idea talks back. This is the moment of recognition — "oh, I didn't realize I was assuming that" — and it is the core thing sound produces.

## When to reach for it

- **After capturing an idea.** Before you commit to building or even critiquing, surface what the idea depends on.
- **When you're unsure why an idea feels right.** Strong conviction with no articulated foundation is a warning sign. Sound converts the feeling into statements you can examine.
- **When the team is divided on an idea.** Disagreements about ideas are usually disagreements about hidden assumptions. Surface the assumptions and the argument becomes productive.
- **Before designing an experiment.** You can't test what you haven't articulated. Sound produces the inputs to [experiment](experiment.md).

Trigger phrases: *what are we assuming*, *what must be true*, *what's underneath this*, *what are we betting on*, *is this resting on anything shaky*.

## How it behaves

Sound is curious, not combative. It is alongside you, turning the idea over together. You should feel like you're discovering the foundations of your own thinking, not defending it.

The skill produces assumptions in four or five categories:

- **Desirability.** Do users want this? Will they engage with it?
- **Usability.** Can users actually use it as designed? Is the interaction what we think it is?
- **Feasibility.** Can we build it? Can we operate it at scale?
- **Viability.** Does it move the business metric? Does it make money, reduce cost, retain users?
- **Causal.** Does our intervention cause the outcome, or does it correlate with something that was going to happen anyway?

For each assumption, sound helps you:

1. **State it as a testable belief.** Not "users will like this" — "on-call engineers will spend more than thirty seconds in the configuration UI on their first visit."
2. **Rate importance.** If wrong, how badly does the idea fail? (High, medium, low.)
3. **Rate evidence.** What do we actually know? (High, medium, low.)
4. **Name what breaks.** If this is wrong, what happens to the idea?

The output is a set of assumption artifacts linked to the parent idea. See [assumption](../artifacts/assumption.md).

## How to use it well

**Be honest about evidence.** The seductive lie is "medium." Most beliefs are low-evidence until someone explicitly checks. "We've talked to some users about this" is low. "We ran a five-person study last quarter and four responded this way" is medium.

**Don't collapse importance into 'high.'** If everything is important, nothing is prioritized. Force yourself to rank. If three things are all high, ask: if only one could fail, which would hurt the most? That is the real high.

**Look for shared assumptions.** The highest-leverage assumptions are the ones underlying multiple ideas. Sound will flag these. One experiment that tests a shared assumption de-risks everything that rests on it.

**Push back on assumptions that don't feel right.** Sound proposes a draft set. Some will miss, some will land. Reject the ones that miss. Add ones the tool skipped. The output is only as good as the conversation.

**Sound is not a checklist.** Four categories doesn't mean every idea has four assumptions. Some ideas are mostly desirability risk with feasibility settled. Some are the reverse. Don't pad.

## How sound differs from critique

This distinction matters. **Sound** probes the assumptions *beneath* a single idea — it looks inward and downward at the beliefs the idea rests on. **Critique** examines the artifact *itself* from multiple *external* viewpoints — personas, frameworks, angles you haven't considered.

Sound asks *what are we betting on?* Critique asks *what would a skeptic see that we don't?*

They complement each other. Sound usually happens first (what are we assuming?), then critique examines the idea from outside. But either can surface work for the other. A critique finding often becomes a new assumption. An assumption examination often reveals a concern worth critiquing.

## Common failure modes

- **Vague assumptions.** "Users will like this" is not an assumption. "Users in segment X will open the feature at least twice in their first week" is. Push for specificity.
- **Assumptions that can't be tested.** If you can't imagine an experiment that would settle the question, the assumption is too abstract. Break it down.
- **Dismissing the obvious.** The most dangerous assumptions are the ones that feel too obvious to name. Sound them anyway. The "obvious" one is often the one that turns out to be wrong.
- **Sound without action.** Surfacing assumptions and then proceeding to build without testing the risky ones is a form of discovery theater. The output of sound should lead to an experiment, a pivot, or a conscious decision to accept the risk.

## Transitions

- High-importance, low-evidence assumption surfaced → [**experiment**](experiment.md) ("want to design a test?")
- Too many assumptions, need to rank → [**prioritize**](prioritize.md) ("which should we test first?")
- Assumptions suggest the idea may be flawed → [**critique**](critique.md) ("several of these look fragile, want to stress-test the idea itself?")
- Shared assumption across ideas → [**prioritize**](prioritize.md) ("three ideas rest on this, testing it once informs all of them")

## Related

- [Skills index](README.md)
- [Assumption artifact](../artifacts/assumption.md) — the thing sound produces.
- [Surfacing assumptions tutorial](../tutorials/surfacing-assumptions.md) — runnable walkthrough.
- [Foundations](../context/foundations.md) — Schön's "back-talk of the materials." Sound is the mechanism.
