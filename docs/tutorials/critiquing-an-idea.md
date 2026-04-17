# Critiquing an idea

**Time:** about thirty minutes. **Prerequisite:** [Getting started](getting-started.md) and an idea you care about defending.

Critique is the move where you step outside your own idea and look at it from perspectives you haven't considered. Done well, it surfaces blind spots, confirms what's strong, and leaves the idea sharper than you found it. Done poorly, it produces a wall of generic concerns nobody acts on.

This tutorial is about doing it well.

## Questions to sit with before you start

1. **When was the last time a colleague critiqued an idea of yours and you genuinely changed your mind?** What made that critique work? Specificity? The persona they adopted? The relationship? Can you reproduce the conditions deliberately?
2. **Do you tend to over-critique or under-critique?** Some PMs over-critique as a risk-management reflex; others under-critique because they've earned conviction and don't want to relitigate. Which are you, and what does it cost you?
3. **Whose perspective is missing from your normal decision-making?** Not just "users" in the abstract — specific roles, specific power relationships, specific kinds of skepticism you rarely have to answer to. What would they see in your current work?

## The idea we'll work with

Use one of your own. If you need a scenario, try this:

*An AI assistant sidebar in a project management tool that suggests next actions based on team activity patterns.*

Before you start, write two things:

- **One sentence on why this idea is good.** Your best defense.
- **One concern a thoughtful skeptic might raise.** Your best attack, before the tool helps.

## Step 1. Open the critique

Type:

```
Critique the AI assistant sidebar idea.
```

Etak moves into **critique**. The first question it asks is usually "whose perspective matters most here?" If you already know — say, a skeptical engineering lead, or a finance partner, or an end user from a specific segment — say so. If you don't, let the tool propose a set.

A well-framed critique session usually runs three to five rounds from different perspectives. Fewer and you miss things. More and you start repeating yourself.

## Step 2. Persona-based round

For the sidebar scenario, Etak might propose:

- **The skeptical user.** A mid-career PM who has tried three AI tools and turned them all off within a week. What does the sidebar look like to them?
- **The adjacent power user.** A team lead who already has their own way of tracking activity. Does the sidebar compete with their workflow?
- **The vulnerable user.** A new team member who doesn't know enough yet to judge whether the suggestions are good. Does the sidebar help them or confuse them?
- **The adversarial case.** A team that games the activity signals to make the sidebar say what they want. What does abuse look like?

Pick one. Etak will inhabit the persona — not caricature, inhabit — and walk through the idea. Findings come with severity: fatal (the idea cannot work as framed), serious (major risk), moderate (friction worth addressing), minor (polish).

The test of a good finding is specificity. "Users might not trust this" is not a finding. "The skeptical user will compare this to the three AI tools they already turned off and will look for the feature that prevents them from turning this one off in the same way. We don't have that feature defined." That is a finding.

Do two or three persona rounds.

## Step 3. Framework-based round

Now switch from personas to analytical lenses. Etak offers a menu:

- **Assumption audit.** Look at the assumptions under the idea. Are any of them load-bearing and unexamined?
- **Second-order effects.** If this idea succeeds, what happens? Does the success create a new problem? A new dependency?
- **Inversion.** If you wanted to sabotage this idea from inside the company, what would you do? What existing behaviors already do that by accident?
- **Competitive response.** If a direct competitor shipped this tomorrow, how would you respond? What does your response reveal about what the feature is actually worth?
- **Edge cases.** What happens at the margins — tiny teams, massive teams, teams that barely use the product?

Pick two. Each should produce a handful of specific findings.

For the sidebar, the inversion lens often produces the most useful findings. "How would we sabotage this? We'd let the suggestions become generic enough to feel like AI slop. We'd rank by recency instead of relevance. We'd fail to handle the cold-start case for new teams." Each of those is a specific thing to guard against.

## Step 4. Confirm what's strong

This is the step most teams skip and it's why critique sessions feel demoralizing.

Ask:

```
What held up well under this critique?
```

Etak will articulate the strengths that survived. For the sidebar, maybe:

- The core insight (team activity patterns contain signal about what's next) survived scrutiny from three perspectives.
- The decision to put it in a sidebar rather than interrupt the user was validated by the skeptical-user persona.
- The feature is differentiated from existing AI assistants in a way the competitive-response lens couldn't easily neutralize.

An idea that survives critique is stronger than an idea that hasn't been critiqued. Name what survived. That's the case you make in the next roadmap review.

## Step 5. Decide what the findings mean

Critique findings fall into three buckets. For each, a different next step.

**New assumptions.** A finding often reveals a belief you hadn't surfaced. "The skeptical user test assumes users give the tool a full week to prove itself" is a new assumption worth capturing. Move to [sound](../skills/sound.md) or just add it to the graph.

**Design refinements.** Some findings point at changes to the idea itself — adjusting scope, rethinking an interaction, handling an edge case explicitly. Update the idea's description. Note what changed and why.

**Fatal concerns.** If critique surfaces a genuinely fatal concern, the right move is not to build. Go back to [explore](../skills/explore.md) and generate alternative approaches from the same opportunity. This is an uncomfortable outcome and the whole point of running critique early.

## Step 6. Know when to stop

Etak tracks coverage. After three or four rounds, new rounds start producing findings that echo earlier ones. That is the signal to stop. Critique has diminishing returns, and a team that critiques forever is a team that ships nothing.

The goal is not to find every possible flaw. The goal is to find the flaws significant enough to change what you do next. When you have those, you're done.

## Step 7. Capture the session

```
Save the critique summary.
```

Etak writes a short artifact: which idea was critiqued, what perspectives were used, the top findings with severity ratings, what changed in the idea as a result. This becomes part of the idea's history. When someone asks in three months "did we think about this from a finance perspective?", the answer is in the graph.

## Go back to your original questions

- **The last time a critique genuinely changed your mind.** Was it because the critic inhabited a persona you don't normally hold? Or applied a lens you don't normally apply? Etak does both on demand. Use it.
- **Over-critique or under-critique?** Over-critiquers can use Etak's coverage tracking to stop early. Under-critiquers can use the persona menu to reach for the specific perspectives they tend to skip.
- **The missing perspective in your normal decision-making.** Name it. Add it as a standing persona in your critique sessions. The tool will inhabit whoever you tell it to.

## Common failure modes

- **Wall of generic concerns.** Push the tool for specificity. Severity ratings help. Generic findings get demoted to minor.
- **Critique without action.** Every finding should point at a concrete next step: new assumption, design change, or idea kill. If findings are piling up without changes, you're doing theater.
- **Critique as veto.** Critique is not permission-granting. A thoughtful PM who has seen the findings and decided to proceed is a thoughtful PM. The graph records the decision.
- **Critiquing ideas that haven't been sounded.** Critique is looking from outside. Sound is looking from inside. They complement each other. If critique keeps bumping into "this rests on belief X," go sound the belief first. See [sound](../skills/sound.md).

## What to try next

- Run critique on an idea someone else is pushing. The same move works on other people's ideas, and it often reveals whether their conviction is earned or inherited.
- [**The full discovery loop**](full-discovery-loop.md) — see critique in the context of the larger cycle.
