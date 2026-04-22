# Critique

**Role:** structured examination of an idea or opportunity from a perspective the team doesn't naturally hold. Surfaces blind spots, challenges hidden assumptions, and confirms strengths. The first move that turns discovery from an inside game into a stress test.

## Why it exists

Every idea and opportunity arrives with the team's fingerprints on it. The people who proposed it believe in it. The risks they see are the ones they're primed to see. Critique exists to change the point of view deliberately — to ask "what would a skeptical end-user think?" or "what would an assumption audit reveal?" before those perspectives surface as user churn or launch failures.

Critique is not about finding fault. It is about finding *what the team doesn't already know*. A good critique round surfaces both risks and strengths. Risks tell you where to direct testing effort. Strengths tell you what holds up under scrutiny and where confidence is earned.

Two stances are available and they work differently:

- **Persona** — adopt a specific perspective and reason from inside it. "I am a first-time user who didn't choose this product. Here is what I would experience." The goal is texture and specificity that abstract analysis misses.
- **Framework** — apply a systematic lens to the artifact. "Apply assumption audit: what must be true for this to work?" or "Apply inversion: how would we make this fail?" Frameworks produce breadth; personas produce depth.

Running both stances across a critique session is the highest-signal combination.

A critique round has diminishing returns. Three to five rounds typically cover the critical angles. When subsequent rounds restate earlier findings, the session is done. More rounds are not better rounds.

## When to create one

- When an idea is moving from `draft` to `exploring` and the team hasn't yet stress-tested it from outside perspectives.
- When an opportunity's framing hasn't been challenged and you want to verify it captures the right problem.
- When a stakeholder challenge surfaces a new perspective worth running systematically.
- When assumption surfacing ([sound](../skills/sound.md)) produces beliefs the team is surprised by — critique often follows sound to examine whether those beliefs hold under external scrutiny.
- When the idea is high-stakes and the cost of a flawed framing is high — the earlier the critique, the cheaper the correction.

## Schema

A critique file aggregates what the graph decomposes. In the graph model, a critique session is a `Critique` node with one `Finding` node per observation, linked via `PART_OF`. In the file model, those are bundled into a single YAML document: session-level fields at the top, followed by a `findings` list. The import path decomposes the file into graph nodes; export re-aggregates them. This is an intentional shape difference, not a drift.

```yaml
---
name: "Skeptical end-user critique of offline-first reading"
type: critique
status: planned  # planned | running | complete
stance: persona  # persona | framework
perspective: "skeptical user — researcher who has been burned by sync failures before"
about_idea: offline-first-reading  # slug of the idea being critiqued; set one of about_idea or about_opportunity
# about_opportunity: null         # slug of the opportunity being critiqued; set one of these, not both
session: q2-discovery-push        # optional grouping slug
summary: ""
body: ""
findings:
  - name: "Assumes users trust the sync indicator"
    kind: risk          # risk | strength
    severity: serious   # fatal | serious | moderate | minor — required when kind is risk
    status: open        # open | addressed | dismissed
    resolution: ""
    body: |
      A researcher burned by a tool that showed "synced" when it wasn't will distrust
      any sync status indicator on first use. The idea needs a trust-building mechanism
      before the sync state carries meaning.

  - name: "Works well for predictable field conditions"
    kind: strength
    status: open
    resolution: ""
    body: |
      The offline-first model fits researchers who travel to archives or fieldwork
      locations with no connectivity. For that persona, the model is exactly right.
---
```

### Session fields

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short title describing the critique round and its perspective. |
| `type` | yes | Always `critique`. |
| `status` | yes | Lifecycle state of the session. |
| `stance` | yes | `persona` or `framework` — how this round approaches the critique. |
| `perspective` | yes | The specific persona or framework being applied. Free text. |
| `about_idea` | conditional | Slug of the idea being critiqued. Set exactly one of `about_idea` or `about_opportunity`. |
| `about_opportunity` | conditional | Slug of the opportunity being critiqued. Set exactly one of `about_idea` or `about_opportunity`. |
| `session` | no | Optional grouping slug to cluster related critique rounds. |
| `summary` | no | The tool's read on what this round found. Populated after the round completes. |
| `body` | no | Narrative context for the critique session — framing, goals, or anything that doesn't fit in individual findings. |

Exactly one of `about_idea` or `about_opportunity` must be set. The graph enforces this at write time — a critique with zero targets or two targets is rejected by the write resolver. The file schema relies on the same rule: set one, leave the other commented out or absent.

### Finding fields

Each element of `findings` corresponds to one `Finding` node in the graph.

| Field | Required | Description |
|-------|----------|-------------|
| `name` | yes | Short claim — one sentence, not a paragraph. |
| `kind` | yes | `risk` or `strength`. |
| `severity` | conditional | Required when `kind` is `risk`. One of: `fatal`, `serious`, `moderate`, `minor`. Not set when `kind` is `strength`. |
| `status` | yes | `open`, `addressed`, or `dismissed`. |
| `resolution` | no | Why or how the finding was addressed or dismissed. Populated when status is `addressed` or `dismissed`. |
| `body` | no | Explanation, evidence, or elaboration. The name carries the claim; the body carries the reasoning. |

## Severity calibration

Severity applies to risk findings only. Calibrate against the impact on the target artifact, not against general concern:

- **fatal** — if true, the idea or opportunity cannot be saved in its current form. Fundamental rethinking required.
- **serious** — significant problem that must be addressed before the idea advances. Not fatal, but not ignorable.
- **moderate** — real problem worth addressing, but the idea survives it. Manageable with targeted changes.
- **minor** — worth noting; unlikely to determine the outcome. Address after more important concerns are resolved.

