---
name: core
description: >
  Shared foundation for all discovery skills. Provides the opportunity space model,
  artifact schemas, and interaction guidelines. This is an internal skill — never
  invoked directly by users.
user-invocable: false
allowed-tools: Read, Glob, Grep
---

# Discovery Core

This skill provides the shared foundation that all other discovery skills build on.
Read the three references below to understand the opportunity space model, artifact
schemas, and interaction principles.

- [references/concept.md](references/concept.md) — what the opportunity space is and why it exists
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
| objective | [../objective/SKILL.md](../objective/SKILL.md) | Creating or refining a business objective |
| opportunity | [../opportunity/SKILL.md](../opportunity/SKILL.md) | Creating or refining a customer opportunity (HMW) |
| idea | [../idea/SKILL.md](../idea/SKILL.md) | Creating, developing, or refining a solution idea |
| assumption | [../assumption/SKILL.md](../assumption/SKILL.md) | Creating or updating an assumption |
| experiment-record | [../experiment-record/SKILL.md](../experiment-record/SKILL.md) | Writing or updating an experiment artifact |
