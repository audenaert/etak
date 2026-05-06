# Opportunity

Reference loaded by [discovery:artifacts](SKILL.md). See [model.md](model.md) for the opportunity space: typed links, lifecycles, naming, traceability.

You help create and refine opportunities — customer needs, pain points, or desires framed
as "How might we..." (HMW) questions. Opportunities live in the customer's world, not the
business's world. They invite solution thinking without embedding a specific approach.

## Schema

```yaml
---
name: "HMW help researchers discover works outside their existing knowledge?"
type: opportunity
status: active  # active | paused | resolved | abandoned
supports:
  - objective-increase-researcher-engagement
---
```

Body: description, evidence, who experiences this, how we learned about it.

Filename: `hmw-<kebab-slug>.md`. Opportunity filenames are prefixed with `hmw-` to make them visually distinguishable.

## What Makes a Good Opportunity

- **Customer-framed** — starts from the customer's perspective, not the business's.
  "HMW help researchers discover related works?" not "HMW increase engagement metrics?"
- **Solution-agnostic** — invites multiple possible answers. If only one solution comes
  to mind, the HMW is too narrow.
- **Specific enough to act on** — points toward solutions without prescribing them.
  "HMW help researchers?" is too broad. "HMW help researchers discover works outside
  their existing knowledge?" is actionable.
- **Evidence-backed** — ideally grounded in something you've observed or heard from
  customers, not pure speculation.

## Moves

### Explore the need
Ask what the PM has observed. Where did this come from — a customer conversation, data,
intuition? Help them articulate the customer's experience: who has this problem, when do
they encounter it, how painful is it, what do they do today?

### Frame as HMW
Help shape the observation into a well-framed HMW question. Offer 2-3 candidate framings
at different altitudes and let the PM choose:
- Broad: "HMW help researchers expand their reading?"
- Medium: "HMW help researchers discover works outside their existing knowledge?"
- Narrow: "HMW surface cross-disciplinary connections researchers wouldn't find on their own?"

### Connect upward
Check existing objectives in `docs/discovery/objectives/`. If a matching objective exists,
link via the `supports` field. If not, briefly note this opportunity is unanchored and offer
to create a lightweight objective.

### Check for duplicates
Read existing opportunities. If a similar HMW already exists, surface it. Maybe the PM is
describing the same need differently, or maybe these are distinct facets that should be
sibling opportunities.

### Assess decomposition
If the HMW is broad, it might be a parent opportunity with sub-opportunities beneath it.
"HMW improve the research experience?" might decompose into discovery, navigation, and
annotation sub-opportunities. Note this possibility but don't force decomposition.

### Write the artifact
Generate a kebab-case filename prefixed with `hmw-`. Write to `docs/discovery/opportunities/`.
Include frontmatter with `name`, `type: opportunity`, `status: active`, `supports` link,
and body with description, evidence, who experiences this, how you learned about it.
Always show before writing.

## Failure Modes

- PM describes a solution as an opportunity — "HMW add a search bar?" is a solution.
  Reframe: what need does the search bar address?
- HMW is too broad to act on — suggest narrowing or decomposing
- HMW has no evidence behind it — ask how they know this is real
- Opportunity duplicates an existing one — surface the match, discuss whether to merge
  or keep distinct
