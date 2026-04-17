# critique

**Stance:** adversarial but constructive. **Purpose:** stress-test an idea from perspectives you haven't considered.

Critique is the move where you step outside your own thinking. The idea has survived your own examination; now it needs to survive perspectives you wouldn't naturally hold. A skeptical user who has turned off three AI tools this year. A finance partner who will notice the cost structure you glossed over. A competitor planning a response. Critique inhabits each in turn and reports what they see.

Done well, critique leaves the idea sharper. Done poorly, it produces a wall of generic concerns nobody acts on. This page is about doing it well.

## When to reach for it

- **Before committing significant investment.** If you are about to scope, spec, or resource a feature, critique first. The cost of an hour of critique against the cost of a bad sprint is not close.
- **When you have unusually strong conviction.** Strong conviction is a signal to look for what you're not seeing. Not to back off — to look.
- **When the team is aligned suspiciously fast.** Universal enthusiasm is rarely a sign of a good idea. More often it's a sign nobody has pressure-tested it yet.
- **After sound surfaces fragile assumptions.** If the idea rests on several low-evidence beliefs, critique examines whether the idea itself holds up.

Trigger phrases: *stress-test this*, *poke holes*, *challenge this*, *what are we missing*, *critique*, *what could go wrong*, *play devil's advocate*.

## How it behaves

Critique uses two families of tools.

### Persona-based critique

The tool inhabits the perspective of a specific person who would interact with, be affected by, or need to build the idea. Not caricature — inhabit. A good persona critique round has specific context: who is this person, what are they optimizing for, what would they notice that the team hasn't?

Standard personas include:

- **The skeptical user.** Someone who has tried similar tools and turned them off.
- **The adjacent power user.** Someone who already has a workaround the tool would compete with.
- **The vulnerable user.** Someone who doesn't know enough yet to judge whether the feature is good.
- **The adversarial case.** Someone who would game the system to get a different outcome.
- **The stakeholder.** Finance, engineering, legal, support — whoever would be affected downstream.

You can also name specific customers or personas relevant to your context. The tool will inhabit whoever you ask it to.

### Framework-based critique

Analytical lenses that stress-test the idea's structure:

- **Assumption audit.** Look at the assumptions under the idea. Are any load-bearing and unexamined?
- **Second-order effects.** If this succeeds, what new problems does the success create?
- **Inversion.** If we wanted to sabotage this idea from inside the company, what would we do?
- **Competitive response.** If a competitor shipped this tomorrow, how would we respond? What does the response tell us about the feature's true value?
- **Edge cases.** What happens at the margins — tiny users, massive users, users who barely engage?

### Specific, severity-rated findings

Every finding comes with a severity: **fatal** (the idea cannot work as framed), **serious** (major risk requiring mitigation), **moderate** (friction worth addressing), **minor** (polish). Severity forces the tool to stake a claim rather than producing a uniform wash of concerns.

Generic findings get demoted to minor or dropped. "Users might not like this" is not a finding. "The skeptical user will compare this to three AI tools they already turned off and look for whichever button makes this one go away" is. Push for specificity.

## How to use it well

**Pick the right perspectives.** Three persona rounds from perspectives that genuinely challenge your thinking beat six rounds from perspectives that don't. Ask yourself whose opinion you most fear. That is the persona to start with.

**Use framework rounds to catch structural issues personas miss.** Personas find experiential problems. Frameworks find logical and strategic ones. You want both.

**Confirm what's strong.** Critique that only finds weaknesses is demoralizing and incomplete. A critique session should also surface what survived scrutiny. Ideas that survive critique are stronger than ideas that haven't been critiqued. Name the strengths explicitly.

**Know when to stop.** The tool tracks coverage. After three or four rounds, new findings start echoing earlier ones. That is the signal to stop. Critique has diminishing returns; a team that critiques forever ships nothing.

**Translate findings into next moves.** Every finding should point at a concrete next step: a new assumption to test, a change to the idea, or a reason to kill the idea. If findings pile up without changes, the session was theater.

## Three useful critique patterns

### Pre-mortem

Before committing to an idea, imagine it shipped and failed. Walk backwards: *why did it fail?* Etak will generate three or four plausible failure modes. The exercise surfaces concerns the team would rather not raise directly.

### Defender-and-prosecutor

Run two critique rounds on the same idea back-to-back. The first adopts the role of the most skeptical person on your team. The second adopts the role of the idea's most ardent defender. Both are specific and concrete. The pairing forces balance.

### Segment-specific

A feature that works for one user segment often breaks for another. Run critique rounds as representatives of three different segments (power users, new users, enterprise). The comparison reveals scope assumptions you hadn't surfaced.

## Common failure modes

- **Generic concerns.** "Users might find this confusing." Specificity is the whole value. If the tool drifts vague, push back.
- **Critique as veto.** Critique produces findings. The PM decides. A thoughtful PM who has seen the findings and chosen to proceed is a thoughtful PM. The graph records the decision.
- **Critique as theater.** Running critique to demonstrate rigor, without being willing to change what you do, is worse than not critiquing. If no finding could change your mind, don't run the session.
- **Critiquing ideas that haven't been sounded.** If critique keeps hitting "this rests on belief X," go to [sound](sound.md) and examine the belief first. Critique examines the idea. Sound examines what the idea rests on.
- **Critique without capture.** The session produced twelve findings. You acted on three and forgot the rest. Save the critique summary; future-you will need it.

## Transitions

- Blind spots reveal untested assumptions → [**sound**](sound.md) ("this critique surfaced some beliefs worth examining")
- Fatal concerns suggest the approach needs rethinking → [**explore**](explore.md) ("want to brainstorm alternative directions?")
- Assumptions surfaced during critique need ranking → [**prioritize**](prioritize.md) ("several worth testing, which first?")
- Critique held up well → move forward with confidence ("this survived scrutiny; main risks are manageable")

## Related

- [Skills index](README.md)
- [Critiquing an idea tutorial](../tutorials/critiquing-an-idea.md) — runnable walkthrough.
- [Sound](sound.md) — the inward counterpart. Sound looks at what an idea rests on; critique looks at the idea from outside.
- [Product-researcher agent](../agents/product-researcher.md) — competitive research often feeds useful critique rounds.
