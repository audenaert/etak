# Plan Tests from ACs

Acceptance criteria are test specifications. The plan move converts them
into concrete test cases before a line of test code is written. It's the
cheapest point to catch ambiguous ACs — because the ambiguity becomes
a "🔍 gap" marker, not a mid-implementation question.

## Process

### 1. Load the story

Load the story and its ACs. Also load:
- The parent epic for scope context
- Any linked spec (it may already specify edge cases)
- Adjacent stories (for patterns — what did *they* test?)

### 2. Derive test cases per AC

For each AC, produce 1+ concrete test cases with:
- **Marker:** ✅ happy path, ⚠️ edge case, 🔍 gap
- **Recommended layer:** unit / integration / API / E2E
- **One-line test name** that reads as behavior

### 3. Add supplementary tests

ACs rarely cover everything testable. Add:
- **Error paths** — what the user sees when the operation fails
- **Boundary conditions** — empty inputs, max values, zero, null
- **Regression guards** — bugs this code has had before, if known

### 4. Present the plan

```
## Test Plan: [story name]

### AC 1: "Learner can see their review schedule"
- ✅ Displays upcoming reviews sorted by due date (E2E)
- ✅ Shows correct count of pending reviews (Unit — scheduling logic)
- ⚠️ Empty state when no reviews are scheduled (E2E)
- 🔍 Gap: AC doesn't specify date format or timezone handling

### AC 2: ...

### Supplementary
- Error: API returns 500 → user sees error state (E2E)
- Boundary: learner with 100+ pending reviews (Unit — pagination)
```

If the user just wants the plan, stop here. If they want to proceed to
writing, move to the write move.

## Picking the layer

| What you're testing | Primary layer | Why |
|---|---|---|
| Pure logic, transformations | **Unit** | Fast, isolated, precise |
| A function's public behavior | **Unit** | Test through the public interface |
| Components interacting across a boundary | **Integration** | Real dependencies |
| Database queries | **Integration** | Needs a realistic database |
| HTTP API contracts | **API** | Verify what consumers see |
| Auth flows on endpoints | **API** | Real middleware chain |
| Critical user journeys | **E2E** | Full stack, real browser |
| UI interactions, form flows | **E2E** | Needs a real browser |

When multiple layers are defensible, pick the **cheapest** one that exercises
what you actually care about.

## Guidelines

- **Gaps are findings.** If you can't derive a clear test from an AC, flag
  🔍 and surface the gap. Don't invent a test for what you assume the AC
  means.
- **Each test earns its place.** At higher layers (integration, E2E),
  justify why. "One critical user flow" is a reason; "for coverage" is not.
- **Don't over-test.** Testing the same behavior at three layers is waste.
  Pick one.
- **The plan is cheap.** Expect to revise it after the first few tests reveal
  what the code actually needs.
