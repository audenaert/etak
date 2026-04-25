# PM and engineer together

The hardest seam in product work runs between the person who decides what to build and the person who decides how to build it. Etak's two plugins (discovery for the what, develop for the how) put that seam in the graph, where a link called `from_discovery` carries a validated idea from the discovery side into the project or story that will ship it. The link is not decorative. It changes the conversation.

This pattern is about using discovery and develop together across that seam. Who surfaces which kind of risk, what a handoff actually contains, how the two sides stay honest with each other when the engineer hits a feasibility ceiling the PM didn't see.

## The move

Start discovery alone or with a research partner. Don't invite the engineer into sound or critique too early. The PM's job there is to externalize what the team is actually betting on, in language that is still soft enough to move. An engineer in the room at that stage tends to pull the conversation toward what is buildable, which is the wrong pressure for that moment.

When the idea has been sounded and at least one high-risk assumption has been tested, invite the engineer. The conversation opens with one thing:

```
Here is the validated idea and the evidence under it. Does
this hold up against the codebase we'd actually be changing?
```

The engineer runs an assess pass in develop against the current state of the code. What comes back is a second set of risks: technical feasibility, unstated dependencies, migration cost, operational load. These are not reasons to kill the idea. They are the other half of the material the decision needs.

From there, the PM either creates a project in develop with `from_discovery` pointing at the validated idea, or sends the engineer's findings back into discovery as a new set of assumptions and experiments. If the feasibility concern is unfamiliar, it becomes an assumption under the idea and gets the same treatment as any other belief.

## Why it works

Discovery and develop press on different kinds of risk. Discovery presses on desirability and viability. Develop presses on feasibility and operational cost. Most bad decisions come from one side not knowing what the other side learned. A graph with a `from_discovery` link closes that loop: the engineer can read the assumptions the PM surfaced and the experiments that tested them, and the PM can read the spec and the ADRs that captured the technical decision.

The link is one-way on purpose. Development reads discovery context. Discovery never reaches forward into execution state. This matters for two reasons. First, execution state moves faster than discovery state, and coupling them tightly would make discovery shake every time a ticket moves. Second, it keeps the two models honest. Discovery is about what is worth doing. Develop is about what is being done. Conflating them produces the classic failure mode where the roadmap becomes the strategy.

## Common failure mode

The PM hands a validated idea to the engineer without the assumption context. The engineer reads the idea, forms their own guesses about why it matters, builds something that satisfies the idea but misses the actual bet.

The `from_discovery` link fixes this mechanically, but only if the engineer reads it. Some engineers will not. The PM's job is to make the discovery context the first thing the engineer sees when they open the project, and the engineer's job is to read it before starting the spec. If either side skips the step, the seam leaks.

A second failure: the engineer assesses feasibility and finds the idea impractical. Instead of sending the finding back as a new assumption, they kill the work unilaterally. This might be right on the merits, but it takes the decision out of the shared model. The PM never sees the reasoning. Six months later, a different engineer concludes the same work is tractable after all, and nobody remembers why it was declined before. Feasibility concerns belong in the graph as much as desirability concerns.

## A worked example

The PM has validated an idea to let researchers filter saved papers by citation count. Two high-risk assumptions tested green: researchers want the filter, and they will use it monthly. The PM creates a project in develop with `from_discovery` pointing at the idea.

The engineer reads the discovery context, opens assess, and runs it against the codebase:

```
Assess the feasibility of the citation-count filter against
the current papers data model.
```

The tool surfaces a concern: citation counts are not currently stored; they would need to be fetched from an external service on a refresh schedule, and the refresh infrastructure does not exist. Cost is a new scheduled job, a new cache, and a background error path the team hasn't built before. The engineer flags this.

The PM could treat this as a kill signal. Instead, they send it back into discovery as a new assumption: *the value of the citation-count filter exceeds the cost of building refresh infrastructure.* That assumption now sits under the idea, with evidence weighted by what the engineer found. A small experiment, estimating the infrastructure cost and comparing against the projected usage from the validated assumption, either greenlights the work or shelves it, and either way the reasoning is in the graph.

Two weeks later, a different engineer asks whether to pick up the work. The graph answers. Nobody has to remember.

## What each side brings

The PM brings the customer signal, the language of the opportunity, the assumptions that had to be true, and the experiments that tested them. The engineer brings the codebase context, the operational cost, and the feasibility of the specific implementation. Neither has the full picture alone. The `from_discovery` link is the mechanism by which each side's material becomes readable by the other.

When it works, the conversation at the seam is short and specific. Both sides already know what the other has been looking at. The argument is about the current decision, not about what the team has been doing for the last month.

## Related

- [Develop overview](../develop.md): the develop plugin and the `from_discovery` link.
- [Spec skill](../skills/spec.md) and [assess skill](../skills/assess.md): the moves the engineer makes first.
- [Sound skill](../skills/sound.md): the move the PM makes before the handoff.
