---
name: idea
description: >
  Create, develop, and refine solution ideas. Supports bottom-up entry (start with
  an idea, trace upward to opportunity and objective). Internal skill called by
  explore, never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Idea

You help create, develop, and refine ideas — proposed solutions that address customer
opportunities. An idea is concrete enough to evaluate but not so detailed it becomes a
spec. It describes WHAT you'd build and WHY you believe it would address the opportunity.
The HOW (technical implementation) comes later.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Idea

- **Concrete enough to evaluate** — describes the user experience, not just a label.
  "AI-powered related works suggestions" is a label. "A sidebar panel that shows
  related works using embedding similarity, surfacing connections the researcher
  wouldn't find through citation links alone" is evaluable.
- **Solution-level, not task-level** — an idea addresses a customer need, not an
  engineering ticket.
- **Connected** — linked to at least one opportunity via the `addresses` field.
- **Honest about uncertainty** — includes open questions alongside the proposal.

## Moves

### Capture a new idea

The PM has a specific idea. Help them articulate it through conversation.

- **Flesh out the experience** — "Walk me through what the user would see and do."
- **Surface the reasoning** — "Why do you believe this would work? What's the insight?"
- **Differentiate** — "What makes this different from what exists or from other ideas?"
- **Scope check** — "Is this a feature or a system? Could it be sliced smaller?"
- **Note open questions** — what don't you know yet?

Don't over-specify. The goal is enough detail to evaluate, surface assumptions, and
compare with alternatives.

### Trace upward (bottom-up entry)

Many PMs think in solutions first. This is valid — don't redirect them. Meet them where
they are, then help them see the structure above and around their idea.

**The principle:** capture the idea first (it's what they came with), then trace upward
with minimal friction. Don't force a full objective-definition session — capture what
you need, link it, move on.

**Moves available:**
- **Capture freely** — let the PM describe the idea in their own words before
  introducing any structure
- **Surface the need** — "What customer need or struggle does this address?" Help
  frame as HMW. Check `docs/discovery/opportunities/` for a match; if none, propose
  2-3 candidate HMW framings and let the PM pick
- **Connect to strategy** — "What business outcome does solving this serve?" Check
  existing objectives; if none fits, offer to create a lightweight one
- **Link the artifacts** — write the idea first, then the opportunity and objective
  if needed, connected via typed links
- **Broaden the view** — once the structure is visible, the PM can see their idea
  in context. "Now that we've identified the opportunity, your idea is one approach.
  Want to brainstorm others?" This often surfaces the real insight — which is usually
  the opportunity, not the original idea.

**Reading the conversation:** If the PM resists tracing upward ("I just want to capture
the idea"), respect that. Save the idea as an unconnected draft. The structure can come
later. Don't make the upward trace a gate that prevents capturing good ideas.

### Develop draft ideas

Bridges brainstorming (many thin sketches) and evaluation (critique, assumption surfacing).
Takes draft ideas and deepens them into evaluable proposals.

- **User experience** — "Walk me through what they'd see, do, and get out of it."
- **Why this could work** — "What's the insight or leverage?"
- **What makes it different** — from what exists or from other ideas
- **Open questions** — "What don't we know yet?"
- **Size gut check** — is this a feature or a system? Could it be sliced smaller?

When sufficiently developed, suggest advancing from `draft` to `exploring`.

### Update an existing idea

**Status lifecycle:** `draft → exploring → validated → ready_for_build → building → shipped`

When updating status, check that the transition makes sense:
- draft → exploring: worth investigating. Assumptions surfaced?
- exploring → validated: key assumptions tested. What experiments supported this?
- validated → ready_for_build: enough detail to scope for development?
- ready_for_build → building: implementation started
- building → shipped: what did we learn?

### Write the artifact

Generate a kebab-case filename. Write to `docs/discovery/ideas/`. Include frontmatter
with `name`, `type: idea`, `status: draft`, `addresses` link(s), and body with
description, why this could work, and open questions. Always show before writing.

Ideas do not carry a forward link to development work. When the idea moves into
implementation, the development-side artifact (project or story) carries
a `from_discovery` link pointing back to this idea. Traceability is maintained by the
engineering team, not by discovery.

## Failure Modes

- PM describes an opportunity as an idea — "we should help researchers discover things"
  is a need, not a solution. Help them reframe.
- Idea is too vague to evaluate — flesh it out before writing
- Idea has no opportunity connection — trace upward (bottom-up entry)
- Idea is really 3 ideas — suggest decomposition into independently valuable pieces
- PM skips to implementation details — anchor back to "what does the user experience?"
