# Skill or agent

Etak gives you two shapes of collaboration for the same underlying work. A skill pairs with you interactively: you describe the move, the tool proposes, you react, it revises. An agent runs autonomously: you hand it a body of work, it goes away for several minutes, it comes back with a result. Both are useful. Choosing the wrong one wastes time: either by burning an afternoon on something an agent could have drafted while you did other work, or by handing the agent a job it wasn't ready to own.

This pattern is about the choice. When to pair. When to dispatch. How to review agent output so the autonomy is earned, not rubber-stamped.

## The move

Default to the skill when any of these are true:

- The scope is small enough that writing the prompt takes almost as long as doing the move yourself.
- You are not sure what you want, and you expect to reshape the request as the work unfolds.
- The material has subtle judgment calls you want to see in real time, not discover in a briefing.
- You are teaching yourself the move and you want to see what the tool does on each turn.

Reach for the agent when any of these are true:

- The scope is large enough that a dialogic pass would run for an hour.
- You want a complete proposal to react to, not a draft to co-author.
- You have other work you could be doing during the run.
- The work has enough independent substructure that parallelism matters.

The architect agent produces a complete spec and ADR set for a project-scale design. The spec skill pairs with you to decide the design one section at a time. Both produce a spec. Which one you want depends on whether you are prepared to react to a draft or you want to author the draft.

## Why it works

Agents are expensive and opaque. They read more of the codebase, they consider more alternatives, they stress-test their own output before they hand it back. That takes time and context. You get a briefing, not a transcript, and you trust the briefing to accurately summarize the judgment calls made along the way. That trust is earned by good agent design, but it is still trust.

Skills are cheap and transparent. You see every proposal, you can redirect turn by turn, and the conversation shapes the output. The cost is your attention, and your attention is the constraint on how much work you can do in an afternoon.

The tradeoff is control versus parallelism. A skill gives you total control over every decision in exchange for your full attention. An agent gives you your attention back in exchange for partial control over the decisions made inside the run. Pick the axis that matches what the work needs.

## How to review agent output

Agent output is not a finished artifact. It is a candidate. Review it the way you would review a pull request from a trusted but unfamiliar colleague: assume the work is competent, look for the specific places where competence is not enough.

Four things to check when an agent briefing lands:

**The open questions.** A good agent comes back with judgment calls labeled as judgment calls. Read them first. These are the decisions the agent was not willing to make on its own, and they are where your input is highest-leverage.

**The alternatives considered.** For any non-trivial choice, the agent should show what it considered and rejected. If the rejected options feel straw-man, described in a way that makes the chosen option obviously better, the agent didn't think hard enough. Push back.

**The citations.** The grounded-in-material rule applies to agent output the same as it does to skill output. Claims about the codebase should reference specific files. Claims about discovery context should reference specific artifacts. If a claim is floating, ask where it came from.

**Your own gut.** If the briefing feels too clean, it probably is. Real design has awkwardness. Ask the agent to name the three things it felt least sure about. A good agent will answer honestly.

## Common failure mode

The common failure is rubber-stamping. The agent comes back with a complete spec, it looks coherent, you approve it, work begins, and two weeks later someone discovers that a load-bearing decision was wrong. The decision was visible in the spec, but it was in the middle of a long section, and you did not read that section carefully because everything around it was fine.

The defense is to read the agent's output against the actual work it is proposing, not against the question of whether it is well-written. A well-written spec that ships the wrong design is the worst failure mode Etak has. When you review an agent's output, you are the safety. If you are going to accept it, you must have actually read it.

A second failure is dispatching the agent without doing the pre-work. The developer agent works well on a ready story. It works badly on a story that is underspecified, has missing acceptance criteria, or is really three stories pretending to be one. The agent will try to complete it anyway and will produce something shaped like success but wrong in details you won't notice until the PR. Readiness-check the story before dispatching. The assess skill exists for this.

## A worked example

You have a validated idea that needs a spec. The scope is a new filtering capability on an existing list view. Three components, one new API endpoint, no migration. Medium-small.

You could dispatch the architect agent. It would read the codebase, propose two approaches, draft the spec and an ADR for the caching choice, and come back in eight minutes. You could use the spec skill interactively, which would walk you through the same sections but ask you to weigh in on each decision. Forty-five minutes, maybe.

You choose the skill. The reason: you are not sure whether the filter should live in the existing list component or in a new wrapping component, and the decision depends on conversations you want to have as you see the proposed code paths. The judgment is the work. Handing it to the agent would flatten the conversation into a briefing.

A different day, a different project: the notifications rewrite. Four subsystems, two migrations, a significant infrastructure choice. You dispatch architect. It runs for twelve minutes. The briefing comes back with a recommended approach, a rejected alternative explained clearly, two ADRs, and three open questions you have to resolve before implementation. You read the alternatives carefully, challenge one citation, resolve the open questions. Total time: thirty minutes of your attention. Total work: what would have been a four-hour pairing session.

You picked right both times because you matched the shape of the work to the shape of the collaboration.

## Related

- [Architect agent](../agents/architect.md): the canonical example of the autonomous shape.
- [Develop overview](../develop.md): all the skills and agents and how they relate.
- [Anti-patterns](anti-patterns.md): dispatching the developer agent without readiness-checking is the anti-pattern this page is the remedy for.
