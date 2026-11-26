## Verification Layer From Scratch

Create the first Verification Layer for this project, covering the given plan and/or Tasks.

**You own the design, plan, implementation, review and verification.** Your scope is restricted to creating the Verification Layer.

### 1. Set Up The Layer

GOALS:
* The verification home, the Evidence folder and an empty Coverage Map exist.

STEPS:
1. Confirm the verification home and the Evidence folder chosen in step 1 of the skill exist, and that the Evidence folder is ignored by git.
2. Pick the test framework. Use the one the project's language and tooling already imply (for example the runner its package manager or build tool ships with). If nothing is implied, pick the most standard runner for the language and say why in the Coverage Map's header.
3. Create `<verification home>/plan-<id>-coverage.md` with a header (plan id, plan path, framework, whole-layer run command), one `Plan` table for plan-level items, and one table per Task, each with the columns `item`, `check`, `run command`, `evidence`, `status`.

### 2. Create The Verify Skill If Needed

GOALS:
* Every visual Check has a way to drive the real app.

STEPS:
1. If the plan or any Task has a visual demo (interactive prototype, video or slides), or the project has a user-facing surface (UI, CLI, API), and no verify skill was found in step 1 of the skill, read `../../create-verification-skill/SKILL.md` and follow it. It cannot be invoked as a slash command by an agent. If its "fix the checkout first" step would mean editing product code, report the problem instead, since product code is outside this skill's scope.
2. Record the verify skill's path in the Coverage Map's header.
3. When the project has no running app, as with a repo of documents, the real surface is the rendered output, such as a README rendered locally. A visual Check drives that instead. If the real surface cannot be reached at all, for example because it needs a push the plan does not allow, mark the item `uncovered` with that reason.

### 3. List Every Check

GOALS:
* Every `final verification` item has at least one Check on the list.

STEPS:
1. For every `.spec.md` of the plan and of each Task, add one **automated Check** per `Behaviors` line, per `Rules` line and per `Examples` row.
2. For every visual demo, add one **visual Check** that drives the real app through the demo's flow with the verify skill and captures screenshots or a recording at the same points as the demo, so a reviewer can compare real and intended side by side.
3. For every line in a spec's `Not shown in the demo`, make sure an automated Check from step 3.1 covers it. Add one if not.
4. For every other `final verification` item (rules, non-negotiables with a **Verify by**), add the Check its **Verify by** describes, or an automated Check that asserts it.
5. **Merge duplicates.** When the same requirement appears in both the plan and a Task (for example identical specs), write one Check under the Task and cite it from the `Plan` table. Never write two Checks for one requirement.
6. **Mark guards.** A Check for an item that is already true now and must stay true is a guard Check, marked `guard`. Examples are "no other file is changed" and "this file stays unchanged".
7. Give every Check: its `final verification` item, its Task (or `plan` for plan-level items), its kind (automated or visual), whether it is a guard, a name, and its Evidence location `<evidence folder>/plan-<id>/<task-n or plan>/<check>/`.
8. Check that every `final verification` item of the plan and of every Task in scope appears at least once. Any item no Check can prove goes on the list as `uncovered` with the reason.

### 4. Return

Return the **Check list** from step 3, every entry marked `add` or `guard`, plus any `uncovered` items.
