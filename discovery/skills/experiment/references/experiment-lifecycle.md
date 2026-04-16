# Experiment Lifecycle

This is your reference for tracking experiments from planning through completion and
making sure the learning they produce actually reaches the rest of the opportunity space.
The design happens once; the lifecycle is where the ongoing discipline lives.

## Status Progression

Experiments move through three statuses: **planned**, **running**, **complete**. Each
transition is a moment of intentionality, not a status update.

## Starting an Experiment (planned --> running)

Starting isn't just flipping a flag. It's a readiness check.

**Before marking an experiment as running, confirm:**

- Success criteria are defined and specific. If they're vague, now is the last chance to
  sharpen them. Once the experiment is running, changing criteria is retroactive
  rationalization.
- The pre-committed action plan exists. The PM knows what they'll do with each outcome.
- Logistics are in place — participants recruited, prototype built, survey written,
  whatever the method requires.
- The PM can state, in one sentence, what they're trying to learn.

**Moves when readiness is questionable:**
- "Your success criteria say 'users find value.' Can we make that more specific before
  you start? It's much harder to define success after you've seen the data."
- "You don't have a plan for what to do if this is invalidated. That's the scenario where
  having a plan matters most."

Note the start date. If the experiment has an estimated duration, you now have a clock
running — use it to nudge the PM when time is up.

## Recording Results (running --> complete)

This is the most important moment in the experiment lifecycle. It's where raw data
becomes learning, and where the temptation to rationalize is strongest.

### Gathering What Happened

Start with the facts before moving to interpretation. Ask the PM to walk through what
they observed, what the data showed, what surprised them.

**Moves for drawing out results:**
- "Walk me through what happened. What did you see?"
- "How did the actual results compare to what you expected?"
- "Was there anything that surprised you — something you didn't anticipate?"

### Classifying the Outcome

Hold the results up against the pre-committed success criteria and interpretation guide.
This is a moment to be honest, not optimistic.

**Validated** — the success criteria were met. The assumption has evidence supporting it.
This doesn't mean it's proven forever — it means the experiment produced supporting
signal.

**Invalidated** — the success criteria were not met. The assumption is challenged. This
is not failure — this is the system working. The PM just avoided building something on a
false belief.

**Inconclusive** — the results don't clearly support or challenge the assumption. The
ambiguous middle ground that the interpretation guide should have anticipated.

**Moves when the PM wants to rationalize:**
- If results clearly missed the criteria but the PM wants to call it validated: "The
  criteria you set in advance were X. The results were Y. Those don't match. What's
  changed — is it the criteria that were wrong, or the assumption?"
- If the PM wants to move the goalposts: "You defined success as X before the experiment.
  If you'd seen these results and defined success differently, that's worth understanding,
  but let's call the original criteria what they are first."
- If the PM is deflating a genuine success: "The results met your criteria. Take the win.
  What did the pre-committed action plan say to do next?"

Don't be rigid. Sometimes criteria were genuinely poorly set, and the PM learned something
valuable despite missing the bar. Name that: "The experiment didn't meet your stated
criteria, but it surfaced something important. Let's record both — the criteria result
and the unexpected learning."

### Extracting Learnings

The binary result (validated/invalidated/inconclusive) is the headline, but the learnings
are often more valuable.

**Moves for surfacing learning:**
- "What did you learn that you didn't expect?"
- "If you ran this again, what would you change about the experiment itself?"
- "Did anything come up that changes how you think about the opportunity, not just the
  assumption?"

Learnings sometimes reveal new assumptions, new opportunities, or new ways of thinking
about the problem. Capture these — they're seeds for future discovery work.

## Propagating Outcomes

This is the highest-value, most-skipped activity in the entire discovery system.
Experiments that complete without propagation are learning that evaporates.

### Update the Tested Assumption

The assumption's status and evidence should reflect what the experiment taught.

- Validated --> assumption status becomes `validated`, evidence field updated with what
  was learned and when.
- Invalidated --> assumption status becomes `invalidated`, evidence field updated.
- Inconclusive --> assumption stays `untested`, but evidence field captures what the
  experiment showed and why it wasn't conclusive.

### Review Impact on Parent Ideas

An assumption doesn't exist in isolation. It supports one or more ideas. When its status
changes, the ideas need to be reconsidered.

**When a critical assumption is invalidated:**
- The parent idea is now standing on a broken foundation. Surface this directly: "This
  was a critical assumption for [idea]. Now that it's invalidated, the idea needs
  rethinking."
- Can the idea be adapted? Maybe the core insight survives but the approach changes.
- Should the idea be shelved? If the invalidated assumption was the load-bearing wall,
  maybe the idea isn't viable in any form.
- Are there alternative approaches? The opportunity may still be real even if this
  particular solution doesn't work.

**When all critical assumptions are validated:**
- The idea has passed its riskiest tests. Suggest advancing its status. "Every critical
  assumption for this idea has been validated. It might be ready to move from discovery
  into scoping."

### Check for Shared Assumptions

This is the highest-leverage insight in the system. One assumption may underlie multiple
ideas. When it's tested, the result radiates outward.

Trace every `assumed_by` link. For each idea that shares the assumption, surface the
implication: "This assumption also supports [idea-2] and [idea-3]. The validation
applies to all of them. [idea-2] now has one fewer untested critical assumption."

Or, for invalidation: "This assumption was also critical to [idea-2]. The invalidation
affects that idea too — it may need the same rethinking."

### Suggest Next Actions

Don't leave the PM with updated statuses and no direction.

**After validation:** "The next riskiest assumption for this idea is X. Want to design
an experiment?" Or: "All critical assumptions are validated. Is this idea ready to
scope?"

**After invalidation:** "The idea may need to change. Want to brainstorm alternative
approaches to the same opportunity?" Or: "There are two other ideas addressing this
opportunity — want to re-prioritize?"

**After inconclusive results:** "You could expand the sample size, try a different
method, or gather more qualitative data first. Which feels right?" Or: "Is there a
cheaper way to get at this question?"

## When Experiments Pile Up

If multiple experiments are planned but not starting, or running but not completing,
or complete but not propagated — name the pattern.

**Planned but not starting:** "You have four experiments planned and none running. Is
something blocking them? Would it help to prioritize which one to run first?"

**Running past duration:** "This experiment was estimated at one week. It's been three.
Do you have results to record, or does it need more time?"

**Complete but not propagated:** "Two experiments finished last week but the results
haven't been connected back to the assumptions and ideas they tested. That's learning
going to waste. Want to propagate them now?"

The opportunity space is a living system. Experiments are its feedback loops. When the
loops don't close, the system stops learning.
