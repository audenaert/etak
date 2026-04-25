---
name: critique
description: >
  Create and update critique artifacts — structured examination rounds that surface
  risks and strengths in ideas and opportunities. Handles writing rounds, updating
  finding status, and coordinating with assumption when findings graduate. Internal
  skill called by the user-facing critique skill.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Critique

You handle the artifact mechanics of critiques — writing rounds, updating finding
status, and coordinating with the assumption skill when risk findings graduate.
The user-facing `critique` skill runs the session conversation; you handle the file
operations.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Critique Artifact

- **One round per artifact.** Each artifact captures a single stance (persona or
  framework) examining a single target. Multi-round sessions are grouped via the
  `session` field, not bundled into one file.
- **Targets exactly one artifact.** Either `about_idea` or `about_opportunity` is
  set — never both, never neither. A critique that is not anchored is notes, not
  a structured finding.
- **Findings are claims, not topics.** "Assumes users trust the sync indicator" is
  a claim. "Sync behavior" is a topic. Claims are actionable.
- **Severity is calibrated to the target's maturity, not abstract concern.** A
  serious finding on a draft idea and a serious finding on a near-launch feature
  are different things. The calling skill calibrates; you record what was decided.

## Moves

### Write a critique round

Called from the user-facing critique skill once a round has produced findings.
You receive the round's stance, perspective, target, findings, and (optionally)
session grouping slug.

Generate a kebab-case filename like `skeptical-end-user-offline-first-reading.md`
and write to `docs/discovery/critiques/`. Frontmatter:

- `name` — short title naming the perspective and target
- `type: critique`
- `status` — usually `complete` when writing after a finished round; `planned` if
  the calling skill is scheduling a future round
- `stance` — `persona` or `framework`
- `perspective` — the specific persona description or framework name
- `about_idea` or `about_opportunity` — exactly one, set to the target slug
- `session` — optional grouping slug if the round is part of a multi-round session
- `summary` — the round's read on what it found, populated when the round completes
- `body` — narrative context if any
- `findings` — list of finding objects (see below)

Each finding has:
- `name` — short claim, one sentence
- `kind` — `risk` or `strength`
- `severity` — `fatal | serious | moderate | minor`, required when kind is `risk`,
  omitted when kind is `strength`
- `status: open` for newly recorded findings
- `resolution: ""` until the finding is addressed or dismissed
- `body` — explanation, evidence, or elaboration

Always show the artifact before writing.

### Update finding status

Findings move from `open` to `addressed` or `dismissed`. Both transitions require a
`resolution` — silence is not a state.

When a finding is **addressed**: record what was done. Common resolutions are
"promoted to assumption <slug>" (see below), "incorporated into idea body",
"target reframed", or "tested via experiment <slug>".

When a finding is **dismissed**: record why it does not apply. "Out of scope for
this artifact's stage" and "duplicate of finding X in <slug>" are typical reasons.

Edit the critique artifact in place — find the finding by name within the
`findings` list, update its `status` and `resolution`. Show the change before writing.

### Promote a finding to an assumption

When a risk finding articulates a testable belief, it graduates into an assumption.
This is a two-step move that you coordinate:

1. Call the internal `assumption` skill to create the assumption artifact, passing
   the finding text and the parent idea or opportunity. The assumption is written to
   `docs/discovery/assumptions/` with a body note recording its origin (the critique
   slug and finding name).
2. Update the originating finding: set `status: addressed`, set `resolution` to a
   sentence naming the new assumption slug — e.g., "Promoted to assumption
   `users-will-proactively-sync-before-going-offline`". The finding's `resolution`
   field is the authoritative file-side representation of the `BECAME_ASSUMPTION`
   edge.

Do not promote strength findings. They are not assumptions.

### Update session status

A critique advances `planned → running → complete` as the round progresses. Most
write operations from a finished round set `status: complete` directly. The
`running` state is used when the user-facing skill writes the artifact at the start
of a long round and updates it later.

### Group rounds into a session

The `session` field is a free-text slug. The calling skill picks it (often something
like `q2-discovery-push` or `<idea-slug>-stress-test`) and reuses it across rounds.
You do not interpret the slug — you write what you are given.

When asked to list rounds in a session, glob `docs/discovery/critiques/` and filter
by the `session` field.

## Failure Modes

- Critique written without a target — both `about_idea` and `about_opportunity`
  unset. Reject and ask the calling skill to specify exactly one.
- Critique written with both `about_idea` and `about_opportunity` set. Same — exactly
  one.
- Risk finding without a severity. Severity is required for risks. Push back to the
  calling skill.
- Strength finding with a severity. Strengths do not have severity. Strip the field.
- Finding marked addressed or dismissed without a resolution. Push back — the
  resolution is the audit trail.
- Promoting a strength to an assumption. Strengths are not testable beliefs in the
  sense an assumption requires. Decline and explain.
- Caller bundles multiple rounds into one artifact. The shape is one round per
  artifact. Split and write separately, joined by `session`.
