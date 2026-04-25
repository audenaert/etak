# spec

**Stance:** pragmatic. **Purpose:** grounded technical design. Specs and ADRs written against the actual codebase, not a hypothetical one.

Spec turns a story or epic into a design the team can implement against. Where [plan](plan.md) shapes what gets built and in what order, spec answers how. It reads the code before it writes prose. When the design includes a hard-to-reverse decision, it records that decision as an ADR.

The rhythm around spec is plan → spec → assess → build. Spec is the design pass that makes the build pass honest.

## When to reach for it

- **A story's approach isn't obvious.** The ACs are clear but the implementation path isn't. A spec makes the design visible before code lands.
- **An epic touches multiple subsystems.** You need a single document that names the seams, the data flow, and the contracts before the stories inside it start independently.
- **A decision is hard to reverse.** Vendor choice, protocol choice, data-model commitment. Capture it as an ADR so the audit trail holds.
- **The team disagrees on approach.** A spec with real alternatives surfaced turns disagreement into a visible choice the team can make.

Trigger phrases: *write a spec*, *design this*, *how should we build this*, *technical design*, *architecture for X*, *what's the approach*, *record this decision*, *write an ADR*.

## How it behaves

Spec reads the code first. Before writing a single section, it greps for the modules the spec will touch, reads the top of each relevant file, checks how similar features are implemented today, and notes the testing pattern neighbors use. A spec written without this reading is an opinion piece. If you push to draft first and verify later, spec pushes back: "I can sketch a skeleton, but the design moves will be guesses until I read the code."

When the code isn't available (greenfield work, or thinking out loud before touching the repo), spec is explicit about it. The current-state and alternatives sections get flagged as conceptual and requiring grounding before review.

### Frame

Before design, spec aligns on what the spec is for. What artifact does it serve — a project, an epic, a story? What's the question the spec answers? What's already decided in prior specs, ADRs, or discovery outcomes? A spec without a `for` anchor drifts into describing the whole system, which is too abstract to be useful. If a spec would just duplicate the story's ACs, spec pushes back rather than generate ceremony.

### Ground

Spec reads the code and captures what it finds in the Current State section: actual file paths, actual abstractions, actual testing patterns, prior ADRs that constrain the design. This is the section reviewers trust most. It's also the section that catches mistaken assumptions early, before they cost implementation time.

### Propose and alternate

Spec drafts the proposed approach, then stress-tests it by naming at least two alternatives, ideally three. The Alternatives Considered section is where the spec earns its weight. If there was only ever one option, this wasn't a real design choice, and spec questions whether the design doc is needed at all.

Non-functional requirements get named honestly: which are forced by this choice, which are deferred. Open questions get listed. If more than a couple of core questions are open, spec suggests a spike first.

### Record the decision (maybe)

Not every spec produces an ADR. Spec applies a hard-to-reverse test: would swapping this choice later cost weeks or more? Does it affect how the team writes code from now on? Does it close off options? If yes, spec invokes the ADR internal skill and links the ADR from the spec. If no, the decision lives in the spec body.

Accepted ADRs are effectively immutable. If the decision changes, spec writes a new ADR that supersedes the old one rather than amending in place.

## How to use it well

**Start from a story or epic, not a theme.** "How do we integrate Google OAuth" is a spec question. "Authentication" is too broad. If the artifact you're writing for doesn't fit on a sticky note, the spec will sprawl.

**Let the codebase push back.** If the spec proposes something the current system won't support cheaply, that's the most valuable thing the ground move catches. Let it reshape the design rather than waving it through.

**Write alternatives honestly.** A spec with one approach and two strawmen is a memo. Real alternatives means two or three options someone on the team would actually argue for.

**Spike before spec when the ground is unknown.** If the design hinges on an unknown ("we're not sure the database can handle this concurrency pattern"), spec suggests a time-boxed spike with decision criteria. Then the spec writes itself.

**Retroactive ADRs are fine.** When a decision made mid-build needs to be recorded, skip the spec and write the ADR directly. Capture the forces, the decision, the alternatives on the table at the time, and the consequences.

## Saving artifacts

Spec writes two artifact types through internal skills: specs (status: draft, review, approved, superseded) and ADRs (status: proposed, accepted, deprecated, superseded). The engineer sees the draft before it lands on disk. Spec sets the `for` anchor to the parent artifact and, when relevant, cross-links ADRs from the spec's `adrs` field.

See [the develop overview](../develop.md#the-artifacts) for how specs and ADRs sit in the hierarchy.

## Common failure modes

- **Writing prose before reading code.** The most common failure. The resulting spec is confident about things the codebase would have contradicted. Push back on yourself and on the engineer.
- **Spec that duplicates the story.** If the spec doesn't add how, it's not a spec. Delete rather than pad.
- **No alternatives considered.** A spec with one approach named is a memo. Reviewers need the alternatives section to form judgment.
- **Every question open.** If the spec is mostly question marks, it's a spike, not a spec.
- **Spec for routine work.** A short ADR plus inline comments handles more than you'd think. Not every change needs a design doc.
- **Amending an accepted ADR.** Write a new one that supersedes. The audit trail stays honest.

## Transitions

- Spec drafted and you want to stress-test it → [**assess**](assess.md) ("run multi-lens critique on this spec")
- Spec surfaces an unknown that needs investigation → spike (via spec's internal skills)
- Spec is approved and the first story is ready → [**build**](build.md)
- Spec surfaces that discovery wasn't actually settled → [**explore**](explore.md) or [**sound**](sound.md) on the discovery side
- Spec is growing beyond one epic's worth of design → [**plan**](plan.md) ("this wants to split")

## Related

- [Skills index](README.md)
- [Develop overview](../develop.md) — the six-skill rhythm and artifact hierarchy.
- [plan](plan.md) — the upstream move that shapes what a spec is for.
- [assess](assess.md) — the downstream move that stress-tests a spec before approval.
- [explore](explore.md) — discovery-side context that a `from_discovery` link points back to.
