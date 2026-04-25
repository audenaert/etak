---
name: core
description: >
  Shared foundation for all development skills. Provides the development graph
  model, artifact schemas, and interaction guidelines. This is an internal skill —
  never invoked directly by users.
user-invocable: false
allowed-tools: Read, Glob, Grep
---

# Development Core

This skill provides the shared foundation that all other development skills build on.
Read the three references below to understand the development graph, artifact schemas,
and interaction principles.

- [references/concept.md](references/concept.md) — what the development graph is and how it connects to discovery
- [references/schemas.md](references/schemas.md) — artifact types, frontmatter, typed links, directory layout
- [references/guidelines.md](references/guidelines.md) — how to be a thinking partner, not a form-filler

## Internal Artifact Skills

When a user-facing skill creates or updates an artifact, read the matching internal
skill's SKILL.md and follow its guidance inline — no mode switch, no announcement.

| Skill | Path | Use when |
|-------|------|----------|
| project | [../project/SKILL.md](../project/SKILL.md) | Creating or refining a bounded project |
| epic | [../epic/SKILL.md](../epic/SKILL.md) | Creating or refining an epic |
| story | [../story/SKILL.md](../story/SKILL.md) | Creating or refining a user-facing story with ACs |
| task | [../task/SKILL.md](../task/SKILL.md) | Creating developer-scoped tasks under a story |
| work-item | [../work-item/SKILL.md](../work-item/SKILL.md) | Creating a standalone bug, chore, or enhancement |
| spike | [../spike/SKILL.md](../spike/SKILL.md) | Creating a time-boxed investigation |
| spec | [../spec/SKILL.md](../spec/SKILL.md) | Writing a technical specification |
| adr | [../adr/SKILL.md](../adr/SKILL.md) | Recording an architecture decision |
| workstream | [../workstream/SKILL.md](../workstream/SKILL.md) | Defining a parallel delivery track within a project |
| milestone | [../milestone/SKILL.md](../milestone/SKILL.md) | Defining a sequenced project checkpoint |
