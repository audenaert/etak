---
name: artifacts
description: >
  Type registry, schemas, and content guidance for discovery artifacts —
  objectives, opportunities, ideas, assumptions, experiments, critiques,
  memos. Load when authoring a new artifact, validating one, or resolving
  the type system. For simple reads (get/list/find), read the artifact
  file directly without loading this skill.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Discovery Artifacts

Entry point for working with discovery artifacts. Load this skill when
authoring a new artifact or resolving the type system. For simple reads,
work directly with the artifact file.

## Type Registry

| Type | Reference | Canonical path | When |
|------|-----------|----------------|------|
| objective | [objective.md](objective.md) | `docs/discovery/objectives/<slug>.md` | Strategic goal — the business outcome being pursued |
| opportunity | [opportunity.md](opportunity.md) | `docs/discovery/opportunities/hmw-<slug>.md` | Customer need framed as "How might we…" |
| idea | [idea.md](idea.md) | `docs/discovery/ideas/<slug>.md` | Proposed solution that addresses an opportunity |
| assumption | [assumption.md](assumption.md) | `docs/discovery/assumptions/<slug>.md` | Belief that must be true for an idea to work |
| experiment-record | [experiment-record.md](experiment-record.md) | `docs/discovery/experiments/<slug>.md` | Test of an assumption — plan, run, results |
| critique | [critique.md](critique.md) | `docs/discovery/critiques/<slug>.md` | One round of structured examination of an idea or opportunity |
| memo | [memo.md](memo.md) | `docs/discovery/memos/<slug>.md` | Sustained analytical document (framework, survey, lit review) |

Slugs are kebab-case derived from the artifact name. Opportunity filenames
are prefixed with `hmw-`.

## Path Discipline

The paths above are canonical. Do not write to per-opportunity
subdirectories, adjacent docs trees, or any other location. The
flat-by-type layout is what makes typed-link traversal work.

If a caller — user, agent, or another skill — specifies a non-conforming
path, **surface the conflict before writing**. Silent compliance with a
wrong path breaks tools that walk the opportunity space. Users and
orchestrating agents routinely guess paths wrong; the schema is the
source of truth.

## Authoring an Artifact

1. Identify the artifact type from intent.
2. Consult the per-type reference (column 2 above) for content guidance —
   schema, body sections, moves, failure modes.
3. Read [model.md](model.md) when shared concerns matter: typed links,
   status lifecycles, naming, traceability to development.
4. Generate the artifact content. Show before writing.
5. Write to the canonical path above.

## Cross-Cutting References

- [model.md](model.md) — the opportunity space: structure, typed links, status lifecycles, naming, traceability
- [guidelines.md](guidelines.md) — interaction posture: thinking partner, signals to surface, anti-patterns
