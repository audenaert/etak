# Designing Experiments

This is your reference for helping PMs design experiments that produce real signal. The
principles here apply regardless of method — whether it's a five-person prototype test
or a thousand-user A/B experiment.

## The Five Elements

A well-designed experiment has five things. Without all five, you're not running an
experiment — you're doing stuff and hoping to learn.

### A Clear Assumption

Every experiment tests a specific, stated belief. Not "let's see what happens" but "we
believe X, and this experiment will tell us whether X is true."

If the PM can't articulate the assumption, the experiment isn't ready. Help them find it:
"What would have to be true for this idea to work? Which of those beliefs are you least
sure about?"

### An Appropriate Method

Match the method to what you're trying to learn, not to what's comfortable or familiar.

**Desirability** — "Users want X." Test with user interviews, fake door tests, surveys,
or landing page experiments. You're measuring expressed or revealed preference.

**Usability** — "Users can do X." Test with prototype tests, concierge MVPs, or
Wizard-of-Oz setups. You're observing actual behavior, not asking about hypothetical
behavior.

**Feasibility** — "We can build X." Test with technical spikes, proofs of concept, or
data analysis. You're testing capability, not desirability.

**Viability** — "X is sustainable." Test with data analysis, financial modeling, or
concierge MVPs with real economics. You're testing the business mechanics.

**Causal** — "X leads to Y." Test with A/B tests, prototypes with measurement, or
before-and-after analysis. You need controlled comparison or careful observation.

For each method, think about what it costs (time, effort, access to users), what quality
of evidence it produces, and what it can't tell you. A five-person interview produces
rich qualitative signal fast but can't quantify adoption rates. An A/B test quantifies
precisely but takes weeks and requires traffic.

**The smallest viable experiment.** Always push toward the smallest test that produces
enough evidence to make a decision. Not perfect certainty — just enough to act. "What's
the cheapest way to learn whether this is worth pursuing?"

### Specific Success Criteria

This is where most experiments fail before they start. Vague criteria produce ambiguous
results, which produce arguments about interpretation, which produce no decision.

**Moves for sharpening criteria:**

When the PM says "users find it useful" — ask "What observable behavior indicates
usefulness? What would you see them do?"

When the PM says "it performs well enough" — ask "What's the threshold? Below what number
would you abandon this approach?"

When the PM says "people are interested" — ask "How will you measure interest?
Clicks? Sign-ups? Time spent? Words said in an interview?"

**Properties of good criteria:**

- **Specific** — not "users like it" but "7 of 10 users complete the task without
  assistance"
- **Measurable** — objectively determinable by someone who wasn't in the room
- **Set in advance** — defined before seeing results, not adjusted after
- **Calibrated** — neither trivially easy to pass (every experiment "validates") nor
  unfairly hard (nothing could pass). Ask: "If the idea is actually good, would we
  expect to hit this bar? If the idea is actually bad, would we expect to miss it?"

Also define what **inconclusive** looks like. There's usually an ambiguous middle ground
between clear success and clear failure. Name it in advance so you don't have to argue
about it after.

### An Interpretation Guide

Results rarely speak for themselves. Help the PM think through the space of possible
outcomes before they happen.

**What does a clear yes look like?** The success criteria are met, the signal is strong,
the evidence is unambiguous.

**What does a clear no look like?** The criteria are missed decisively, not by a hair.

**What are the ambiguous middle scenarios?** Partial success, mixed signals, criteria met
but with concerning patterns. Name the specific scenarios: "What if 5 of 10 engage but
only 2 rate relevance above 4? That's behavioral interest without perceived quality."

**What confounding factors could muddy results?** Selection bias in recruitment, novelty
effects, social desirability in interviews, small sample noise. You can't eliminate
confounds, but you can name them so you don't over-interpret.

**What should you NOT conclude from this experiment?** Every experiment has boundaries.
A five-person test doesn't tell you about adoption rates. A fake door test doesn't tell
you about retention. Name the limits explicitly so the PM doesn't over-generalize.

### Pre-Committed Actions

Before the experiment runs, the PM commits to what they'll do with each outcome.

**If validated** — What's the next step? Advance the idea? Test the next assumption?
Build a prototype?

**If invalidated** — What happens to the idea? Can it be adapted? Should it be shelved?
Are there alternative approaches worth exploring?

**If inconclusive** — What's the follow-up? Bigger sample? Different method? Additional
data? "We'll figure it out" is not a plan.

Pre-commitment prevents the most dangerous failure mode in discovery: post-hoc
rationalization. Without it, invalidated experiments get explained away, inconclusive
results get declared victories, and the team builds what they were going to build anyway.

Push on this: "What would you do differently if this experiment didn't exist? If the
answer is nothing, the experiment isn't worth running."

## Multi-Assumption Experiments

Sometimes one experiment can test multiple assumptions. This is efficient but increases
interpretation complexity. If you do it, make sure the success criteria address each
assumption independently. "We'll know desirability is validated if X. We'll know usability
is validated if Y." If the criteria are entangled — one test, one result, two assumptions —
you can't tell which assumption the result speaks to.

## When to Push Back on Experiment Design

**The PM wants to skip success criteria.** This is non-negotiable. Without criteria, there
is no experiment, only activity.

**The PM wants a huge experiment.** Ask what the smallest version is. Can you test with
5 users instead of 50? Can you use a mockup instead of a prototype? Can you have
conversations instead of building a survey tool?

**The PM already knows what they want to build.** If the experiment can't change the
decision, don't run it. "If this came back invalidated, would you actually change course?
If not, save the effort and just build it."

**The criteria are designed to pass.** If the bar is so low that any result would
"validate," the experiment is theater. "Could a bad version of this idea pass these
criteria? If so, the bar is too low."
