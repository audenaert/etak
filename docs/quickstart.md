# Quick start

**For:** a product manager who just installed Etak and has about five minutes before the next meeting.

**Question this page answers:** how does Etak help my team ship the right things faster?

## The short answer

Teams ship the wrong things for four boring reasons: the opportunity was vague, the idea rested on beliefs nobody tested, the team optimized for output instead of outcome, and the learning never made it back into the decision. Etak is a thinking partner that attacks those four things in the order that matters.

You don't stop to "do discovery." You keep doing your job. Etak captures what you're already producing, forces the next useful question, and leaves a trail of artifacts the next person can read.

## What it gives you

A living **opportunity graph** in `docs/discovery/` with seven kinds of artifact:

- [**Objectives**](artifacts/objective.md) — business outcomes you are on the hook for.
- [**Opportunities**](artifacts/opportunity.md) — customer needs framed as *how might we* questions.
- [**Ideas**](artifacts/idea.md) — proposed solutions.
- [**Assumptions**](artifacts/assumption.md) — the beliefs an idea is resting on.
- [**Experiments**](artifacts/experiment.md) — tests that turn beliefs into evidence.
- [**Critiques**](artifacts/critique.md) — structured examination rounds that stress-test an idea or opportunity from outside perspectives.
- [**Memos**](artifacts/memo.md) — in-depth analytical documents (frameworks, surveys, research syntheses) that inform the opportunity space without being a gap to fill or a solution to build.

Every artifact is a markdown file with a YAML header. You can read them, edit them, grep them, put them in git. Etak uses them to answer questions like "what are we betting on?" and "what should we work on next?"

## The five minutes

Open a Claude Code session in the repo where the product work lives. Then run these four moves.

### 1. Drop an idea in (30 seconds)

```
I have an idea: let researchers bookmark papers with one click.
```

Etak captures the idea and asks what problem it solves. Trace it upward until you have an opportunity (the customer need) and an objective (the business outcome). If either already exists, Etak links to it. If not, it proposes one.

You did this conversationally. Etak did the filing.

### 2. Ask what it rests on (90 seconds)

```
What are we assuming here?
```

Etak walks the idea and surfaces the beliefs underneath it: desirability ("researchers want to save papers"), feasibility ("we can integrate with Zotero"), viability ("saves increase weekly active use"). Each assumption gets an **importance** rating (if wrong, how bad?) and an **evidence** rating (what do we actually know?).

The point isn't the ratings. The point is you can now see, in one view, the three beliefs that are high-importance and low-evidence. Those are the ones worth your attention.

### 3. Ask what to test first (30 seconds)

```
Prioritize these assumptions.
```

Etak ranks them by risk. The top of the list is the belief you'd most regret being wrong about. It's often not the one you thought.

### 4. Design the cheapest test that would settle it (2 minutes)

```
Design an experiment for the top one.
```

Etak picks a method (user interview, prototype test, fake door, data pull, survey), writes success criteria *before* the test runs, and forces you to pre-commit to what you'll do with each outcome: validated, invalidated, inconclusive. Pre-commitment is the feature. It prevents the results meeting from turning into a negotiation.

Hand the experiment to whoever is going to run it. You got from a hunch to a falsifiable test in five minutes.

## Why this makes your team faster

Faster shipping is a second-order effect of three things Etak is designed to produce:

**Fewer dead-end builds.** A two-hour assumption test is always cheaper than a two-sprint feature nobody uses. You find out earlier.

**Less roadmap defense.** When a stakeholder pushes a feature, you have somewhere to put it. It goes in the graph as an idea, with assumptions attached. The conversation stops being "yes or no" and starts being "what would we need to believe?" That is a much shorter conversation.

**Learning that compounds.** Every experiment, every invalidated assumption, every shelved idea stays in the graph. When the next initiative looks like one you already tried, you can see why it failed last time without having to remember.

None of this is speed for its own sake. It is speed at the part of the work that matters: getting to the next decision with better evidence than you had yesterday.

## Where to go from here

- **Try the [getting-started tutorial](tutorials/getting-started.md).** About twenty minutes. Builds a graph around a realistic scenario.
- **Read [about the name](context/about-the-name.md)** if you want to understand why the tool is shaped the way it is. It's short and it matters.
- **Skim the [skills reference](skills/)** to see the full menu. You never need to invoke skills by name, but knowing what's there lets you recognize the move when it's the right one.

## A warning

Etak rewards honesty. An assumption marked "high evidence" when you actually have a hunch produces a graph that lies to you. A pre-committed action plan you ignore when results come in is worse than no plan. The tool is only as good as the candor you bring to it.

That's the trade. Bring candor, get leverage.
