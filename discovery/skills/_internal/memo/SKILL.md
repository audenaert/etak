---
name: memo
description: >
  Create and update memos — sustained analytical artifacts (frameworks, surveys,
  literature reviews, structured arguments) that inform the opportunity space.
  Handles writing the file, status transitions, and the supports list. Internal
  skill called by the product-researcher agent and any other producer of
  analytical work, never invoked directly by users.
user-invocable: false
allowed-tools: Read, Write, Edit, Glob, Grep
---

# Memo

You handle the artifact mechanics of memos — writing the file, updating status,
and managing the `supports` list. The calling skill or agent does the analytical
work; you persist the result.

Read [the core foundation](../core/SKILL.md) for schemas and interaction guidelines.

## What Makes a Good Memo Artifact

- **One coherent analytical frame per memo.** A memo trying to cover everything
  is a memo that needs splitting. Push the caller to scope when a memo sprawls.
- **Format serves the argument.** A framework memo has structured sections and
  tables. A worked-through problem may be discursive. Both are fine; do not
  impose a template.
- **`supports` is optional but valued.** Foundational memos may not link
  anywhere — that is legitimate. But if a memo informs a specific opportunity or
  objective, the link should be there. Ask the caller.
- **Status reflects standing.** `draft` means in flux or under review. `active`
  means you would stand behind this today. `archived` means superseded — kept for
  the record, not for current reference.

## Moves

### Write a new memo

The caller arrives with analytical content (a framework, a survey, a synthesis)
and the slugs (if any) of artifacts the memo informs.

Generate a kebab-case filename from the memo's topic — e.g.,
`graph-theoretic-structure-of-opportunity-space.md`. Write to
`docs/discovery/memos/`. Frontmatter:

- `name` — short descriptive title
- `type: memo`
- `status` — usually `draft` when first written, `active` when the caller is
  publishing finished work
- `supports` — list of objective or opportunity slugs the memo informs; omit the
  field entirely if the memo is foundational and links to nothing

Body: free-form analytical content, in whatever shape the caller has produced.
Do not reformat substantive content. Light cleanup (consistent heading levels,
fixed markdown) is fine; rewriting is not.

Always show the artifact before writing.

### Update status

`draft → active → archived`. Each transition is a deliberate move:

- **draft → active**: the memo is finished and available for reference. The
  caller (usually the PM) is the one who promotes — agents typically write
  drafts.
- **active → archived**: the memo has been superseded by later work or is no
  longer relevant. Record the reason in the body — what replaced it, or why it
  is no longer the team's view.
- **draft → archived** is rare but valid (the analysis was abandoned). Note why.

Edit the artifact in place — update `status` in the frontmatter.

### Update the supports list

Memos accumulate links over time. As specific opportunities and objectives
emerge from a foundational memo, the caller may ask to add slugs to `supports`.
Conversely, an opportunity that's been removed from the discovery graph should
be removed from the memo's `supports`.

Edit the frontmatter in place. If the list becomes empty, omit the field
entirely rather than leaving `supports: []`.

### Validate links

When asked, glob `docs/discovery/objectives/` and `docs/discovery/opportunities/`
to confirm each slug in `supports` resolves to a real artifact. Report stale
links so the caller can decide whether to remove or repoint.

## Failure Modes

- **Memo without analytical depth.** If the content is a single observation or a
  passing thought, it is not a memo — it is a note that belongs in an
  opportunity's evidence section or in conversation. Push back.
- **Multiple analytical frames in one memo.** If the memo covers two
  unrelated arguments or surveys, suggest splitting before writing.
- **`supports` slug does not resolve.** Report and ask the caller to fix or
  remove.
- **Caller wants to silently overwrite an `active` memo.** Active memos are the
  team's standing reference. Editing in place is fine for typos and small
  clarifications; substantive revisions should either move the memo to `draft`
  during revision, or archive the current version and write a new one.
