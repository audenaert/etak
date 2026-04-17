# explore

**Stance:** generative. **Purpose:** see what's in the space, notice gaps, produce options.

Explore is the expansive face of Etak. Where [orient](orient.md) listens and reflects, explore looks outward. It handles two activities on a continuum: viewing the opportunity graph you already have, and generating new material to fill it out.

These are not separate activities. A PM who asks "show me the tree" is often, thirty seconds later, asking "we need more ideas for that opportunity." A PM who asks to brainstorm ideas for retention needs the tool to first see what's already in the graph so it doesn't regenerate it. Explore handles both ends and the transition between them.

## When to reach for it

- **Starting a new initiative.** You have an objective and want to see (or build) the opportunity space under it.
- **Filling gaps.** You've noticed an opportunity with no ideas, or an objective with one thin opportunity, and you want more options on the table.
- **Reviewing what you have.** You want to see the current state of the graph before deciding where to work next.
- **Bottom-up entry.** You have an idea and want to place it — which opportunity does it address, which objective does that serve.

Trigger phrases: *show me*, *what's there*, *brainstorm*, *I have an idea*, *what opportunities exist*, *explore ideas for*, *show me the tree*.

## How it behaves

Explore is biased toward abundance. It would rather put twenty options on the table and let you react than carefully construct three right ones. This is deliberate. The tool is best at generating; the PM is best at pruning. Let each do what it's best at.

Generation is grounded, not ungrounded. Explore reads the graph before producing anything. It knows what's already been tried, what's been tested, what's been shelved. The options it proposes build on the landscape rather than ignoring it.

### Graph viewing

Ask "show me the tree" and explore produces a readable view of the current state. The view is not a data dump. It highlights structure — which objectives have opportunities, which opportunities have ideas, which ideas have assumptions, which assumptions have experiments. It flags imbalance: one objective with twelve opportunities while another has zero usually indicates either a focus problem or a framing problem with the quiet objective.

You can filter by type (`show me only opportunities`), status (`show me draft ideas`), or focus (`show me everything under the engagement objective`).

### Divergent generation

Ask "brainstorm ideas for [opportunity]" and explore opens a generation session. It will propose ideas across different stances — direct solutions, adjacent solutions, contrarian approaches — and ask which ones are worth keeping. Saved ideas become draft artifacts. Unsaved ideas disappear. The tool does not clutter the graph with every brainstormed fragment.

Explore handles generation at any level:

- **Opportunities under an objective.** Given a business outcome, what customer needs could serve it?
- **Sub-opportunities under an opportunity.** Given a broad *how might we*, what narrower framings are worth exploring?
- **Ideas under an opportunity.** Given a customer need, what solutions could address it?

### Bottom-up entry

Not everyone thinks top-down. When you arrive with a specific idea ("I have an idea for fixing search"), explore does not redirect you to "start with an objective." It captures the idea first, then helps trace upward: what opportunity does this address, which objective does that serve, are there other ideas addressing the same opportunity?

The tracing reveals structure the PM may not have articulated. It also often surfaces the real insight — which is usually the opportunity, not the idea.

## How to use it well

**Generate more than you save.** A brainstorming session that produces three ideas and saves three is probably too convergent. Aim for a spread of twenty options, save the four or five worth developing. The discarded fifteen taught you what the space looks like.

**Explore before you sound.** Surfacing assumptions under the first idea you generate is tempting and often wasteful. Generate four or five ideas first. The comparison tells you which one is actually worth examining.

**Use it to refresh the graph.** Every few weeks, run a light explore pass on a neglected corner of the graph. Ask "what's thin here? what might we be missing?" The tool will propose opportunities and ideas that wouldn't have come up in the course of normal work. Some will be bad. Some will be the thing you've been missing.

**Trust the tool on breadth, trust yourself on taste.** Explore is very good at breadth — it will produce options you wouldn't have thought of. It is less good at the subtle judgment of which options matter for your specific strategic context. Use the tool's breadth and your own taste together. Neither replaces the other.

## Saving artifacts

Explore generates options but does not own artifact creation. When you want to save something, explore calls the relevant internal skill to write the artifact. You will always see the artifact content before it's written. You can edit or reject before anything lands on disk.

See the artifact references:
- [Objectives](../artifacts/objective.md)
- [Opportunities](../artifacts/opportunity.md)
- [Ideas](../artifacts/idea.md)

## Common failure modes

- **Brainstorming without reading the graph first.** Produces duplicates and regenerates work you already did. Explore tries to read first, but you can short-circuit the habit by asking for generation without context.
- **Keeping everything.** A graph with five hundred draft ideas is noise. Save what's worth developing. Let the rest go.
- **Generation without constraint.** "Brainstorm ideas for the engagement objective" is a bad prompt. "Brainstorm ideas for this specific opportunity, under this constraint, for this user segment" is much better. Give the tool something to push against.
- **Using explore to avoid sound or critique.** If you keep generating new ideas without examining any of them, the problem isn't a shortage of ideas. Go to [sound](sound.md) or [critique](critique.md).

## Transitions

Explore is where options are born. Other skills are where they get examined and tested.

- Generated an idea worth testing → [**sound**](sound.md) ("let's surface what this is betting on")
- Generated an idea that needs stress-testing → [**critique**](critique.md) ("let's poke holes before committing")
- Too many options on the table → [**prioritize**](prioritize.md) ("you have eight ideas, want to rank them?")
- Stepping back to reflect → [**orient**](orient.md) ("sounds like you want to take stock before generating more")

## Related

- [Skills index](README.md)
- [Artifacts reference](../artifacts/) — explore can create any of these.
- [Product-researcher agent](../agents/product-researcher.md) — use before explore sessions to ground brainstorming in external evidence.
