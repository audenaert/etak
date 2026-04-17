# Getting started

**Time:** about twenty minutes. **Prerequisite:** Etak installed ([root README](../../README.md)).

You will walk one idea through the full discovery loop at shallow depth. By the end you will have an objective, an opportunity, an idea, three assumptions, and an experiment, all linked together in `docs/discovery/`. You will also have a working mental model of the five moves Etak supports.

## Questions to sit with before you start

Spend two minutes on each. Write your answers down somewhere.

1. **What is the last thing you shipped that nobody used?** What did you believe when you started building that turned out not to be true? When could you have found out?
2. **How do you currently tell a stakeholder no?** Is there a shared artifact you both point at, or does it come down to your authority in the moment?
3. **If a new team member asked "what are we actually betting on right now?", what would you show them?** A roadmap? A slide? A set of tickets? How quickly would they understand it?

Keep these in mind. The tool will put pressure on all three.

## Scenario

You're the PM for a research platform used by academics. You've had three customer conversations in the last two weeks where people mentioned, in different words, that they lose track of interesting papers. You don't have a clear theory yet. You just have a signal.

Open Claude Code in a project where you're comfortable creating a `docs/discovery/` directory. The examples below use a research platform, but any scenario works.

## Step 1. Arrive with a half-formed thought

Type something like:

```
I've been hearing from researchers that they lose track of
interesting papers. I don't know if it's a real opportunity or
just a symptom of something else. Help me think about it.
```

Etak is in **orient** mode now. It is receptive, not generative. It will ask clarifying questions: which researchers, what exactly they said, whether you've heard any version of this before. Answer honestly. The tool is better at proposing structure when the input is specific.

At some point Etak will say something like: "this is starting to sound like an opportunity under an objective about deeper engagement with the platform. Want to explore that?" Say yes.

**What just happened.** Orient is the lowest-formalism skill. It exists so the tool has somewhere to meet you when you don't yet have a clean request. See [orient](../skills/orient.md) for more.

## Step 2. Name the objective and the opportunity

Etak will propose a draft objective and a draft opportunity. Something like:

- **Objective:** increase depth of engagement for active researchers.
- **Opportunity:** *how might we* help researchers keep track of works they want to return to?

Read the drafts. If the wording feels off, say so. This is the moment the team's language gets written down. Getting it right is worth thirty seconds.

When you're happy with the drafts, say "save them." Etak writes them to `docs/discovery/objectives/` and `docs/discovery/opportunities/`.

**What just happened.** You went from a signal to two linked artifacts. The opportunity is framed as *how might we* on purpose — it keeps the space open. "Build a bookmarking feature" would already be an idea. See [opportunity](../artifacts/opportunity.md) for why the framing matters.

## Step 3. Propose an idea

Now go bottom-up. Say:

```
I have an idea: let researchers bookmark papers with one click
and review them later in a dedicated list.
```

Etak captures the idea and links it to the opportunity you just created. If you already have a different opportunity that this idea could also address, Etak will flag the link. An idea can serve more than one opportunity.

Ask Etak to show you the current graph:

```
Show me the tree.
```

You should see objective → opportunity → idea, all connected.

**What just happened.** You gave a solution-first input and the tool still placed it correctly. Most product conversations start with solutions. Etak is built to accept that and trace upward instead of demanding you start at the top.

## Step 4. Surface what the idea rests on

Say:

```
What are we assuming?
```

Etak moves into **sound** mode. It will propose a set of assumptions — probably between four and eight — in three or four categories:

- **Desirability.** Researchers want a place to return to for papers they've seen.
- **Usability.** One click is actually the right interaction, not two.
- **Feasibility.** The list stays useful past twenty entries.
- **Viability.** The behavior correlates with the engagement metric on the objective.

For each, Etak asks you to rate importance (if wrong, how bad?) and evidence (what do we actually know?). Be honest. "We had three conversations" is low evidence, not medium.

Save the assumptions when you're happy with them.

**What just happened.** You externalized the beliefs your idea is running on. Most of them you already held. Some you didn't realize you held until you saw them stated. That moment — "oh, I didn't know I was assuming that" — is the core thing sounding produces. See [sound](../skills/sound.md).

## Step 5. Find the one that matters

Say:

```
Which of these should we test first?
```

Etak moves into **prioritize**. It ranks by risk (high importance, low evidence) and recommends a single assumption to test first. The top of the list is often not the one you would have guessed.

Notice what the tool is not doing. It is not deciding. You can push back — "actually viability matters more than I marked it, here's why" — and the ranking updates. Prioritization is a conversation. Etak brings rigor to the evidence side. You bring judgment about context. See [prioritize](../skills/prioritize.md).

## Step 6. Design the cheapest test

Say:

```
Design an experiment for the top assumption.
```

Etak will pick a method, draft success criteria, and force you to pre-commit to three action plans: what you'll do if the assumption is validated, invalidated, or inconclusive.

Pre-commitment is the feature here. When the results come in, you will not be negotiating with yourself about what they mean. You already decided. If the results are genuinely ambiguous, the experiment tells you that too, and the pre-committed action is to run a sharper one. See [experiment](../skills/experiment.md).

Save the experiment.

## Step 7. Look at what you built

```
Show me the tree.
```

You should now see a connected graph: objective at the top, opportunity below, idea below that, assumptions under the idea, and an experiment attached to one assumption. All in `docs/discovery/`. All version-controllable. All readable without Etak if you ever decide to walk away.

Open one of the markdown files in a text editor. Look at the YAML header. This is the whole data model. No hidden state, no database. The artifact is the artifact.

## Go back to your original questions

The three questions from the top of this page. Answer them again now, in the language of the graph you just built.

- **What are we actually betting on?** Point at the assumptions. The top two are the answer.
- **How do I tell a stakeholder no?** Not on feature X — on the assumption under feature X that nobody has tested. The conversation is different.
- **What would a new team member see?** An objective, a tree of opportunities, a set of ideas, and a log of what's been tested. Five minutes of reading time.

That is the delta Etak is trying to produce.

## What to try next

- The [**full discovery loop**](full-discovery-loop.md) tutorial takes this much further, through critique, experiment execution, and propagating results back into the graph.
- The [**surfacing assumptions**](surfacing-assumptions.md) tutorial goes deep on step 4 when the beliefs are stubbornly invisible.
- Try the flow above on a real initiative you have half-formed opinions about. That is when the tool starts to pay for itself.
