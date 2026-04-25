# Size

Relative sizing. Story points, t-shirt sizes — the unit matters less than
the *comparison*. "This is 3x harder than that" is more useful than "this
will take 4 days," because it stays accurate when schedules change and
people change.

## Running the estimation

### 1. Load the items

Usually a set — sizing one item in isolation is weak. "Stories in the auth
epic" or "remaining M1 work" or "this story + its siblings."

### 2. Read the codebase

Same ground-in-codebase rule as feasibility. Without reading, you're
guessing:
- How much code changes for each item?
- Does it follow an existing pattern or break new ground?
- What's the test burden?

### 3. Pick a baseline

The smallest well-understood item becomes the reference. Everything else
sizes relative to it.

### 4. Assign sizes

| Size | Meaning |
|------|---------|
| **XS** | Trivial — follows an existing pattern exactly, < 1 hour |
| **S** | Small — well-understood, a few files, < half a day |
| **M** | Medium — some complexity, multiple files, ~1 day |
| **L** | Large — significant complexity, new patterns, 2-3 days |
| **XL** | Too large — split it |

XL is a **signal, not a size**. It means "this needs to be broken down."

### 5. Present ranked with justification

```
📖 Add login button (S) — follows button pattern in `components/`
📖 Implement OAuth callback (M) — new route, similar to existing webhook handler
📖 Token refresh logic (L) — no existing pattern, error handling is the hard part
📖 Multi-provider abstraction (XL) — split: provider interface + providers
```

### 6. Flag outliers

- Items much larger than siblings → suggest splitting
- Items with high uncertainty → suggest a spike, not an estimate
- Items dependent on unresolved decisions → note the estimate is conditional

## Guidelines

- **Relative, not absolute.** "3x harder" beats "4 days."
- **Estimates are communication, not commitment.** They help prioritize and
  catch scope surprises early.
- **If you can't estimate, that's the finding.** "Too vague to size" points
  at what needs refinement first.
- **Don't estimate spikes.** Spikes have time-boxes, not sizes.
- **Don't re-size without reading the code.** If the first estimation was
  grounded and nothing material has changed, trust it.
