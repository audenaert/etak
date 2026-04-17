# Foundations

**For:** readers who want to understand why Etak works the way it does. **Length:** about fifteen minutes. **Required?** No. The [quick start](../quickstart.md) and [tutorials](../tutorials/) stand on their own. But if you've used discovery tools that felt subtly wrong and you want to know what Etak is doing differently, this is the longer answer.

Etak is not a new idea. It is an old set of ideas made newly possible.

This document explains the conceptual ground the product stands on. It is written for people who want to understand why Etak works the way it does, not for researchers. Where we name academic sources, we do it so you can trace the lineage yourself if you want to go deeper. None of the arguments below require you to read the citations to make sense.

**On this page.** The basic claim · reflection and the reflective conversation · incremental formalism · mixed-initiative interaction · why the ideas are newly actionable · continuous discovery and the Opportunity Solution Tree · where Etak departs · the graph is a model, not a map · what this means in practice · where to go next.

## The basic claim

Product discovery is a creative process. Not a data-analysis process. Not a requirements-gathering process. Not a prioritization exercise.

Most tools in this category treat discovery as documentation: a place to write down opportunities, rank them, and move on. That gets the work backwards. The real work of discovery is figuring out what the opportunities actually are, which ones connect to which, which of your beliefs about them are load-bearing, and which of those beliefs might be wrong. The artifacts are outputs. The thinking is the work.

Etak is built to support the thinking. Everything else follows.

## Reflection and the reflective conversation

In 1983, Donald Schön published *The Reflective Practitioner*. His argument was that expertise in any serious creative discipline (architecture, design, engineering, medicine, teaching) does not work the way we tend to describe it. Professionals do not apply general theory to specific cases and read off the answer. They hold a conversation with the situation. They make a move, they see how the situation talks back, they adjust.

Schön gave us two terms that are useful here. **Reflection-in-action** is the thinking that happens while you are in the middle of the work: you try something, you notice it didn't quite work, you adjust on the fly. **Reflection-on-action** is the thinking that happens afterward: you step back, you examine what you did, you take in what you learned, you adjust for next time.

Both require something to reflect against. You cannot reflect on a blank page. You reflect on an artifact, a sketch, a model, a draft. The artifact talks back to you. Schön called this "back-talk of the materials." You see things in your own sketch that you did not consciously put there. The externalized version shows you what you actually think, and sometimes it is different from what you thought you thought.

Product teams need this. A loose opportunity map pinned on the wall shows you something you cannot see in your head: which ideas cluster, which assumptions are doing too much work, which ideas have no experiment behind them. The artifact is the material. The reflection is the work.

## Incremental formalism

Here is the tension at the heart of any tool for creative thinking.

Structure is useful. Without it, everything is a pile of notes and nothing connects. But structure committed too early is worse than no structure at all. Force a team to decide whether an idea is an "opportunity" or a "solution" in the first five minutes of a conversation and they will either pick wrong or stop thinking.

The research literature calls this **incremental formalism**: the principle that a good tool for creative work lets you start informal and get more formal as your understanding clarifies. You should be able to jot a half-formed observation, leave it loose, come back to it later, connect it to something else, promote it to a typed artifact when you know enough to type it, and demote it again if you were wrong.

Frank Shipman and Catherine Marshall made the definitive argument for this idea in their 1999 paper "Formality Considered Harmful." Their claim, developed across years of building experimental hypertext systems at Xerox PARC, was that tools which demand formal structure up front do not just slow users down. They cause users to reject the tool entirely, or to produce badly structured artifacts, because the cognitive cost of formalizing an idea you have not yet finished thinking is higher than the value the formalism provides. The answer they proposed was not less structure. It was structure that gets introduced gradually, at the pace of the user's own thinking, ideally with the system helping to recognize what is emerging and suggest formalizations the user can accept, reject, or revise.

This idea has been around for decades in various forms. It has been hard to implement well because you need the tool to recognize what is emerging in your informal arrangements and help you formalize what is worth formalizing. Until recently, that recognition had to come from the human. The tool just held the notes.

