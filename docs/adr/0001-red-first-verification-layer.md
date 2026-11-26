---
status: accepted
---

# The Verification Layer is written red-first, before execution

`/cross-verification` builds the Verification Layer for a plan from its demos and specs before any of the plan is executed, and every new Check must fail at that point because the behavior is missing. We chose this because a Check that has never been seen failing for the right reason has not been shown to distinguish the current state from the final state, and because writing Checks from the approved demos, rather than from the code being built, keeps the executor from fitting the Checks to its own implementation.

## Considered Options

- **Checks written alongside implementation.** Faster feedback per change, but the executor writes the Checks with its implementation in mind, so they tend to prove what was built rather than what was approved.
- **Checks written after implementation.** Cheapest upfront, but nothing proves a passing Check would ever have failed.

## Consequences

- Upfront cost before any feature work, and a judge step to confirm coverage.
- Existing Checks that assert behavior the plan changes must be altered or removed up front, which is only safe under the rule that a Check is never weakened.
