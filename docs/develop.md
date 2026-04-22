# Develop

The `etak-develop` plugin turns validated ideas into shipped software. It is
the engineer's thinking partner: a tech lead in the terminal. You describe
the move you want to make, it picks the right skill and pairs with you.

Where discovery is exploratory — generative, receptive, adversarial —
development is executional. It still runs on the same idea: describe what
you want, the tool routes. You do not need to learn skill names.

## The six skills

| Skill | Stance | The move it supports |
|-------|--------|----------------------|
| **survey** | Receptive | Home base — navigate the work graph, see what's ready, blocked, or stalled |
| **plan** | Structural | Scope validated ideas, decompose into workstreams and epics, sequence milestones |
| **spec** | Pragmatic | Grounded technical design — specs and ADRs against the actual codebase |
| **assess** | Evaluative | Readiness, feasibility, sizing, critique — before committing to build |
| **test** | Rigorous | Strategy-first testing — plan from ACs, write at the right layer, debug flakes |
| **build** | Collaborative | The inner loop end to end — TDD pairing, self-review, PR, feedback |

## The rhythm

Development is not a linear waterfall. The skills form a loop you return
to at every scale:

**Plan → spec → assess → build (with test) → back to plan for the next
slice.**

You scope and sequence (plan), ground the design (spec), stress-test
before committing (assess), and implement with care (build + test). When
one slice lands, you look at the graph again (survey) and pick the next
move.

## How to choose a move

Match the skill to the state of the work:

- **You have a validated idea that needs structure** → plan
- **You have a story and need to decide how to build it** → spec
- **You have a story or spec and want to know if it holds up** → assess
- **You want to know what's ready to build and what's blocked** → survey
- **You're about to write tests** → test
- **You're about to write code** → build

Describe what you want in plain language. The tool routes.

## The three agents

Agents do longer-running autonomous work. You hand them a body of work
and review the result.

- **architect** — produces a complete technical design for a project or
  epic. Reads the codebase deeply, considers alternatives, drafts spec +
  ADRs, reviews through multi-lens critique. Use when you want a proposal
  to review, not to co-author.
- **tech-lead** — orchestrates delivery. Takes a project, fills gaps
  (specs, readiness checks, test plans), creates an execution plan,
  dispatches other agents. Parallelizes independent work. Operates at the
  cadence you choose: story, milestone, or full auto.
- **developer** — autonomous implementation in isolated worktrees. Picks
  up a ready story (or bug, or spike), works through it with TDD, creates
  the PR. Honest self-review at the end — if an AC isn't satisfied, it
  says so.

## The artifacts

Development work is tracked as a graph of typed artifacts under
`docs/development/`:

- **Initiative / Project / Epic / Story / Task** — the hierarchy. One
  owner, one parent.
- **Workstream / Milestone** — parallel tracks and shipping checkpoints
  that cut across the hierarchy.
- **Spec / ADR** — technical design and hard-to-reverse decisions.
- **Spike** — time-boxed investigation to answer a question.
- **Bug / Chore / Enhancement** — standalone work items.

Every development artifact can carry a `from_discovery` field that links
back to a validated idea in `docs/discovery/`. Discovery never points
forward — development owns traceability. That matters when compliance
auditors ask "why was this change made?"

## Relationship to etak-discovery

`etak-discovery` validates *what to build*. `etak-develop` decides *how to
build it and ships it*.

Validated ideas in `docs/discovery/` flow into development via the
`from_discovery` link on the artifact that takes up the work (usually an
initiative, project, or story). The link is one-way: development reads
discovery context, but discovery never reaches forward into execution
state.

## Future

- **etak-design** (planned) — UX and interaction design. Informs stories
  and specs; distinct from technical design.
- **etak-deliver** (planned) — downstream quality, operations, release.
  Code review, QE verification, CI/CD, security audits, release notes,
  docs updates. `tech-lead` is already wired to dispatch its agents
  (quality-engineer, devops, security-lead, reviewer, tech-writer) when
  they're installed.

## Read next

- [**Skill definitions**](../develop/skills/) — each skill's full
  specification.
- [**Agent definitions**](../develop/agents/) — each agent's full
  specification.
- [**Discovery reference**](skills/) — the upstream plugin.
