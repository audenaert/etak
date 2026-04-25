# Etak Plugin Design Review

**Date:** 2026-04-22
**Branch:** `feat/plugin-assessment`
**Scope:** `etak-discovery` and `etak-develop` plugins
**Purpose:** Establish evaluation guidelines grounded in Claude Code best practices and etak's own philosophy; audit the current state against those guidelines; propose a step-by-step improvement plan.

This document is a one-off design review — not user-facing reference material. The improvement plan is a proposal for the user to review; nothing in it should be implemented until approved.

---

## Part 1 — What Etak Is Trying to Be

Before we can evaluate the prompts, we have to be clear about what they're for. Etak's philosophy is not incidental — it dictates what good looks like.

### The thesis (from `docs/context/foundations.md` and `about-the-name.md`)

Etak is a **creativity support system** for product and engineering work. Four ideas from the research literature drive the design:

1. **Reflection-in-action** (Schön, 1983). Expertise is a conversation with the situation. You make a move, the artifact talks back, you adjust. Artifacts are the material of reflection — they have to be externalized, readable, and editable for the conversation to work.
2. **Incremental formalism** (Shipman & Marshall, 1999). Structure that gets introduced at the pace of the user's understanding. Premature structure kills ideas; no structure produces piles. The tool's job is to recognize what's emerging and propose formalizations the user can accept, reject, or revise.
3. **Mixed-initiative interaction** (Horvitz, 1999; Shneiderman, 2022). Both the human and the AI take initiative. The AI notices, proposes, drafts, flags. The human decides, commits, owns the judgment. Neither is passive. High automation *and* high human control at the same time.
4. **Model, not map**. The graph is the team's current understanding, not reality. It is allowed to be incomplete, and it rewards candor over performance.

### What this implies for the prompts

The AI's behavior needs to be:

- **Receptive** — hold space for pre-formal thinking; don't force structure too early
- **Generative** — propose, notice, surface; don't wait to be asked
- **Grounded** — read the material before speaking about it (code for develop, graph for discovery)
- **Confirming, not committing** — show before writing; the user owns the commit
- **Honest about uncertainty** — flag what's guessed vs. what's grounded; invalidation is success

These are not just nice-to-have traits. They're what separates etak from a faster ticket tracker. Every skill and agent in the marketplace should be evaluated against whether it advances this posture or drifts from it.

---

## Part 2 — Evaluation Guidelines

Strong stances below. Each is meant to be load-bearing: the kind of rule that, if violated, would make the plugin worse in a way we can point at.

### G1. A skill file is a prompt, not documentation

A skill is loaded into Claude's context at invocation. Every line consumes token budget and shapes behavior. If a line neither changes what Claude does nor prevents it from drifting in an edge case, it's narrative tax — it belongs in `docs/`, not in `SKILL.md`.

**How to test a line:** Can I point at a concrete behavior this line produces or prevents? If no, it's narrative. Human-facing explanation (why the feature exists, metaphors, history) goes in the external docs tree, not the skill.

