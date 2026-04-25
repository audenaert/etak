# docs

**Stance:** receptive technical writer. **Purpose:** keep documentation honest — matching what the code actually does, not what someone intended at design time.

Docs is the move where someone reads the code first, reads the docs second, and notices the seam. Drift is what lives at that seam — pages that describe an older intent rather than the current reality. Done well, docs catches drift early and updates pages in the user's frame. Done poorly, it produces wall-of-API reference that lists every method without telling the user when to reach for which.

This skill is the collaborative version. The autonomous counterpart is the [tech-writer agent](../agents/tech-writer.md), which does drift correction across whole doc sets autonomously.

## When to reach for it

- **A feature shipped without docs.** Docs writes the new pages, in the user's frame, in the order they need.
- **A doc page describes the old behavior.** Names changed, response shapes changed, defaults changed — and the page didn't keep up.
- **You're auditing a doc set.** Walk the pages and check each against the code. Some match, some drift, some topics that should exist don't.
- **A release is going out.** What's the user-visible change? Where do the docs say something different from what the code now does?

Trigger phrases: *update docs*, *docs audit*, *check documentation*, *sync docs*, *are docs up to date*, *document this*, *doc drift*, *the docs are wrong*.

## How it behaves

Docs reads the code first — actual function signatures, actual response shapes, actual flags — and then reads the docs that describe it. Drift is what it finds at the seam. It writes in the user's frame: what they're trying to do, what they need to know, in the order they need it.

The sharpest move docs makes is noticing when documentation describes intent rather than reality. A page that says "the API supports pagination" when the code only paginates one endpoint is documentation that misleads. Match the docs to the code, not the older intent.

### The three moves

**Audit.** Walk a doc page or doc set and check it against the code. Names (function names, endpoint paths, flag names, env vars, option keys), shapes (request/response shapes), behavior (errors, defaults, special values), coverage (code paths that should be documented but aren't), links (cross-links resolve, files haven't moved). Output a structured report: pages that match, pages with drift, pages or topics that should exist.

**Generate.** When a feature ships without documentation, write new docs. Read the spec and the code — the spec gives intent, the code gives reality, the doc should match the code. Write in the user's frame, not the API surface. Pick the structure that matches the doc type: API reference (name, signature, parameters, return, errors, example), how-to (scenario, prerequisites, steps, verification), concept (model, key terms, how it fits), tutorial (scenario, runnable steps, expected outcome).

**Update.** When drift is found, update the page. Match the code (if docs and code disagree, code is authoritative — update docs unless the code is the bug). Preserve the user's frame (don't restructure as a side effect of a fix). Walk the neighbors (a change to documented shape often affects an adjacent how-to or example).

## How to use it well

**Read the code first, every time.** The docs describe what the code did when the page was written. Now read what the code does today. The seam is the work.

**Write in the user's frame.** What are they trying to do? In the order they need it? The doc is not a transcription of the API surface — it's a path through the API surface for the task at hand.

**When code and docs disagree, code wins (usually).** Update the docs unless the code is the bug. If the code is the bug, surface that — don't silently update the docs to hide it.

**Walk the neighbors.** A change to a documented shape often affects an adjacent how-to or example. The audit finds the seam; updating one page often touches three.

**Don't generate from comments.** That's a build tool's job. Docs handles the human-curated layer that sits above auto-generation.

## What docs does NOT do

- **Write specs.** Specs describe intent before implementation. That's [`etak-develop /spec`](../skills/spec.md). Docs describes what the code does after implementation.
- **Generate API reference from inline comments.** That's a build tool's job. Docs handles the human-curated layer.
- **Maintain a marketing site.** Docs are technical documentation. Marketing copy lives elsewhere.
- **Translate.** Localization is a separate workflow.

## Common failure modes

- **Doc drift normalization.** "The doc was already wrong, so I left it." Cause: scope avoidance. Fix: if you touched the code, you own the doc page that describes it.
- **Intent-over-reality.** Updating docs to describe what the system *should* do rather than what it does. Cause: the spec was authoritative once. Fix: docs match code; if code is wrong, fix code.
- **Wall-of-API.** API reference that lists every method without telling the user when to reach for which. Cause: auto-generation without curation. Fix: write the path-through-the-API for the actual task.
- **Stale tutorials.** Tutorial steps that no longer work because the CLI flags changed. Cause: tutorials weren't checked when the flags moved. Fix: run the tutorial against the current code at audit time.
- **Doc-as-code-comment.** Docs that explain implementation details instead of user-visible behavior. Cause: writing for the engineer who built it. Fix: write for the user; implementation details live in code comments and ADRs.

## Transitions

- Drift reveals a missing spec → [**etak-develop /spec**](../skills/spec.md) ("The behavior here was never specified. Want to write the spec while we're documenting?")
- Drift reveals a code bug → [**etak-develop /build**](../skills/build.md) ("The code does X, the docs say Y. The docs are right — Y is what the spec promised. Want to fix the code?")
- New feature with no spec or docs → [**etak-develop /spec**](../skills/spec.md) ("Let's spec this before documenting; otherwise the doc is all we have.")
- Audit reveals widespread drift → schedule a focused doc sweep rather than fixing inline ("Five pages drifted — worth a doc sweep, not five small PRs?")
- Long-running drift correction → [**tech-writer agent**](../agents/tech-writer.md) for autonomous sweep

## Related

- [Skills index](README.md)
- [Tech-writer agent](../agents/tech-writer.md) — the autonomous version of this skill.
- [Deliver overview](../deliver.md) — how docs fits.
- [review](review.md) — when reviewing a PR, ask whether the change updates documentation that describes it.
