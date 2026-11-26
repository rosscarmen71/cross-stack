---
status: accepted
---

# A plan's Permissions cover how code is edited, not how it ships

A plan's `Permissions` section only grants or restricts how the code may be changed: which files, what kinds of edit, installing dependencies, destructive edits. Shipping concerns (commits, branches, worktrees, pushing, opening or merging PRs) are executor settings chosen in `/cross-execute` step 0, never in the plan. We chose this because a plan describes what the final state is and what may be touched to reach it, while how and when work is shipped depends on the run (who is executing, how autonomous it is, how parallel it runs), and the same plan should be executable under different shipping strategies without being rewritten.

## Considered Options

- **The plan controls shipping, step 0 fills the gaps.** Lets the user pin shipping rules once per plan, but ties every run of the plan to one shipping strategy and splits shipping rules across two places.
- **Both can set shipping, the plan wins on conflict.** Most flexible, but the reader has to check two places to know how a run will ship.

## Consequences

- The `Permissions` note in `cross-plan/templates/plan.md` and the "add any explicit permissions" goals in `cross-plan/SKILL.md` must say that shipping rules do not belong in the plan.
- `/cross-execute` step 0 must offer the shipping and branching settings, with defaults.
