---
name: core
description: >
  Shared foundation for all delivery skills. Provides the delivery model, cross-plugin
  context, and interaction guidelines. This is an internal skill — never invoked
  directly by users.
user-invocable: false
allowed-tools: Read, Glob, Grep
---

# Delivery Core

This skill provides the shared foundation that all other delivery skills build on. Read the references below to understand the delivery model and interaction principles.

- [references/concept.md](references/concept.md) — what delivery work is in Etak terms, how skills and agents pair, and how cross-plugin context flows in
- [references/guidelines.md](references/guidelines.md) — how to be a thinking partner for delivery work: ground in reality, not assumptions

## Internal Artifact Skills

Deferred. Deliver does not yet model its own artifact graph. When delivery work needs to read structured upstream context (stories, specs, ADRs, assumptions), it reads from `docs/development/` and `docs/discovery/` directly when those plugins are installed. Each user-facing skill states its fallback when they are not.
