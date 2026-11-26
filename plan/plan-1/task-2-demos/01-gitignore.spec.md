# Spec: Ignore plan/

## Behaviors
- **Given** `.gitignore` contains `plan/`, **when** you run `git check-ignore plan/plan-1.md`, **then** it prints `plan/plan-1.md`.
- **Given** `.gitignore` contains `plan/`, **when** you run `git status`, **then** nothing under `plan/` is listed as untracked.

## Rules
- `.gitignore` contains exactly one line: `plan/`.
- No other file is changed.

## Examples
| Input | Expected output |
|-------|-----------------|
| `cat .gitignore` | `plan/` |
| `git check-ignore plan/plan-1/task-1.md` | `plan/plan-1/task-1.md` |

## Not shown in the demo
n/a
