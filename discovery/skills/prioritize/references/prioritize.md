# Prioritization Frameworks

This is your reference for evaluating and ranking different artifact types. Each type
has a natural framework, but the real work is in the conversation — challenging ratings,
surfacing hidden tradeoffs, and connecting the ranking to a concrete action.

## Assumption Prioritization: The Risk Matrix

The Torres importance-x-evidence matrix is your primary tool. Every assumption lives
somewhere in this space:

```
                    IMPORTANCE
                 High         Low
EVIDENCE  High | Monitor    | Park      |
               | (safe)     | (low risk)|
          Low  | TEST FIRST | Revisit   |
               | (risky!)   | (unclear) |
```

**High importance, low evidence — TEST FIRST.** These are the riskiest bets in the
opportunity space. If they're wrong, the idea they support collapses. And you don't
actually know if they're right. This quadrant drives the experiment queue.

**High importance, high evidence — Monitor.** Probably true, and it matters. Keep an
eye on these but don't spend testing resources here unless something changes.

**Low importance, low evidence — Revisit.** Unclear if they matter, unclear if they're
true. Not worth testing now, but worth checking back when understanding deepens.

**Low importance, high evidence — Park.** Safe to ignore. True and inconsequential.

### Challenging the Ratings

Ratings are starting points, not facts. The conversation is where the real prioritization
happens.

**Moves for challenging importance:**
- "You rated this high importance. Walk me through what breaks if it's wrong."
- "Is this actually critical, or just uncomfortable to be wrong about?"
- "If this assumption turned out to be false, could the idea be adapted, or is it dead?"

**Moves for challenging evidence:**
- "You rated evidence as high. What's that based on — data, conversations, intuition?"
- "When was the last time you tested this directly? Things may have shifted."
- "Is this evidence from your target users, or from a different context?"

**Moves for the PM who rates everything high:**
- "If you could only test three of these, which three? The others are medium by definition."
- "Imagine explaining to your team why this one matters more than the others — what would you say?"
- "Which of these would make you lose sleep if it turned out to be wrong?"

### Shared Assumptions

Assumptions that appear across multiple ideas have amplified importance regardless of
their individual rating. One experiment can inform three ideas. Flag these explicitly:
"This assumption underlies four of your ideas — testing it is the highest-leverage thing
you can do right now."

## Idea Prioritization

Ideas live in a space of impact, effort, and confidence.

- **Impact** — How well does this address the parent opportunity? How many users benefit?
  How deeply?
- **Effort** — Rough implementation complexity. Don't over-specify — this is discovery,
  not sprint planning.
- **Confidence** — How validated are the underlying assumptions? An idea with untested
  critical assumptions is a guess, no matter how appealing.

### Categories

**Quick wins** — high impact, low effort, decent confidence. Do these. They build
momentum and free up capacity for bigger bets.

**Big bets** — high impact, high effort. Worth it if the underlying assumptions validate.
The question is whether to invest in validation before committing engineering time.

**Easy experiments** — low effort, high learning potential. Even if impact is uncertain,
the information value may justify the small investment.

**Defer** — low impact, or low confidence with no clear validation path. Not never — just
not now.

### Failure mode: ranking ideas without examining assumptions

If the PM wants to rank ideas but hasn't surfaced assumptions for any of them, flag it.
"We're ranking these based on intuition about impact, but we haven't examined what they're
betting on. Want to surface assumptions first so we know which rankings are backed by
evidence and which are hopes?"

## Opportunity Prioritization

Opportunities sit between strategic objectives and concrete ideas. Evaluate on:

- **Frequency** — How often do customers encounter this pain or unmet need?
- **Severity** — How painful is it when they do? Does it block progress or merely annoy?
- **Breadth** — What fraction of your target audience is affected?
- **Strategic alignment** — How directly does this serve the parent objective?
- **Evidence strength** — How confident are you this opportunity is real, not projected?

### High severity, low evidence

This combination demands attention. If the pain is severe but you're not sure it's real,
a small research investment has high expected value. Flag these: "This could be your most
important opportunity or a phantom — a few customer conversations would tell you which."

## Objective Prioritization

When the PM has multiple objectives competing for attention:

- **Downstream activity** — Which objective has the most developed opportunity space?
  That's where momentum lives. (But watch for sunken-cost bias.)
- **Evidence strength** — Which objective's opportunities are backed by the strongest
  evidence? Pursuing well-evidenced opportunities is less risky.
- **Time sensitivity** — Are any of these objectives expiring? Market windows, competitive
  pressure, strategic commitments?
- **Strategic optionality** — Which objective, if pursued, opens up the most future
  options? Some objectives are one-way doors; others create platforms.

## Presenting Results

Format every prioritization as a clear ranked list. For each item:

1. **Rank** — where it falls
2. **Name** — the artifact
3. **Key ratings** — the 2-3 dimensions that drove the ranking
4. **Rationale** — one sentence on why it's here, not higher or lower
5. **Recommended action** — what to do about this item specifically

End the list with a summary: "Your top priority is X because Y. The recommended next step
is Z." Then offer to help with that next step.

## When You Can't Rank Confidently

Sometimes the information isn't there. Two assumptions might be equally important with
equally thin evidence. Two opportunities might be genuinely equivalent. Say so. "I can't
differentiate these two — they're genuinely tied on the dimensions we're using. Is there
a tiebreaker you care about that we haven't considered?" Honest uncertainty is more useful
than false precision.
