# Decomposition

Decomposition turns a project into workstreams, epics, and the relationships
between them. The goal is *parallelizable* work with clear integration points —
not just work broken into smaller pieces.

## Finding the natural seams

A workstream is a track of work that can progress independently of other tracks
*between* integration points. Good seams are:

- **Frontend / Backend** — classic, works when the API contract is the seam
- **Service boundaries** — one workstream per service when services are
  independently deployed
- **Data / Application** — when a schema or pipeline change gates app work
- **Read path / Write path** — in systems where ingest and serve have different
  owners
- **Internal / External** — when external integration has different risk profile

Bad seams (contrived or org-chart based):

- "The senior eng's stuff" vs. "the junior eng's stuff"
- "Interesting work" vs. "boring work"
- Splits that mirror team membership rather than system architecture

When seams are unclear, ask: if workstream A shipped and workstream B didn't,
would A be useful? If the answer is no, A isn't really independent — it's
waiting on B and the seam is in the wrong place.

## Interface contracts

Every cross-workstream interaction needs a contract. At minimum:

- **The API, schema, or protocol.** Testable text, not aspirational prose.
  "POST /auth/token returns JWT with claims {sub, exp, iat}" is a contract.
  "We'll coordinate on authentication" is not.
- **Which workstream owns it.** One and only one.
- **Who consumes it.** Everyone else should be listed.
- **When it must be ready.** Tied to a milestone.

Write interface contracts as testable statements in the workstream's
`interface_contracts` field. When the producing workstream ships, the
contract should be verifiable by running code.

## Identifying epics

Within each workstream, identify the epics it owns. You don't have to have all
stories ready, but you should know what capabilities the workstream will
produce.

Rules of thumb:

- An epic should fit within one or two milestones
- If an epic naturally spans workstreams, either split it or retheme
- If a workstream has one epic, you're probably over-decomposing
- If a workstream has ten epics, you may be under-decomposing (is this really
  a project?)

## Ownership

Workstreams need owners — ideally named people, at worst a team. Unowned
workstreams don't make progress; they get dropped.

Owner can be deferred to planning, but should be named before M1.

## Common failures

- **Workstreams with no contracts.** Integration chaos. Postpone start until
  contracts exist.
- **Serial dependencies disguised as parallelism.** If every milestone requires
  all workstreams to ship in lockstep, the parallelism is fake.
- **Too many workstreams.** Each workstream adds coordination cost. Three
  workstreams with clear seams beats six that blur into each other.
