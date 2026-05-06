---
name: artifacts
description: >
  Type registry, schemas, and content guidance for development artifacts —
  projects, epics, stories, tasks, specs, ADRs, spikes, work-items,
  workstreams, milestones. Load when authoring a new artifact, validating
  one, or resolving the type system. For simple reads (get/list/find), read
  the artifact file directly without loading this skill.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Develop Artifacts

Entry point for working with development artifacts. Load this skill when
authoring a new artifact or resolving the type system. For simple reads,
work directly with the artifact file.

## Type Registry

| Type | Reference | Canonical path | When |
|------|-----------|----------------|------|
| project | [project.md](project.md) | `docs/development/projects/<slug>.md` | Bounded deliverable; coordination hub |
| epic | [epic.md](epic.md) | `docs/development/epics/<slug>.md` | Themed group of stories |
| story | [story.md](story.md) | `docs/development/stories/<slug>.md` | User-frame increment with testable ACs |
| task | [task.md](task.md) | `docs/development/tasks/<slug>.md` | Engineering-frame technical change |
| work-item | [work-item.md](work-item.md) | `docs/development/work-items/<slug>.md` | Standalone bug, chore, or enhancement |
| spike | [spike.md](spike.md) | `docs/development/spikes/<slug>.md` | Time-boxed investigation |
| spec | [spec.md](spec.md) | `docs/development/specs/<slug>.md` | Technical specification |
| adr | [adr.md](adr.md) | `docs/development/adrs/adr-NNN-<slug>.md` | Architecture decision record (sequential numbering, project-wide) |
| workstream | [workstream.md](workstream.md) | `docs/development/workstreams/<slug>.md` | Parallel delivery track within a project |
| milestone | [milestone.md](milestone.md) | `docs/development/milestones/<slug>.md` | Sequenced project checkpoint |

Slugs are kebab-case derived from the artifact name. ADRs use sequential
numbering across the entire project — check existing ADRs (and unmerged
branches) to find the next integer.

## Path Discipline

The paths above are canonical. Do not write to per-project subdirectories,
adjacent docs trees, or any other location. The flat-by-type layout is what
makes typed-link traversal and ADR numbering work.

If a caller — user, agent, or another skill — specifies a non-conforming
path, **surface the conflict before writing**. Silent compliance with a
wrong path breaks tools that walk the work graph. Users and orchestrating
agents routinely guess paths wrong; the schema is the source of truth.

## Authoring an Artifact

1. Identify the artifact type from intent.
2. Consult the per-type reference (column 2 above) for content guidance —
   schema, body sections, moves, failure modes.
3. Read [model.md](model.md) when shared concerns matter: common fields, typed
   links, status lifecycles, naming, traceability, readiness rules.
4. Generate the artifact content. Show before writing.
5. Write to the canonical path above.

## Cross-Cutting References

- [model.md](model.md) — the work graph: hierarchy, frames, common fields, typed links, lifecycles, naming, traceability, readiness
- [guidelines.md](guidelines.md) — interaction posture: thinking partner, ground in codebase, signals to surface, anti-patterns
