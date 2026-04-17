# prioritize

**Stance:** evaluative. **Purpose:** converge on what matters most, given your constraints.

Prioritize is the convergent face of Etak. Where [explore](explore.md) expands the space of possibilities, prioritize narrows it. It helps you decide where to put your limited attention, time, and political capital.

The skill presents analysis, surfaces tradeoffs, and challenges ratings. It does not decide. Prioritization is a conversation, not a formula. The PM brings judgment about context, stakeholders, and strategy. The tool brings rigor about evidence and the structure of the decision.

## When to reach for it

- **Too many options on the table.** Eight ideas, twenty assumptions, five experiments worth running. Prioritize narrows to the set worth attention.
- **Limited time or bandwidth.** Most prioritization conversations are really conversations about constraints. Tell the tool the constraint and it filters the options accordingly.
- **A stakeholder or leadership conversation.** When you need to explain why you're working on A and not B, a defensible ranking beats a gut feeling.
- **Pre-planning.** Before a sprint, before a quarter, before a roadmap review. Prioritize the work, then the conversations about the work become shorter.

Trigger phrases: *which should we focus on*, *rank these*, *prioritize*, *what's most important*, *triage*, *what should we do first*.

## How it behaves

Prioritize works on any set of artifacts — assumptions, ideas, opportunities, objectives — and applies the appropriate ranking method to each.

### Assumptions: risk-based

Importance times inverse evidence. High importance, low evidence goes to the top. These are the beliefs you're betting the outcome on with the least to back them up. Those are the assumptions worth testing first.

### Ideas: multiple factors

Ideas get ranked on a combination: value to the parent opportunity, cost to build and test, confidence (how many assumptions tested so far, with what results), strategic fit. The tool presents the factors explicitly so you can see and challenge the weighting.

### Opportunities: potential and alignment

Opportunities rank on potential impact to the objective, evidence strength (how well we understand the need), and alignment with strategic context you provide.

### Objectives: strategic judgment

Objectives are rarely prioritized numerically. Prioritize here means making the strategic call explicit — which objective is the team actually most responsible for delivering this quarter, and which are secondary. The tool surfaces conflicts ("two of your three objectives compete for the same user attention") but the call is yours.

## The central tension

Everything feels important when you're close to it. The job of prioritize is to create distance.

When a PM says everything is high priority, that means nothing is prioritized. The tool pushes back. "If you could only test three of these ten assumptions, which three?" "If you had two weeks instead of two months, which opportunity would you pursue?" Forcing the constraint reveals the ranking that lives in your head but hasn't been articulated.

Push back on the tool too. If its top pick feels wrong, say so — "I know the risk math says X, but we tested something adjacent last quarter and learned something relevant." The tool will update the rationale and the ranking. Judgment lives with the PM.

## How to use it well

**Give it a real constraint.** "Prioritize everything" is a weak prompt. "Prioritize these assumptions given I have three days and one engineer" is strong. The constraint is what produces a useful ranking.

**Watch for ratings that contradict behavior.** Sometimes the PM's stated ratings contradict their behavior. They say an assumption is high-importance but haven't tested it in three months. They say an opportunity is low-severity but keep circling back. The tool names the discrepancy. Answer honestly. If the behavior is right and the rating is wrong, update the rating.

**Prioritize is not a one-shot exercise.** The graph changes. A ranking from last month may be wrong this month — an experiment finished, a new signal came in, a competitor shipped something. Re-prioritize periodically. The cost is low and the drift is real.

**Connect to action.** A ranked list is not an outcome. Every prioritization conversation should end with a concrete next step. "We've identified your riskiest assumption — want to design an experiment?" "Your top opportunity has no ideas — want to brainstorm?" If you walk away with a ranking and no plan, the session wasn't finished.

## Updating ratings mid-conversation

Prioritize sometimes shifts ratings during the conversation itself. A PM realizes an assumption is more important than they thought. New evidence lands that changes confidence. Prioritize updates artifacts directly, with confirmation: "Based on this conversation, I'd update [assumption] from low to high importance. Sound right?" These are small edits to frontmatter fields, not rewrites.

## Common failure modes

- **Treating ranking as the output.** The output is what you do next. A ranked list is intermediate.
- **Using prioritize to avoid hard calls.** If the ranking keeps coming out "unclear, need more data," the tool is either right (design an experiment) or you're avoiding the call the context actually demands. Notice which.
- **Ignoring the political dimension.** Some ideas are high-priority not because of their technical merit but because the team needs a win or a stakeholder needs a signal. The tool can model this if you tell it about the context. Don't pretend the context isn't there.
- **Over-fitting to framework weights.** The tool offers ranking frameworks with explicit weights (importance, evidence, effort, value). The weights are starting points, not truths. Adjust them if your context demands it.

## Transitions

Prioritization reveals what to do next. Name the next move:

- Riskiest assumption identified → [**experiment**](experiment.md) ("want to design a test?")
- Area of the graph is thin → [**explore**](explore.md) ("your top objective has only one opportunity, want to brainstorm more?")
- Ideas lack examined assumptions → [**sound**](sound.md) ("before ranking, we should know what these are betting on")
- Step back to reflect → [**orient**](orient.md) ("sounds like you want to sit with this before acting")

## Related

- [Skills index](README.md)
- [Artifacts reference](../artifacts/) — prioritize ranks these.
- [Assumption artifact](../artifacts/assumption.md) — importance and evidence are the inputs.
