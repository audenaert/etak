# Graph hygiene

The graph you started with six weeks ago is not the graph you have now. An opportunity that once had three clean ideas now has twenty. Two opportunities are saying the same thing in slightly different words. A handful of experiments were designed and never run. Ideas appear under the "engagement" objective that you don't recognize and can't remember accepting.

This is normal. The graph is a record of your team's thinking, and thinking is messy. But a graph that has stopped being useful to read has stopped doing its job. Hygiene is the practice of pruning, merging, and decomposing often enough that the graph stays readable as a model of what the team currently believes.

## The move

Run an explicit hygiene pass on a cadence. Every two weeks is a defensible default; once a sprint boundary is also fine. Set aside thirty minutes. Do not combine it with generative work; the stance is different.

Start with the orient skill and ask a direct question.

```
What's gotten stale? What looks duplicative? Where is the graph
lying to me?
```

The tool walks the graph and proposes three piles:

- **Candidates for pruning.** Draft ideas with no assumptions under them and no activity for four-plus weeks. Experiments designed but never run past six weeks. Opportunities with a single thin idea that never moved.
- **Candidates for merging.** Opportunities whose *how might we* framings overlap by more than feel. Ideas that describe the same behavior from different angles. Assumptions stated two different ways under two different ideas.
- **Candidates for decomposition.** Opportunities with more than roughly eight ideas underneath. That count is not a rule, but past it, the opportunity is almost always two opportunities wearing one name.

For each pile, react. The tool proposes. You decide.

## Why it works

The graph earns its keep as a shared model the team can read in one sitting. When it gets noisy, people stop consulting it, and then it stops being the shared model and becomes another stale wiki. A small regular pass costs less than a big cleanup after the graph has been abandoned for two months.

The three moves (prune, merge, decompose) map to three distinct failure modes of an evolving model. Pruning removes material that has quietly gone stale. Merging removes duplication that accumulated because two people thought about the same thing on different days. Decomposition is the hardest and most valuable. When an opportunity has twenty ideas, the opportunity is usually too broad, and the ideas are clustering into two or three real shapes. Splitting it turns a junk drawer into two useful drawers.

## Common failure mode

The most common failure is not doing it at all. The second most common is doing it too aggressively and deleting material the team might have come back to. A shelved idea, kept with a note explaining why it was shelved, is often more valuable than a deleted one. When you next consider a similar idea, the graph will remind you what happened last time.

Use `status: shelved` or `status: invalidated` rather than outright deletion for ideas and assumptions that did real work before falling out of favor. Reserve actual deletion for material that was never load-bearing: accidental duplicates, brainstormed fragments that nobody argued for, typos promoted to nodes.

## A worked example

You open the graph and notice the "retention" objective has fourteen opportunities under it, some of which you do not recognize. You run a hygiene pass. The tool proposes:

Two opportunities, "help users return after a week away" and "surface recent activity on login," are restating the same need. You merge them, keeping the sharper *how might we* framing and carrying forward the assumptions from both.

Three ideas under "reduce friction at signup" haven't moved in eight weeks and have no assumptions under them. You shelve them with a one-line note: "not prioritized this quarter; revisit if signup conversion becomes the focus."

One opportunity, "improve the notification experience," has eleven ideas under it spanning email, in-app, mobile push, and digest logic. The tool suggests decomposing into three: notification content, notification channel, notification timing. You accept two of the three and reshape the third. The eleven ideas redistribute cleanly. The graph is legible again.

Thirty minutes, maybe forty. The next person who opens the graph can read it in five.

## Related

- [Orient skill](../skills/orient.md): the receptive stance that hygiene work runs in.
- [Artifacts reference](../artifacts/README.md): the types and statuses you are working with.
- [Anti-patterns](anti-patterns.md): keeping everything is the anti-pattern this page is the remedy for.