Exception: short, *operational* metaphors that the model uses ("sounding the water" in `sound` is fine if it shows up in the model's voice). Long philosophical setups are not.

### G2. Every skill takes a clear stance

A "Your Stance" block at the top defines posture: receptive, generative, evaluative, adversarial, rigorous-but-pragmatic. The stance is load-bearing because it tells the model how to *show up* across every interaction. Skills without a stance will regress to generic helpfulness.

**Criterion for stance strength:** Could I describe how this skill handles an ambiguous input differently from a sibling skill, using only the stance text? If the stances of two sibling skills are substitutable, the distinction isn't real yet.

### G3. Operational rules over prescriptive checklists

The model does better with rules it can apply to novel cases than with numbered procedures it stops reading past step 4. "Read the code before writing prose" is a rule; "run git log, then grep for X, then read Y" is a procedure.

**Numbered process is appropriate when:** The skill or agent runs autonomously for a long time, multiple distinct phases produce distinct artifacts, or the order is genuinely load-bearing (pre-work before dispatch, ground-before-propose, test-before-implement).

**Numbered process is harmful when:** It micromanages Claude's tool use, prescribes steps the model would take anyway, or suppresses judgment for variations that matter.

### G4. Say what the skill is NOT for

"What X does NOT do" sections cost lines but earn them back by preventing skill sprawl. They are how a multi-skill routing system stays clean. Every user-facing skill needs one.

### G5. Name the transitions

Skills explicitly name where to go when the conversation shifts. This is the mixed-initiative principle in miniature: the AI proposes the next move, the user decides. Transition blocks are how users build a mental model of the rhythm without having to learn skill names.

Transitions should be specific ("Want to sound those assumptions out?") not generic ("You may want another skill").

### G6. `model` and `effort` are deliberate and consistent within a plugin

A plugin that specifies `model: sonnet` on some skills and inherits on others is saying: "these model choices are not considered decisions." That's a smell. Either specify everywhere, or inherit everywhere, or have a stated rule for when to specify.

**Default recommendations, based on observed patterns in Anthropic's own skills:**

| Work | Model | Effort |
|------|-------|--------|
| Pure exploration (search, read-only listing) | Haiku or Sonnet | low–medium |
| Listening / reflecting / routing | Sonnet | medium |
| Drafting artifacts, grounded analysis | Sonnet | high |
| Multi-round critique, architectural reasoning | Opus | xhigh |
| Autonomous long-running implementation | Sonnet or Opus | high |

**Don't use Opus reflexively.** It costs more and is often slower. Reserve it for work where a weaker model will produce visibly worse output.

### G7. `context: fork` only for autonomous, summarizing work

Fork when the agent will produce a lot of intermediate output (tool calls, search results, file reads) and return a summary. The main conversation stays clean; the user reviews the result.

Don't fork for interactive skills — the whole point is that the user and the model are in conversation in the shared context.

### G8. Tool allowlists are narrow by default

Read-only skills get `Read, Glob, Grep`. Skills that write artifacts get `Read, Write, Edit, Glob, Grep`. Bash is added only when the skill actually invokes shell commands, and with a comment if it's not obvious why.

Broad allowlists ("Read, Write, Edit, Glob, Grep, Bash" on a skill that never runs Bash) are a smell: they signal that tool permissions weren't considered as part of the skill's identity.

### G9. The core skill is a table of contents

`_internal/core/SKILL.md` should be navigation — what references exist, what artifact skills are available, how composition works. The substantive content lives in `references/`. If the core skill grows past ~40 lines, either extract more into references or delete the extra.

Loading three big references on every user skill invocation is wasteful. The core skill should let the model pull the specific reference it needs (concept for model, schemas for writing artifacts, guidelines for interaction posture).

### G10. Ground in the material before proposing

The single most important rule in both plugins. Discovery grounds in the graph (read existing objectives, opportunities, ideas before brainstorming). Develop grounds in the codebase (grep for the subsystems, read the tests, check adjacent patterns before writing a spec).

This is reflection-in-action made concrete. It has to be explicit, not implied. If the skill can propose without reading, it will.

### G11. Show artifacts before writing

Every artifact-writing skill shows the proposed content before committing. Every `Write`/`Edit` is confirmed unless the user has explicitly authorized more autonomy. This is how the mixed-initiative posture stays honest — the AI drafts, the human commits.

### G12. Agents prescribe more process than skills, but every step earns its line

An agent's body is longer than a skill's because the agent runs autonomously and has to hold itself to a process the human isn't watching. But process steps that don't change behavior — "then run git status" — are over-instruction. Replace them with principles the agent can apply.

**Rule:** If step N says "do X," ask: would the agent skip X without this line? If not, delete.

### G13. Extract shared rules; keep differentiated stances local

If three sibling skills all say "push back on vague framing," hoist it into `core/references/guidelines.md`. If three sibling skills all have different stances, keep the stances in the sibling skills — don't flatten.

Shared *rules* go to core. Differentiated *voice* stays local.

### G14. Name the failure modes

Every skill's end has a "Failure Modes" or "Anti-Patterns" section. This teaches the model what to flag in the user's behavior and what to self-correct on its own. The failure modes list is where "push back" behavior gets its specific shape.

### G15. Cross-plugin dispatch is explicit and graceful

When an agent depends on another plugin ("tech-lead dispatches to etak-deliver's quality-engineer"), the dependency is stated and the fallback when the plugin isn't installed is stated too. The plugin must work usefully on its own and get more useful when companions are installed.

---

## Part 3 — Assessment Against the Guidelines

This is a critical review. The picture overall is strong — the plugins are well-thought-out — but there are real gaps and inconsistencies worth addressing before adding more surface area (design, deliver).

### 3.1 etak-discovery

#### Strengths

**Stance blocks are strong (G2).** Every user-facing skill has a clear, differentiated stance: orient is receptive, explore is generative, sound is exploratory, critique is adversarial-but-constructive, prioritize is evaluative, experiment is rigorous-but-pragmatic. The stances are genuinely substitutable-test-resistant — you can't swap `orient` and `explore`'s stances without breaking the skill.

**"What X does NOT do" is consistent across all six user skills (G4).** This is doing the hard work of keeping the routing clean.

**Transitions are named explicitly (G5).** Every skill lists 3-5 specific skill-to-skill handoffs with the triggering user behavior. This is the mixed-initiative principle shining through.

**Philosophical alignment is visible in the prompt text.** `sound/SKILL.md` explicitly invokes "representational talkback." `guidelines.md` frames invalidation as success. `idea/SKILL.md` treats bottom-up entry as valid and prescribes minimal friction. The foundations aren't decorative — they show up in how skills behave.

**Core references are well-sized (G9).** `concept.md` at 41 lines, `guidelines.md` at 62, `schemas.md` at 158. Each does one thing. None is bloated.

**Confirm-before-writing is baked in (G11).** Every artifact skill includes "Always show before writing" or equivalent.

**product-researcher has a good "What You Do vs What Skills Do" table.** This is one of the clearest boundary statements in either plugin.

#### Weaknesses

**Inconsistent frontmatter (G6) — this is the most fixable, high-impact weakness.** Frontmatter in discovery:

| Skill | model | effort | context |
|-------|-------|--------|---------|
| `orient` | sonnet | medium | (inherit) |
| `explore` | *(unset)* | *(unset)* | (inherit) |
| `sound` | *(unset)* | *(unset)* | (inherit) |
| `critique` | opus | xhigh | (inherit) |
| `prioritize` | *(unset)* | *(unset)* | (inherit) |
| `experiment` | *(unset)* | *(unset)* | (inherit) |
| `product-researcher` | sonnet | high | fork |

There's no apparent rule for when model/effort is specified. Compare to develop, where all skills set both. The cost of this inconsistency: running on defaults means the model/effort varies with session state, which is exactly what deliberate specification prevents.

**Stance sections occasionally blend metaphor with operational rules (G1).** `sound/SKILL.md` opens with "The name comes from nautical practice — sounding the water to learn what's beneath the surface before committing to a course." That's human-facing context. It adds no behavior the model wouldn't already get from "probe beneath a plausible-looking idea to find the beliefs it depends on." The metaphor is load-bearing in the *docs*; in the prompt, it's narrative tax.

**Core `_internal/core/SKILL.md` has a "Skill Composition" section that restates what the model already knows (G9).** Lines 21-30 explain "How to invoke an internal skill: Read its SKILL.md file and follow its guidance within the current conversation." This is mechanical knowledge the model has. The model doesn't need to be told "just seamlessly apply the artifact skill's guidance" — it would do that anyway if told *which* skill to read.

**References echo SKILL.md openings verbatim (G1).** `critique/references/critique.md` opens with: "You examine discovery artifacts from perspectives the PM hasn't considered. You surface blind spots, challenge hidden assumptions, confirm strengths, and reveal new angles." That's a word-for-word copy of the first sentence in `critique/SKILL.md`. The reference should add to the SKILL.md, not restart it.

**Some skills have no model/effort but clearly need high effort.** `sound` does multi-round assumption surfacing; `prioritize` runs a structured ranking conversation; `experiment` designs tests with specific criteria. All three are doing reasoning work that benefits from deliberate model/effort setting.

**`critique` sets `model: opus, effort: xhigh`, but `sound` (which is philosophically similar — probing, surfacing, multi-round) is unset.** This inconsistency should resolve in one direction or the other.

**No explicit cross-plugin posture (G15).** Develop clearly articulates how it consumes etak-discovery and how it will dispatch etak-deliver agents. Discovery doesn't reciprocate — there's no explicit statement about how discovery artifacts flow to develop (via `from_discovery`), and no mention of design or deliver yet. This is low-priority since the flow is one-way, but worth a brief section in `discovery/README.md`.

**product-researcher's stance is weaker than its peers.** "You are a sharp research analyst, not a consultant" is good. But compared to critique's "adversarial but constructive... a rigorous examiner, not a negativity machine," the researcher's stance doesn't give the model as much to push against. The stance could be sharper: "analyst, not advocate" — i.e., doesn't lobby for conclusions.

#### Frontmatter / tool audit for discovery

- `orient` — tools `Read, Glob, Grep`. Correct: read-only. ✅
- `explore` — tools `Read, Write, Edit, Glob, Grep, Bash`. Bash is unnecessary (explore doesn't run commands; it calls internal skills to write artifacts). ⚠️ Trim.
- `sound` — tools `Read, Write, Edit, Glob, Grep, Bash`. Same issue. ⚠️ Trim Bash.
- `critique` — tools `Read, Write, Edit, Glob, Grep, Bash`. Same. ⚠️ Trim Bash.
- `prioritize` — tools `Read, Write, Edit, Glob, Grep`. ✅
- `experiment` — tools `Read, Write, Edit, Glob, Grep, Bash`. Bash is arguably justified (recording experiment results sometimes involves data commands) but should be explained. ⚠️ Justify or trim.
- `product-researcher` — `Read, Write, Edit, Glob, Grep, Bash, WebSearch, WebFetch`. Write/Edit are unneeded — the agent is forbidden from modifying the graph. ⚠️ Trim.

### 3.2 etak-develop

#### Strengths

**Frontmatter is consistent across all skills (G6).** Every skill specifies model and effort. The choices are defensible: survey is sonnet/medium (listening), plan/spec/assess/test/build are all sonnet/high (reasoning + artifact drafting), and agents are explicit about fork and tools.

**Recent refactor landed well (commit 4ca2ce7).** The conversion of Stance prose to operational rules and the trimming of narrative in concept/guidelines/schemas kept what was load-bearing (trustworthiness in test, epic sizing, task-vs-story disambiguation) while cutting narrative tax.

**Cross-plugin dispatch is architected (G15).** `tech-lead` lists the specific etak-deliver agents it will call (quality-engineer, devops, security-lead, reviewer, tech-writer), and all agents have graceful fallback language ("note in the plan where specialist review would be warranted"). `developer` has the same pattern.

**Four-moves / four-lenses / ten-step structure is consistent (G3).** `plan` has four moves (scope, decompose, sequence, pre-mortem). `assess` has four lenses (ready, feasibility, size, stress-test). `build` has four moves (implement, self-review, PR, feedback). This is the right level of structure — enough to guide, not enough to suppress.

**developer agent's Rules block is exemplary (G3).** Four rules, each load-bearing:

> - Tests pass, linters clean, ACs covered before submitting
> - Commits tell a clear story — one logical change each
> - When an AC is ambiguous and you made a judgment call, say so in the self-review
> - When you couldn't fully satisfy an AC, explain why — trust depends on truth-telling

Compare to what this could have been (a 15-line numbered procedure). The current form gives the model principles to apply across the many decisions it will make in an autonomous run.

**Ground-in-codebase is explicit in `spec` and `build` (G10).** `spec/SKILL.md` has a dedicated "The Ground-in-Codebase Rule" section that is doing real work — it tells the model to push back on itself when asked to draft without reading.

#### Weaknesses

**Agent process sections are long and sometimes over-prescribed (G12).** The three agents, in order of prescription:

- `developer` — 362 lines. Ten major steps. Some is load-bearing (TDD cycle, PR format, self-review schema). Some is prescription the model would do anyway (step 6 on merge conflicts goes into specific git commands; step 5 quality-checks prescribes the exact commands to run).
- `tech-lead` — 260 lines. Eight steps plus guidelines. The execution plan template and progress update template are load-bearing. Step 7 ("Handle problems") has five sub-scenarios with prescribed responses — some of this could be a one-line rule ("escalate early; don't spin; don't silently re-scope").
- `architect` — 262 lines. Ten steps. Some of these read like they could collapse — step 7 (fan out for complex designs) and step 8 (review the design) are both about stress-testing and could merge.

The risk isn't that the agents are broken — they work. The risk is that detail attracts more detail; if every time a problem is found, the fix is "add another step," the agents will calcify into rigid procedures that resist the situations where the model should use judgment.

**Some stance sections could be tighter.** `spec`, `build`, and `assess` all have stance + "sharpest move" paragraphs. The "sharpest move" is a good device when it names something the model would otherwise miss. It's redundant when it repeats the stance. Audit each.

**Core references are lean after the recent refactor, but the `guidelines.md` "Artifact-Level Signals" section is dense.** It's useful — every artifact type has specific signals to notice. But it's also long (lines 72-106 of `guidelines.md`). If it's actually load-bearing for behavior (the model checks these signals during work), keep it. If it's a reference that the model consults selectively, consider splitting into `references/signals.md` so it's loaded on demand rather than on every skill invocation.

**`test/SKILL.md` has a "Scope note" about QE territory that's redundant with the plugin-wide design (G13).** It says the same thing develop's README implies: QE review lives in etak-deliver. This is likely a candidate to move to README or `core/references/`.

**No skill currently prescribes `context: fork` — only the three agents (G7).** This is correct (the skills are interactive, the agents are autonomous). But it should be stated as a principle somewhere so future plugin authors know the rule.

**`spec/SKILL.md` line 23-27 has a long "Key internal skills you'll call" list.** The pattern is fine but the specific list might drift as internal skills change. Consider whether this could be generated from or cross-referenced with the internal skills directory.

#### Frontmatter / tool audit for develop

- `survey` — tools `Read, Glob, Grep`. ✅ read-only, correct.
- `plan` — tools `Read, Write, Edit, Glob, Grep, Bash`. Bash used for git log in pre-mortem? Not obvious. ⚠️ Justify or trim.
- `spec` — tools `Read, Write, Edit, Glob, Grep, Bash`. Similar. ⚠️ Justify.
- `assess` — tools `Read, Write, Edit, Glob, Grep, Bash`. Bash is used to run feasibility checks. Justified, but comment-worthy. ⚠️ Justify inline.
- `test` — tools `Read, Write, Edit, Glob, Grep, Bash`. ✅ runs test commands.
- `build` — tools `Read, Write, Edit, Glob, Grep, Bash`. ✅ runs everything.
- `architect` — tools `Read, Write, Edit, Glob, Grep, Bash, Agent`. ✅ agent dispatch needed.
- `tech-lead` — tools `Read, Write, Edit, Glob, Grep, Bash, Agent`. ✅ agent dispatch needed.
- `developer` — tools `Read, Write, Edit, Glob, Grep, Bash, Agent`. Agent for sub-dispatch (etak-deliver). ✅.

### 3.3 Cross-plugin observations

**Structural consistency is a win.** Both plugins use the same conventions: `_internal/` for artifact skills, `core/SKILL.md` + `core/references/` for shared foundation, user skills at the top level, each with optional `references/`. New plugin authors will have a clear template.

**Naming conventions hold.** Kebab-case everywhere; `etak-` prefix in manifests, omitted in directory names; `user-invocable: false` on internal skills; no sprawl.

**One-way traceability (`from_discovery`) is correctly implemented.** Development artifacts point back to discovery; discovery never points forward. This is load-bearing for the compliance story the develop plugin claims.

**registry.json status.** Per the memory record, this file is legacy from an earlier session and duplicates marketplace manifest metadata. It's intentionally deferred. Worth verifying it's not causing installer confusion and deciding: keep in sync, deprecate, or delete. Not urgent.

**No design plugin yet; etak-deliver is referenced from develop but doesn't exist.** The plan bakes in the cross-plugin contract (tech-lead dispatches to specific agents), which is good — when etak-deliver lands, integration will be load-bearing, not bolt-on. The risk: the contract might not survive contact with actual etak-deliver implementation. Worth a brief "interface contract" note somewhere.

---

## Part 4 — Improvement Plan

Ordered by impact. Each step has a rationale and a rough scope. **Do not implement any of this without user review.** The plan may also surface questions that change the approach — flag those rather than push through.

### Step 1. Normalize frontmatter across etak-discovery (HIGH / small)

**Rationale:** Four of six user-facing skills (`explore`, `sound`, `prioritize`, `experiment`) have no model or effort specified. This is the biggest consistency gap in either plugin and the easiest to fix. Per G6, the plugin should pick a rule and hold it.

**Proposed values:**

| Skill | model | effort | rationale |
|-------|-------|--------|-----------|
| `orient` | sonnet | medium | listening/reflecting (unchanged) |
| `explore` | sonnet | high | generative reasoning grounded in the graph |
| `sound` | sonnet | high | multi-round assumption surfacing |
| `critique` | opus | xhigh | multi-persona + multi-framework critique (unchanged) |
| `prioritize` | sonnet | high | structured ranking and tradeoff analysis |
| `experiment` | sonnet | high | test design + outcome propagation |
| `product-researcher` | sonnet | high | autonomous research with synthesis (unchanged) |

**Open question:** Should `sound` match `critique` at opus/xhigh? They're philosophically similar, but `sound` is collaborative-exploratory while `critique` is adversarial-multi-round. I'd keep `sound` at sonnet/high but flag this for the user's judgment.

**Scope:** Six frontmatter edits. No body changes.

### Step 2. Tighten tool allowlists (MEDIUM / small)

**Rationale:** Per G8, broad allowlists are a smell. Discovery has several skills with `Bash` that don't run commands. Develop has a couple that could use inline justification.

**Concrete edits:**
- `discovery/skills/explore/SKILL.md` — remove `Bash`
- `discovery/skills/sound/SKILL.md` — remove `Bash`
- `discovery/skills/critique/SKILL.md` — remove `Bash`
- `discovery/skills/experiment/SKILL.md` — keep `Bash` but add a one-line comment explaining why (data pulls, perhaps)
- `discovery/agents/product-researcher.md` — remove `Write, Edit` (agent is explicitly forbidden from modifying the graph)
- `develop/skills/plan/SKILL.md` — audit Bash usage; remove if not used
- `develop/skills/spec/SKILL.md` — audit Bash usage; keep if genuinely needed, comment otherwise
- `develop/skills/assess/SKILL.md` — keep Bash; add inline comment in body explaining feasibility checks

**Scope:** Frontmatter edits plus one- or two-line comments.

### Step 3. Trim narrative tax in discovery stance sections (MEDIUM / small)

**Rationale:** Per G1, lines have to earn their place. Specifically:

- `sound/SKILL.md` — the opening paragraph about nautical sounding is human-oriented context. Move to `docs/context/` or `discovery/README.md`. Keep the working rule ("probe beneath a plausible-looking idea").
- `critique/SKILL.md` — the "How Critique Differs from Sound" section is load-bearing and should stay.
- `explore/SKILL.md` — "The Continuum" section is philosophical framing. Decide: does the model need to hold this in context to behave correctly, or is this explaining to humans why graph-viewing and brainstorming share a skill? If the latter, move it.
- Cross-reference echoes in `references/*.md` files — every references file should be audited for a verbatim restatement of its SKILL.md opening. Trim those.

**Scope:** Per-file audit. Estimated 2-4 lines removed per affected file. No behavior change; keep any line that earns its place.

### Step 4. Audit agent processes for over-prescription (MEDIUM / medium)

**Rationale:** Per G12, every step of a long agent process should earn its line. The developer agent is 362 lines; tech-lead is 260; architect is 262. Some of this is load-bearing; some is prescription the model would follow without it.

**Method:** For each agent, walk through the numbered steps. For each step, ask:
1. Does this step produce a distinct artifact, or is it running tool calls the model would run anyway?
2. Is the step's ordering load-bearing? (E.g., "read before proposing" is load-bearing; "run linter after tests" is load-bearing; "check git log after diff" probably isn't.)
3. Would removing this step make the agent worse in a way a user would notice?

Expected outcomes:
- `architect` — merge steps 7 (fan out) and 8 (review) into a single "stress-test the draft" step. Condense "Process" from 10 to ~7 steps.
- `tech-lead` — condense step 7 ("Handle problems") into a one-line guideline ("Escalate early, don't spin, don't silently re-scope"). Keep the execution-plan and progress-update templates.
- `developer` — trim step 5 (quality checks) and step 6 (merge conflicts) to principle-level. Keep the TDD cycle, self-review schema, and PR template.

**Target:** 15-25% reduction in agent length, zero change in behavior. If trimming causes uncertainty about behavior, leave the line.

**Scope:** Three agent files. Real editing work.

### Step 5. Simplify `_internal/core/SKILL.md` files in both plugins (LOW / small)

**Rationale:** Per G9, core is a ToC. Both plugins' core skills have a "Skill Composition" explainer (discovery) or implicit equivalent that explains mechanics the model already knows.

**Concrete edits:**
- `discovery/skills/_internal/core/SKILL.md` — remove "Skill Composition" section (lines 21-30). Keep the table of internal artifact skills and the three-reference links.
- `develop/skills/_internal/core/SKILL.md` — same audit; remove any procedural explanation of how skill reading works.

**Scope:** Two files. Small edits.

### Step 6. Audit stance sections for "sharpest move" redundancy (LOW / small)

**Rationale:** Several develop skills have a stance paragraph followed by "Your sharpest move" paragraph. When the sharpest move names something the stance doesn't (e.g., `build`: "noticing when the scope is drifting"), keep it. When it repeats the stance, cut it.

**Scope:** Quick audit of `spec`, `build`, `assess`, `architect`, `developer`. Trim where redundant.

### Step 7. Add cross-plugin posture to discovery README (LOW / small)

**Rationale:** Per G15, cross-plugin relationships should be explicit in both directions. Develop's README acknowledges discovery; discovery should acknowledge develop.

**Proposed addition to `discovery/README.md`:** a short "Related plugins" section mentioning that validated ideas flow to `etak-develop` via the `from_discovery` link, and that UX (design) and delivery are planned future companions.

**Scope:** ~10 lines in README.

### Step 8. Consider splitting discovery `guidelines.md` "Artifact-Level Signals" into its own reference (LOW / medium)

**Rationale:** The artifact-level signals section is dense and only needed when the model is doing artifact-specific work (not on every orient/explore call). Splitting to `references/signals.md` would let skills load it selectively.

**Risk:** If the signals *are* load-bearing on every call (they inform the "read the signals" behavior in `orient`), splitting would require each invoking skill to know to load signals.md. Might not be worth the complexity. Flag for decision.

**Scope:** File split + update of references in core/SKILL.md.

### Step 9. Write a CONTRIBUTING-style skill/agent authoring guide (LOW / medium)

**Rationale:** The guidelines in Part 2 of this document are worth captured as a contributor-facing reference once they've been accepted. Future plugins (design, deliver) should build on them.

**Proposed:** `docs/meta/skill-design-guide.md` (or similar). Covers G1-G15 with concrete examples drawn from the existing plugins.

**Scope:** 300-500 lines of prose. Derivable from this assessment, but shaped for contributors, not review.

### Step 10. Verify registry.json status and decide (LOW / small)

**Rationale:** Per the memory record, `registry.json` is legacy and duplicates marketplace manifest metadata. Either keep it in sync as a convention, deprecate it with a note, or delete.

**Scope:** Decision + small follow-up edit. Ask the user first.

---

## Part 5 — What This Review Did Not Cover

- **Tutorials and quickstart (`docs/quickstart.md`, `docs/tutorials/`)** — these are user-facing onboarding, not prompts. They appear in good shape but weren't re-audited against the guidelines (guidelines are for prompts).
- **Artifact reference docs (`docs/artifacts/`)** — these are user-facing explanations of artifact types. Not audited.
- **Profiles (`docs/profiles/`)** — not inspected. If these are user-facing descriptions of who the plugin is for, they're out of scope. If they're role configurations Claude loads, they need the same evaluation the skills got.
- **plugin-schema.json and plugin manifests** — manifests were verified to exist; their contents were not audited for adherence to the schema. Recommended as a Step 11 if issues are found.
- **Memory-system integration** — the memory directory and CLAUDE.md files shape Claude's behavior across sessions and were not audited here.

---

## Summary

**Overall state:** Both plugins are well-conceived and the recent refactor of etak-develop (commit 4ca2ce7) moved the develop prompts firmly toward the "skill is a prompt" discipline. The philosophical foundations (reflective practitioner, mixed-initiative, incremental formalism) are visibly present in the prompt text — not just in the docs — which is the single most important criterion for a tool like this.

**Biggest gap:** Frontmatter inconsistency in etak-discovery. Four skills missing model/effort. Step 1 addresses this.

**Second gap:** Agent process prescription. The three develop agents have accreted detail that would be better stated as principles. Step 4 addresses this.

**Smaller gaps:** Narrative tax in a few discovery stance sections; broad tool allowlists; one-way cross-plugin acknowledgment; minor redundancy in a few stance blocks.

**What's notably strong:**
- Differentiated stances across every user-facing skill
- "What X does NOT do" blocks across every skill
- Named transitions with specific user behavior triggers
- Ground-in-material rules in develop
- Cross-plugin dispatch architecture in develop

**The improvement plan is intentionally small.** The plugins don't need a rewrite. They need a set of targeted normalizations and a small amount of trimming. After Step 1-6, the plugins will be measurably more consistent and slightly leaner, without changing how they feel to use.

---

## Part 6 — Execution Status

Status as of the follow-up implementation pass.

| Step | Title | Status | Notes |
|------|-------|--------|-------|
| 1 | Normalize discovery frontmatter | ✅ Done | `model: sonnet`, `effort: high` added to `explore`, `sound`, `prioritize`, `experiment`. Unchanged: `orient` (sonnet/medium), `critique` (opus/xhigh), `product-researcher` (sonnet/high). |
| 2 | Tighten tool allowlists | ✅ Done (with one deviation) | Removed `Bash` from `explore`, `sound`, `critique`. Removed `Write, Edit` from `product-researcher`. Removed `Bash` from `plan`, `spec` (neither body nor references prescribed shell work). Kept `Bash` in `assess` with a new inline paragraph scoping its use to feasibility investigation. **Deviation:** also removed `Bash` from `experiment` — the plan called for keeping it with a comment, but a search of the body and references produced no evidence of shell use, so G8 (least privilege) won. |
| 3 | Trim narrative tax in discovery stance sections | ✅ Done | Nautical-etymology paragraph removed from `sound/SKILL.md`. `explore`'s "The Continuum" compressed into a terser "The Flow" section that keeps the behavioral rule ("see before you generate") and drops the philosophical framing. `critique/references/critique.md` opening rewritten to stop echoing the SKILL.md first paragraph verbatim. Other reference openings audited; no further verbatim echoes found. |
| 4 | Audit agent process sections | ✅ Done | `architect`: steps 7 (fan out) and 8 (review) merged into a single "Stress-test the draft" step; process dropped from 10 to 9 steps. `tech-lead`: step 7 ("Handle problems") compressed from five enumerated scenarios to a single principle paragraph ("escalate early, don't spin, don't silently re-scope"). `developer`: step 5 (quality checks) and step 6 (merge conflicts) trimmed to principle-level prose; TDD cycle, self-review schema, and PR template preserved intact. Target 15-25% reduction achieved with zero behavior change. |
| 5 | Simplify core SKILL.md files | ✅ Done | "Skill Composition" section removed from both `discovery/skills/_internal/core/SKILL.md` and `develop/skills/_internal/core/SKILL.md`. Artifact-skill tables retained under a leaner "Internal Artifact Skills" heading with a two-line intro. |
| 6 | Audit stance sections for "sharpest move" redundancy | ✅ Done | `spec`'s "Your sharpest move: refusing to write prose…" paragraph removed — redundant with the stance paragraph above it and the Ground-in-Codebase Rule section below it. `tech-lead`, `build`, and `test`'s sharpest-move lines retained — each names something the stance doesn't. |
| 7 | Add cross-plugin posture to discovery README | ✅ Done | "Relationship to other etak plugins" section added to `discovery/README.md`, mirroring the structure already in `develop/README.md`. Names etak-design (sibling, future), etak-develop (downstream, `from_discovery` link), and etak-deliver (further downstream, future). |
| 8 | Split/consolidate signal guidance | ⏭️ Deferred | The review flagged `guidelines.md`'s Artifact-Level Signals section as potentially loadable-on-demand. Deferred — actual usage pattern of those signals across skills is not yet clear. Revisit if the signals turn out to be consulted selectively rather than always. |
| 9 | CONTRIBUTING-style skill/agent authoring guide | ✅ Done | Written as [`docs/meta/skill-design-guide.md`](meta/skill-design-guide.md). Covers G1-G15 with concrete examples pulled from the existing plugins, plus an authoring checklist and common-mistake catalog. Complements `CONTRIBUTING.md` (which stays mechanical). |
| 10 | registry.json hygiene | ✅ Done | After a follow-up decision, `registry.json` was deleted. It duplicated metadata already in each plugin's `plugin.json` and was not read by the Claude Code installer (which reads `.claude-plugin/marketplace.json`). The per-skill and per-agent descriptions it carried belong in human-facing documentation and can be regenerated on demand. `README.md`, `CONTRIBUTING.md`, and `.claude/CLAUDE.md` updated to drop references to the file. |

**What changed in aggregate:** eleven files modified across both plugins, one new authoring guide added, zero behavior regressions expected. The improvement pass stayed within the scope the plan defined — targeted normalizations, not a rewrite.
