# Memo

**Role:** a freeform analytical document capturing in-depth thinking about the opportunity space. Not a gap to fill, not a proposed solution — a record of reasoning that informs how the team understands what they are building and why.

## Why it exists

Some discovery work doesn't fit cleanly into any OST node. A mathematical framework for thinking about graph structure. A competitive landscape survey. A deep dive into a research area. A first-principles argument for or against a strategic direction. These are substantive artifacts — they shape how the team thinks about opportunities and ideas — but they are not themselves opportunities or ideas.

Without a home for this work, it disappears: written once, cited informally, never linked, eventually lost. The memo type gives it a first-class place in the discovery graph.

Memos are intentionally format-agnostic. A memo might be a literature review, a position paper, a framework analysis, a market survey, a system theory sketch, or a structured argument. What they share: significant analytical depth, a named author or session, and a connection to the opportunity space they inform.

## When to create one

- When you've done research or analysis that the team will want to reference — not a passing observation, but something worth preserving as a document.
- When you've developed a theoretical or conceptual framework that shapes how you're thinking about the problem space.
- When a competitive analysis, market survey, or literature review produces findings worth linking to specific opportunities.
- When a working session produces a structured argument that doesn't fit an experiment, assumption, or critique.

If the finding is a single sharp *aha* from a user interview, that belongs in an opportunity's evidence section. If the finding is a multi-page synthesis you arrived at through sustained analysis, write a memo.

## Schema

```yaml
---
name: "Human-readable name"
type: memo
status: draft  # draft | active | archived
supports:
  - opportunity-or-objective-filename-without-ext
---
```

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short descriptive title for the memo. |
| `type` | yes | Always `memo`. |
| `status` | yes | Current lifecycle state. |
| `supports` | no | Slugs of objectives or opportunities this memo informs. Omit if the memo is foundational rather than tied to a specific artifact. |

## Status lifecycle

```
draft  ────▶  active  ────▶  archived
```

- **draft** — being written or still in flux.
- **active** — complete and available for reference; reflects current thinking.
- **archived** — superseded by later work or no longer relevant. Kept for the record.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `supports` | Objective(s), Opportunity(ies) |

Memos are not required to link forward. A memo that develops a foundational framework for thinking about the product space may not target any specific artifact — it informs the whole. As specific opportunities and ideas emerge from the framework, they carry that connection back in their own evidence sections or body.

## Example

```markdown
---
name: "The graph-theoretic structure of an opportunity space"
type: memo
status: active
supports:
  - no-computational-model-for-opportunity-exploration
---

## Summary

An opportunity space is a typed, directed, heterogeneous graph. Its structural
properties — connected components, spanning subgraphs, bridge edges, community
partitions, and graph projections — are not incidental to the model; they are a
readout of how well a team understands their problem space...
```

## Tips

- **Depth over breadth.** If a memo is trying to cover everything, it probably needs to be split — one memo per coherent analytical frame.
- **Link what it informs.** Even a loose `supports:` link to one or two opportunities is better than orphaning the memo. It makes the memo discoverable from the artifacts it shaped.
- **Don't over-structure the body.** The format should serve the argument. A framework memo might have clean sections and a table. A working-through-a-problem memo might be more discursive. Both are fine.
- **Status honestly.** `active` means you'd stand behind this today. If the thinking has moved on, archive it rather than letting a stale memo silently mislead.

## Related

- [Opportunity](opportunity.md) — the most common `supports` target for a memo.
- [Objective](objective.md) — for memos that inform strategic direction rather than a specific problem area.
- [Critique](../artifacts/README.md) — for adversarial stress-testing of a specific artifact. A memo is generative; a critique is adversarial.
- [Artifacts overview](README.md)