## Status lifecycle

```
planned  ─────▶  running  ─────▶  complete
```

- **planned** — the critique round is scheduled but not yet run.
- **running** — actively examining the artifact.
- **complete** — findings recorded, session summary written.

### Finding lifecycle

```
open  ─────▶  addressed   (finding was resolved — update resolution field)
open  ─────▶  dismissed   (finding is not relevant — state why in resolution field)
```

A finding does not expire quietly. If addressed, record how. If dismissed, record why. A finding that silently disappears is a signal that got dropped.

Risk findings with high severity often graduate into explicit assumptions. When a finding becomes an assumption, the graph carries a `BECAME_ASSUMPTION` edge from the Finding node to the new Assumption node. In the file, this is captured in the finding's `resolution` field and a corresponding assumption artifact.

## Relationships

| Direction | Link | Target |
|-----------|------|--------|
| Outgoing | `about_idea` | one Idea |
| Outgoing | `about_opportunity` | one Opportunity |
| Findings list | bundled in file; `PART_OF` in graph | this critique |

Exactly one of `about_idea` or `about_opportunity` is set per critique. A critique cannot target both, and cannot be an orphan. This is by design: a critique that is not anchored to a specific artifact is notes, not a structured finding.

In the graph, the back-edges are on the idea and opportunity types: `Idea.critiques` and `Opportunity.critiques` traverse to all critiques of that artifact.

## Example

```markdown
---
name: "Assumption audit of offline-first reading idea"
type: critique
status: complete
stance: framework
perspective: "assumption audit — what must be true for offline-first reading to work?"
about_idea: offline-first-reading
session: q2-discovery-push
summary: |
  The core mechanics are sound, but the idea rests on two untested assumptions:
  that users will proactively manage their offline library before traveling, and
  that sync conflicts will be rare enough not to degrade trust. Both are testable.
  The strengths around field researcher fit were confirmed across all four personas
  examined in earlier rounds.
findings:
  - name: "Assumes users will proactively sync before going offline"
    kind: risk
    severity: serious
    status: addressed
    resolution: |
      Added a 'travel mode' assumption to the assumption set. Flagged for user
      interview testing before the idea advances to ready_for_build.
    body: |
      The offline-first model only works if users have already synced the content
      they need. A researcher who opens the app on an airplane and finds nothing
      cached will form a strong negative impression on first contact with the feature.
      The idea assumes proactive syncing behavior that may not exist.

  - name: "Assumes sync conflicts will be rare in practice"
    kind: risk
    severity: moderate
    status: open
    resolution: ""
    body: |
      If a user edits annotations on two devices before syncing, the conflict
      resolution behavior becomes visible. The idea hasn't specified what that
      behavior looks like or how users will feel about it.

  - name: "Strong fit for field researchers and archivists"
    kind: strength
    status: addressed
    resolution: "Confirmed. Retained as supporting evidence in the idea body."
    body: |
      For researchers who work in archives, remote sites, or areas with unreliable
      connectivity, offline-first is not a feature — it is a prerequisite. This
      persona segment values the capability highly and the fit is genuine.
---

## Session notes

Run as a framework critique (assumption audit) following three persona rounds.
The persona rounds covered: skeptical tech-adjacent user, low-connectivity
field researcher, platform IT administrator.

Key move: promoting the sync-before-travel assumption to an explicit assumption
artifact so it can be tested before the idea advances.
```

## Useful actions

### Run a critique

```
Critique this idea.
```

or

```
Critique this opportunity from the perspective of a non-technical stakeholder.
```

[Critique](../skills/critique.md) proposes stances and perspectives, runs the round, and saves the findings. You confirm before anything is written.

### Review open findings

Ask for a triage of open findings across the domain. High-severity open findings on ideas approaching `ready_for_build` are the primary signal to act on.

### Promote a finding to an assumption

When a risk finding articulates a testable belief, promote it:

```
Promote this finding to an assumption.
```

[Sound](../skills/sound.md) or [critique](../skills/critique.md) creates the assumption artifact, updates the finding's status to `addressed`, and links them in the graph via `BECAME_ASSUMPTION`.

### Close a finding

When a finding is resolved or no longer relevant, mark it `addressed` or `dismissed` and record the resolution. This is the hygiene move that keeps the open-findings queue meaningful.

## Tips

- **Both risks and strengths matter.** A critique that only finds fault is not a balanced read. Strengths confirmed under scrutiny are evidence worth keeping.
- **Personas produce depth; frameworks produce breadth.** Use both across a session.
- **Name findings as claims.** "Assumes users will proactively sync before going offline" is a claim. "Sync behavior" is a topic. Claims are actionable; topics are not.
- **Severity is calibrated to the artifact, not to abstract concern.** A moderate concern on a stage-1 idea and a moderate concern on a near-launch feature are not the same thing.
- **Diminishing returns is a signal, not a failure.** When new rounds restate old findings, the session has done its work. Stop.
- **Fatal findings don't always kill the idea.** They redirect it. "This must be rethought" is a valuable output — it is cheaper than building the wrong thing.

## Related

- [Idea](idea.md) — the solution a persona or framework may challenge.
- [Opportunity](opportunity.md) — the framing that a critique may validate or reframe.
- [Assumption](assumption.md) — where high-severity risk findings often graduate.
- [Critique skill](../skills/critique.md) — runs the critique session.
- [Sound skill](../skills/sound.md) — surfaces assumptions; complements critique.
- [Artifacts overview](README.md)