Large language models change this. An AI can read a loose set of observations and notice that three of them look like variations on the same opportunity. It can spot an assumption hiding inside an idea. It can suggest the cheapest experiment that would tell you whether that assumption is right. Most importantly, it can do this as a proposal, not a decision. You stay in charge. The tool does the mechanical work of recognizing structure. You do the judgment work of accepting, rejecting, or reshaping what it proposes.

That is the kind of collaboration Etak is built around.

## Mixed-initiative interaction

The model for this collaboration is **mixed-initiative interaction**, a phrase from a 1999 paper by Eric Horvitz. The idea is simple and underused. In most software, the human takes all the initiative. You click, the computer responds. The computer never proposes anything. It never says, "I notice you are about to do something expensive, would you like me to suggest an alternative?"

A mixed-initiative system does. Both the human and the system can take initiative. Both can propose actions, raise objections, and ask questions. The system handles the mechanical parts of the work. The human holds authority over the judgment calls. Neither side is passive.

Ben Shneiderman extended this into a design principle he calls **human-centered AI**: the goal is not to choose between high automation and high human control, but to achieve both at the same time. A tool that automates everything takes the thinking away from you. A tool that automates nothing leaves you doing work a machine could do better. The interesting design problem is to give you both: a collaborator that carries its share of the load while leaving you firmly in the driver's seat.

Etak's AI is designed on this model. It proposes. It notices. It flags. It drafts. It never commits your thinking for you. When it formalizes something, it shows you what it did and lets you accept, reject, or revise. The collaboration is the product.

## Why the ideas are newly actionable

These ideas are not new. Schön wrote *The Reflective Practitioner* in 1983. Horvitz's mixed-initiative paper is from 1999. Incremental formalism has been discussed in the research literature for over thirty years. For most of that time, the ideas have been easier to describe than to build. The tools did not have the pattern-recognition capacity to hold up their end of the conversation. They could store your notes. They could not read them.

Large language models close this gap. A modern LLM can read a page of half-formed observations and return a reasonable structural interpretation. It can notice that three ideas are variations on one theme. It can extract an unstated assumption from a paragraph of reasoning. It can look across a set of experiments and flag the one whose results contradict an assumption you made two months ago.

None of this is magic. It is pattern recognition at a level that was not previously available in software. The consequence is that a whole generation of research ideas about how to support creative work (ideas that were clearly correct but frustratingly difficult to implement) now have the technical substrate they always needed.

Etak is, in essence, a bet that the right thing to build with this new capability is a tool that takes those research ideas seriously. Not a faster ticket tracker. Not a better whiteboard. A creativity support system in the precise sense the research community has meant that phrase for three decades.

## Continuous Discovery and the Opportunity Solution Tree

Teresa Torres's *Continuous Discovery Habits* is the most influential practitioner framework in product discovery. Her core argument is that discovery should be continuous (not a phase you do at the start of a project), that it should be hypothesis-driven (not "let's see what the data says"), and that it should be organized around a structured artifact she calls the **Opportunity Solution Tree**.

The tree links things together in a specific way. At the top is an **objective**: the outcome the team is responsible for. Below the objective are **opportunities**: customer needs and pain points the team has surfaced through research. Below each opportunity are **ideas**: candidate solutions the team might pursue. Below each idea are **assumptions** that would have to be true for the idea to work. The assumptions are what get tested through **experiments**.

This is a good model. It makes the team's thinking explicit. It forces them to trace each proposed solution back to a customer problem worth solving. It treats ideas as hypotheses rather than as commitments. Etak takes it seriously enough to build its discovery vocabulary around these exact artifact types.

## Where Etak departs

Torres's framework presents the structure as a tree. That is a useful diagram. It is not, in our view, a useful data model.

A tree has one root. Each node has exactly one parent. Every relationship is hierarchical. If you draw your discovery as a tree, you are forced to assign each opportunity to exactly one objective, each idea to exactly one opportunity, each assumption to exactly one idea. The real world does not behave this way. A single opportunity often bears on two or three objectives. An assumption is frequently shared by several ideas across different branches. An experiment can inform multiple assumptions at once. Forcing this reality into a tree means either duplicating nodes, losing connections, or making arbitrary parenting choices that distort what you actually believe.

Etak models the opportunity space as a **graph** instead. Nodes are typed (objective, opportunity, idea, assumption, experiment). Edges are typed (supports, addresses, assumed-by, tests). A single assumption can be linked to every idea that depends on it. A single experiment can be linked to every assumption it informs. The shape of the thinking is preserved rather than flattened.

