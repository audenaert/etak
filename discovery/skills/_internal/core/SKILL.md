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

## Internal Artifact Skills

When a user-facing skill creates or updates an artifact, read the matching internal
skill's SKILL.md and follow its guidance inline — no mode switch, no announcement.

| Skill | Path | Use when |
|-------|------|----------|
| objective | [../objective/SKILL.md](../objective/SKILL.md) | Creating or refining a business objective |
| opportunity | [../opportunity/SKILL.md](../opportunity/SKILL.md) | Creating or refining a customer opportunity (HMW) |
| idea | [../idea/SKILL.md](../idea/SKILL.md) | Creating, developing, or refining a solution idea |
| assumption | [../assumption/SKILL.md](../assumption/SKILL.md) | Creating or updating an assumption |
| experiment-record | [../experiment-record/SKILL.md](../experiment-record/SKILL.md) | Writing or updating an experiment artifact |
| critique | [../critique/SKILL.md](../critique/SKILL.md) | Writing a critique round or updating finding status |
| memo | [../memo/SKILL.md](../memo/SKILL.md) | Writing or updating a memo (framework, survey, analytical synthesis) |
