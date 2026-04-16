---
name: experiment-record
description: >
  Create and update experiment artifacts. Handles writing experiment plans and recording
  results. Internal skill called by the user-facing experiment skill.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Experiment Record

You handle the artifact mechanics of experiments — writing experiment plans and recording
results. The user-facing `experiment` skill handles the full lifecycle conversation;
you handle the file operations.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## Writing an Experiment Plan

Create the artifact with full frontmatter:
- `name` — descriptive, includes method and scope ("Prototype test: related works panel
  with 5 researchers")
- `type: experiment`
- `status: planned`
- `tests` — assumption filename(s) being tested
- `method` — one of: user_interview, prototype_test, fake_door, concierge_mvp,
  data_analysis, ab_test, survey
- `success_criteria` — specific, measurable, set in advance
- `duration` — estimated duration
- `effort` — low/medium/high
- `result: null`
- `learnings: ""`
- `interpretation_guide` — clear validation, clear invalidation, inconclusive signals,
  confounding factors, what this doesn't tell us
- `action_plan` — if_validated, if_invalidated, if_inconclusive

Body includes: protocol, participant criteria, setup, procedure, data collection plan,
timeline.

Generate a kebab-case filename. Write to `docs/discovery/experiments/`. Always show
before writing.

## Recording Results

Update an existing experiment artifact:
- Set `status: complete`
- Set `result` to validated, invalidated, or inconclusive
- Fill in `learnings` with what was observed
- Add a Results section to the body with data, observations, and interpretation against
  the pre-committed criteria and interpretation guide

## Starting an Experiment

Update `status` from planned to running. Note the start date in the body.

## Failure Modes

- Experiment written without success criteria — the calling skill should have ensured
  this, but flag if missing
- Results recorded without referencing the interpretation guide — anchor back to the
  pre-committed criteria
- Result doesn't match any outcome in the action plan — flag as unexpected and discuss
  with the PM
