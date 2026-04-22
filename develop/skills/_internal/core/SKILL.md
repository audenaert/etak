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

## Skill Composition

Skills compose by reading each other's SKILL.md files. When a user-facing skill needs
to create or update an artifact, it reads the appropriate internal skill to get the
guidance for that artifact type.

**How to invoke an internal skill:** Read its SKILL.md file and follow its guidance
within the current conversation. The internal skill's instructions become part of your
active context. You don't need to switch modes or announce the transition to the user —
just seamlessly apply the artifact skill's guidance when the moment calls for it.

**Internal artifact skills available:**

| Skill | Path | Use when |
|-------|------|----------|
| initiative | [../initiative/SKILL.md](../initiative/SKILL.md) | Creating or refining a multi-project initiative |
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
