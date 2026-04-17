# Etak Documentation Voice Guidelines

A reference for generating documentation that sounds like Etak. These guidelines apply to README files, skill and agent descriptions, inline help, CLI output, and any other developer-facing text in this repository.

## Who we're writing for

The reader is a technical product leader: a PM with engineering instincts, or an engineering leader with product sensibility. They have already chosen to install a Claude Code plugin. They don't need to be sold on AI-assisted development. They need to know what this tool does, how it's different, and whether it's worth the five minutes it takes to try.

Assume intelligence. Don't over-explain. Don't hedge. If something is opinionated, say so directly rather than softening it into a suggestion.

## The core voice

Etak's voice is **quiet, declarative, and specific**. It carries conviction without performing it. It reaches for the concrete before the abstract. It trusts the reader to do some of the work.

Three qualities to hold together:

**Declarative, not tentative.** Write claims cleanly. "This skill builds the opportunity space," not "This skill is designed to help you potentially build out your opportunity space." Avoid "might," "may," and "can help you" when a direct verb is available.

**Concrete over abstract.** Name the actual artifact, the actual command, the actual output. "Creates an opportunity node with typed edges to the parent objective" beats "helps structure your thinking about opportunities." When in doubt, reach for the noun.

**Measured, not breathless.** No exclamation points. No "✨ magical ✨." No "supercharge your workflow." The product is serious and the documentation should match. A reader who wants enthusiasm can generate it themselves.

## Structure and rhythm

**Short paragraphs.** Two to four sentences. Occasional one-liners for emphasis. Avoid the flowing essayistic paragraph; it signals the wrong register.

**Headers over prose walls.** Use `##` and `###` liberally. A reader scanning should find what they need without reading every word.

**Lists when lists are honest.** Bullet points for genuinely enumerable things: arguments, flags, steps. Prose for reasoning and explanation. Don't bullet things that want to be sentences.

**Varied sentence length.** Not all short, not all long. A three-word sentence lands harder when it follows a twenty-word one.

## Specific conventions

**Active voice.** The subject does the thing. "The skill creates a markdown file" beats "A markdown file is created by the skill." "Etak stores artifacts in Neo4j" beats "Artifacts are stored in Neo4j." Passive voice hides the actor and adds words. Use it only when the actor is genuinely unknown or irrelevant, which is rare in product documentation.

Watch especially for:

- "is designed to," "is intended to," "is meant to": replace with the verb itself
- "can be used to": replace with "use it to" or just the verb
- "will be created," "is stored," "gets processed": name who or what is doing the creating, storing, processing

**Em-dashes: at most one per page, and often zero.** Em-dashes are a crutch. Most places that reach for one can use a comma, a colon, a period, or parentheses. Before using an em-dash, try rewriting the sentence without it. If the rewrite is worse, keep the dash. If the rewrite is the same or better, keep the rewrite.

**No emojis in documentation prose.** Status indicators (✓, ✗, ⚠) are fine in CLI output where they earn their keep. Decorative emojis in READMEs and skill descriptions are not.

**Use "you" for the reader, not "the user."** Documentation is a conversation. "You'll get a markdown file" beats "The user receives a markdown file."

**Name the thing.** Use the terms from the domain model: opportunity, assumption, experiment, idea, objective. Avoid generic substitutes like "item" or "entry." The vocabulary is part of the product.

**Skill and agent descriptions lead with the verb.** "Build and explore an opportunity space using the OST framework," not "A skill for building and exploring." The reader wants to know what happens when they invoke it.

## What to avoid

**Marketing voice.** No "seamlessly," "powerful," "cutting-edge," "revolutionary," "supercharge," "unlock," "elevate," "empower." These words do no work.

**Defensive hedging.** "We believe," "we think," "it's generally considered." Cut them. If it's in the documentation, we're asserting it.

**Vague abstractions.** "Workflow," "process," and "experience" used without a specific referent. If the sentence still works when you delete the abstract noun, delete it.

**False modesty.** "This might be useful for": if we didn't think it was useful, we wouldn't have built it. State the use case directly.

**Over-explained metaphors.** The Etak name carries navigation connotations naturally. Don't force the metaphor into every paragraph. One well-placed reference does more work than five weak ones.

**Redundant framing.** "In this section we will discuss…" Just discuss it. The header already announced the topic.

## Examples

**Too soft:**
> The discovery skill can help teams explore their opportunity space by potentially identifying opportunities they may have missed.

**Right register:**
> The discovery skill builds your opportunity space as a typed graph. It proposes opportunities you haven't captured, surfaces assumptions hidden inside ideas, and flags stale experiments worth revisiting.

---

**Too breathless:**
> ✨ Etak's amazing OST framework supercharges your product discovery workflow! Unlock the power of structured thinking today! 🚀

**Right register:**
> Etak models discovery as an Opportunity Solution Tree. Objectives branch into opportunities, opportunities into ideas, ideas into assumptions, assumptions into experiments. Each node is typed. Each edge carries meaning. The structure is what makes the AI useful.

---

**Too abstract:**
> This system provides capabilities for managing your product development process end-to-end through an integrated experience.

**Right register:**
> Etak runs the full loop, from customer signal through opportunity, idea, assumption, and experiment to scoped work. Every artifact links to the one before it and the one after. Nothing gets lost.

---

**Too long:**
> The `/discovery` skill is an interactive collaboration tool that enables you to work with AI to build, explore, and critique your opportunity space using Teresa Torres's continuous discovery framework. It supports brainstorming, assumption surfacing, experiment design, and multi-persona critique.

**Right register:**
> `/discovery` builds and explores an opportunity space. Brainstorm opportunities, surface hidden assumptions, design experiments, critique from multiple personas, prioritize by risk. The skill suggests the most valuable next action based on the current state of the graph.

---

**Passive voice:**
> Artifacts are stored as markdown files with YAML frontmatter, and version control is provided by git.

**Active voice:**
> Etak stores artifacts as markdown files with YAML frontmatter. Git handles version control.

## When to break these rules

Good documentation has texture. If every sentence follows the same pattern, the prose flattens. A longer, more reflective passage is fine when it earns its place: an introductory section explaining why the product exists, a design note about a non-obvious decision, a prose description that reads better than a list would.

The rules above are defaults, not laws. The goal is a voice that sounds like one serious person writing to another, not a content mill. If a sentence reads naturally and honestly, keep it.

## Quick reference

Before committing a documentation change, check:

- Does every sentence make a claim or do work? Cut the ones that don't.
- Is anything hedged that shouldn't be? Strengthen it.
- Are abstractions hiding what would be clearer as specifics? Replace them.
- Is the subject of each sentence doing the verb? If not, rewrite to active voice.
- How many em-dashes does the page have? Reduce to one or zero.
- Does the register feel earnest and quiet, or does it push too hard? Pull it back.
- Would the reader recognize this as a real product written by someone who cares, or as generic AI output? If the latter, rewrite.
