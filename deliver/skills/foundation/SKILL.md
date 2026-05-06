---
name: foundation
description: >
  Shared foundation for delivery skills — delivery model (the six phases),
  cross-plugin context (how delivery work consumes development and discovery
  artifacts), and interaction posture. Load when starting delivery work; for
  ongoing posture see guidelines.md directly.
user-invocable: false
allowed-tools: Read, Glob, Grep
---

# Delivery Foundation

Shared context for delivery skills. Deliver is downstream of develop and
discovery, and most of what makes a delivery decision *informed* lives in
artifacts those plugins produce.

Deliver does not yet model its own artifact graph — there are no per-type
artifact writers here. Each user-facing skill works against existing
artifacts (PRs, stories, specs, ADRs) and reads upstream context when the
companion plugins are installed.

## Cross-Cutting References

- [model.md](model.md) — what delivery work is in Etak terms, the six phases, skill ↔ agent pairing, cross-plugin context, standalone-install fallback
- [guidelines.md](guidelines.md) — interaction posture: thinking partner, ground in reality, signals to surface, anti-patterns, cross-plugin fallbacks
