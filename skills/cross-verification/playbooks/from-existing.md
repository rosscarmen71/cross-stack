## Verification Layer From The Existing Layer

Extend and adjust the project's existing Verification Layer to cover the given plan and/or Tasks.

**You own the design, plan, implementation, review and verification.** Your scope is restricted to altering the Verification Layer. Follow `principle-never-weaken-a-check` on every decision about an existing Check.

### 1. Inventory The Existing Layer

GOALS:
* Know what the existing Checks prove, how they run and where their output goes.

STEPS:
1. List the existing Checks, the framework and runner, the run commands, and where output goes.
2. Run the existing layer once and record which Checks pass. This is the baseline for the red-first check in step 6 of the skill.
3. If a Coverage Map for this plan already exists, open it. Otherwise create `<verification home>/plan-<id>-coverage.md` as in `./from-scratch.md` step 1.3, using the existing framework.

### 2. Create The Verify Skill If Needed

GOALS:
* Every visual Check has a way to drive the real app.

STEPS:
1. Follow `./from-scratch.md` step 2 exactly.

### 3. List Every Check

GOALS:
* The full list of Checks the plan needs, before looking at what already exists.

STEPS:
1. Build the list exactly as in `./from-scratch.md` step 3. It does not matter yet whether a Check already exists somewhere in the layer.

### 4. Match Against The Existing Layer

GOALS:
* No Check is duplicated, no existing Check contradicts the final state, and every reuse is traceable.

STEPS:
For each Check on the list, and for each existing Check that touches behavior the plan changes:
1. **Already covered.** An existing Check already proves the item exactly. Do not write a new one. Mark the entry `existing-cited` and record the existing Check in the Coverage Map. Confirm it by reading the Check's assertions, not its name. An existing Check counts only if it is cited in the map.
2. **Near match.** An existing Check almost proves the item. Mark the entry `alter`, edit that Check in place to assert the item exactly, and never copy it into a new file. The edit must make the Check stricter or red now, never easier to pass.
3. **Contradicts the final state.** An existing Check asserts *current* behavior that the plan changes (for example it expects a button labelled "Sign in" and the plan renames it "Log in"). Mark it `alter` to assert the new behavior, which makes it red now. If the plan removes the behavior entirely, mark it `remove` with the `final verification` item or `current delta` line that removes it.
4. **Not covered.** Nothing existing fits. Mark the entry `add`.
5. Never mark an existing Check `alter` or `remove` only because it fails, is slow or is inconvenient. Only a change in the asserted behavior, cited from the plan, justifies it.

### 5. Return

Return the **Check list**: every entry marked `add`, `guard`, `alter`, `remove` or `existing-cited`, each with its `final verification` item, Task, kind and Evidence location, plus any `uncovered` items.
