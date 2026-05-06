---
name: "HMW prevent behavioral regressions in our agents and skills as they evolve?"
type: opportunity
status: active
---

## Description

Etak's agents and skills are defined by prompts. Prompts evolve constantly — wording sharpens, sections move, internal-skill references get rewritten. That evolution silently produces behavioral drift that breaks user expectations and can corrupt the user's work graph: artifacts written to non-conforming paths, frontmatter shape regressions, an agent skipping a required step, a skill that used to surface a conflict now silently complying.

For users — especially dogfooders and the first wave of beta users — the failure mode is *"it worked yesterday; why does it produce nonsense today?"* Trust erodes faster than it builds. Etak's positioning depends on agents that behave consistently across releases.

## Evidence

This is grounded in an incident that just happened, not speculation.

On 2026-05-04, during the substrate-pivot work on the `audenaert/forge-app` repo, the `architect` agent produced a complete project-design corpus — project doc, spec, nine ADRs — and wrote it all to `docs/projects/postgres-substrate-migration/` (with ADRs nested under the project folder). The schema-prescribed paths are `docs/development/projects/<slug>.md`, `docs/development/specs/<slug>.md`, and `docs/development/adrs/adr-NNN-<slug>.md` — flat-by-type, with global ADR numbering.

Two contributing factors:
1. The dispatcher's brief specified the wrong path. The architect followed it without surfacing the conflict.
2. The architect's instructions referenced `skills/_internal/spec/` and `skills/_internal/adr/` as schemas to "follow" rather than as documents to read for write-path discovery.

The deviation was caught by a human reader, not by tooling. PR #85 on `audenaert/forge-app` captures the artifacts and PR description if anyone wants to walk the failure.

The architect-side and skill-side fixes for *this specific issue* are landing alongside this opportunity. But the underlying class of problem — silent behavioral drift between releases of an agent or skill prompt — is broader. The next regression will not be path-related.

## Who experiences this

- **Dogfooders** (Neal today, broader internal users tomorrow) — discover regressions while doing real work, lose hours, and start working around the tool.
- **Beta users** — encounter regressions without the context to diagnose them; conclude the tool is unreliable.
- **Paying customers (future)** — any regression that corrupts their work graph is a churn event.

## How we learned about it

Direct observation of a real failure on 2026-05-04, plus the recognition (in the post-incident analysis) that the contributing factors generalize beyond paths. The fix in flight closes the path-conformance gap; the eval-suite idea below closes the broader regression-detection gap.
