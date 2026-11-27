---
status: accepted
---

# A fresh judge runs every verification during execution

During `/cross-execute`, every run of Checks is carried out by a fresh judge agent that has no knowledge of the reasoning of the agent that did the work. The judge runs the Checks, compares the result with the Baseline and the Target Checks, and diagnoses each failure, but never changes code. The agent that did the work never reads the Checks or their output; it only receives the judge's verdict and does the fixing. We chose this because an agent that grades its own work tends to explain failures away, and an agent that can read the Checks tends to fit its code to them instead of to the approved behavior, which is the same risk ADR 0001 removes from the writing of Checks.

## Considered Options

- **The agent that did the work runs its own Checks.** Cheapest and fastest, with no extra agent per run, but the same agent decides whether a failure is "only because the work is unfinished", and it can read the Checks while writing the code.
- **Self-check during the work, independent judge only at the end.** Cheaper than a judge per run, but problems are found late, after other work has been built on them.

## Consequences

- Every verification costs an extra agent, which counts toward the run's agent cap.
- The agent doing the work depends on the quality of the judge's diagnosis, since it cannot look at the failing Check itself.
- The judge must be given facts and paths only, never an explanation from the agent whose work it grades.