This matters for a second reason. A graph handles **incompleteness** gracefully. A tree, visually, looks wrong when branches are missing. You want to fill in the gaps. A graph does not carry that aesthetic pressure. It can be sparse in some regions and dense in others, and that is fine, because that is what real discovery looks like.

## The graph is a model, not a map

The most important thing to understand about Etak's opportunity graph: it is an **evolving model of your team's current understanding**. Not a specification of reality. Not a comprehensive map of the market. Not a complete catalog of every need every customer has ever expressed.

The graph is messy on purpose. It has missing links. It has ideas with no assumptions under them yet because you have not thought hard enough about why they might fail. It has assumptions that have never been tested. It has opportunities that were promising six months ago and have quietly gone stale. This is not a bug. This is an accurate reflection of the state of the team's thinking at any given moment.

Traditional PM tools push you toward a false sense of completeness. Every field must be filled in. Every ticket must have a status. The state of the board is supposed to match the state of reality. Etak is built on a different premise: your understanding is always partial, always evolving, and always a few steps behind the world. The tool should help you see that clearly, not paper over it.

The practical consequence is a posture we take seriously: **ship value now rather than strive for a complete view of the world**. Discovery is not the process of waiting until the graph is "done" before building anything. That day never comes. Discovery is the process of knowing enough, about the right parts of the space, to make the next move with confidence. Etak's job is to help you find those parts, understand them well, and get to the next move quickly. Not to make the graph perfect.

## What this means in practice

The ideas above are not decorative. They show up in specific product decisions:

The discovery skill surfaces the most valuable next action based on the current state of the graph (stale experiments, high-risk untested assumptions, validated ideas not yet scoped), which is a mixed-initiative behavior in the Horvitz sense.

The graph accepts informal nodes and promotes them to typed artifacts as the team's understanding clarifies, which is incremental formalism in the Shipman sense.

Every validated experiment, rejected idea, and superseded assumption stays in the graph as part of the team's history. When a new initiative starts looking like one the team already tried, the graph has the material to let you reflect on what happened last time. That is reflection-on-action with real artifacts behind it.

The AI proposes structure, points out gaps, and drafts formalizations. The human accepts, rejects, or revises. The division of labor is deliberate.

## Where to go next

If the ideas above are new to you and you want to go deeper, five sources are worth the time:

Teresa Torres, [*Continuous Discovery Habits*](https://www.amazon.com/Continuous-Discovery-Habits-Discover-Products/dp/1736633309) (2021). The practitioner text for how to run the discovery work. Read it even if you already run OSTs in some form.

Donald Schön, [*The Reflective Practitioner*](https://www.amazon.com/Reflective-Practitioner-Professionals-Think-Action/dp/0465068782) (1983). The foundational argument for why creative work is a conversation with the situation rather than an application of general theory. Dense but rewarding.

Eric Horvitz, ["Principles of Mixed-Initiative User Interfaces"](https://erichorvitz.com/chi99horvitz.pdf) (1999). Short, readable, and still the clearest statement of the design principles Etak's AI is built on.

Frank Shipman and Catherine Marshall, ["Formality Considered Harmful"](https://people.engr.tamu.edu/shipman/formality-paper/harmful.html) (1999). One of the great paper titles in the HCI literature, and a better-than-most guide to why the tools we have tend to fail at supporting the messy middle of creative work.

Ben Shneiderman, [*Human-Centered AI*](https://www.amazon.com/Human-Centered-AI-Ben-Shneiderman/dp/0192845292) (2022). The modern synthesis. High automation and high human control together, which is exactly what a tool like Etak is trying to achieve.

None of these will teach you how to use Etak. They will teach you why it is built the way it is.

## Related

- [**About the name**](about-the-name.md) — the metaphor that gives the tool its orientation. Shorter than this page, and the better place to start.
- [**Quick start**](../quickstart.md) — what these ideas look like in five minutes of practice.
- [**Skills reference**](../skills/) — how the foundations show up in each skill.
- [**Artifacts reference**](../artifacts/) — the typed graph the incremental-formalism argument justifies.
