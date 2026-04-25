# Pushing back on the tool

The tool will propose things you do not believe. It will surface an assumption and claim your idea depends on it when it doesn't. It will mark evidence as "medium" when the source was one conversation three months ago. It will suggest a ranking that puts a low-stakes assumption at the top because the evidence column is empty. Some of this is the tool being wrong. Some of it is the tool noticing something you'd rather not.

Telling those apart is the work. The handshake between tool confidence and human judgment is the whole premise of Etak. It breaks when you rubber-stamp everything the tool produces, and it also breaks when you reflexively override whatever you disagree with. Neither is collaboration.

## The move

When the tool proposes something that feels wrong, do not accept and do not reject. Ask it to justify.

```
Where did the claim that researchers want a saved-papers list
come from? Cite your source.
```

Etak's rule is that proposals are grounded in material. When it surfaces an assumption, it can point at the idea text, a customer conversation note, a prior experiment, or another artifact it read to generate the claim. When it ranks by evidence, it can show which artifacts it scanned and which fields it weighed.

The citation is the seam. If the material genuinely supports the claim, your disagreement is about interpretation, and that is a productive conversation. If the material does not support the claim, meaning the conversation cited doesn't actually say what the tool thinks it says, the tool is hallucinating, and your disagreement is about accuracy. Different fix.

## Why it works

"Grounded in material" is not a politeness. It is the only way to separate two different kinds of tool failure. One is factual error: the tool read something wrong or invented a connection. The other is a real observation you were not ready to hear. Both feel the same in the moment. The citation tells you which you are dealing with.

When the material is thin, the tool should say so rather than fabricate. Ask directly: *how confident are you in this, given what you read?* A well-calibrated response looks like "the only source is one note from a customer interview two months ago, and the note doesn't use the exact framing I'm proposing; treat this as a weak hypothesis." That is useful. A response that repeats the claim with more adjectives is not.

If the tool pushes a claim it cannot ground, treat that as a signal about the claim, not about your authority. The tool is allowed to be wrong. You are allowed to override it. Say what you are doing and why, and the override lands in the artifact.

## Common failure mode

Two symmetrical failures.

The first is reflexive agreement. The tool surfaces an assumption and you accept it because you assume the tool read something you forgot. This produces a graph that looks well-grounded but actually reflects the tool's guesses layered over your unexamined agreement. The beliefs are nobody's.

The second is reflexive rejection. The tool surfaces an assumption that makes you uncomfortable and you override it because it doesn't match your intuition. Sometimes your intuition is right. Sometimes the assumption is uncomfortable because it is load-bearing and you hadn't been looking at it. The citation check forces the question: is this wrong, or is this something I didn't want to see?

## A worked example

You've drafted an idea to let researchers comment on papers they've saved. You ask the tool to surface assumptions. It proposes, among others: *researchers want their comments visible to other researchers in their institution.*

Your gut says this is wrong. Nobody asked for that. You are about to override. Instead:

```
Where does the institution-visibility assumption come from?
```

The tool says: "From the objective text, which mentions collaboration as part of the engagement outcome. I inferred that commenting was serving the collaboration branch of that objective."

You look at the objective. It does mention collaboration, but as one of three supporting themes, and the idea you're working on is aimed at the other two. The tool made a defensible inference from real material, but the inference is wrong for this idea. You tell it so, and it updates. The assumption gets removed. The conversation took ninety seconds and the graph is now more accurate.

A different day, a different idea, the citation comes back and you realize the material does say what the tool claims. You were going to override because the assumption felt inconvenient. Now you cannot. You keep it and mark the evidence as weak. You have learned something about your own bias. That is the handshake working.

## Related

- [Sound skill](../skills/sound.md): where assumption surfacing happens, and where pushback is most common.
- [Foundations](../context/foundations.md): the mixed-initiative section explains the design principle behind proposal and override.
- [Anti-patterns](anti-patterns.md): rubber-stamping agent output is the anti-pattern this page is the remedy for.
