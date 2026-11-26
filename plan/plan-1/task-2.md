# Sub-plan: Ignore plan/

**plan-id:** 1  ·  **task-id:** 1.2  ·  **Date:** 2026-11-25  ·  **Status:** approved

**dependencies**: none (can run in parallel with 1.1)


## Goal
Add `plan/` to the existing empty `.gitignore`, so plans made by `/cross-plan` in this repo aren't committed.

## Why
- **Why the user wants this:** not given (the user chose to skip it).
- **Why this result:** not given.
- **Why this approach:** not given.

## Out of scope
- Any file other than `.gitignore`.
- Adding any other ignore rule (for example `.DS_Store`).

## Permissions
- Inherited from plan 1. File scope: `.gitignore` only.



## Final State
`.gitignore` contains the single line `plan/`, and git ignores everything under `plan/`.

### Final Verification
- `plan/plan-1/task-2-demos/01-gitignore.spec.md`: the spec demo for this Task.
- `git check-ignore plan/plan-1.md` prints the path.
- `.gitignore` contains exactly one line: `plan/`.



## Current Delta
- `.gitignore` exists in the repo root but is empty (0 bytes).



## How 
- Append the single line `plan/` to the existing `.gitignore`. Don't add anything else.

### How out of scope
- Removing files that are already tracked under `plan/` (nothing is tracked there).



## Other Specs
- none
