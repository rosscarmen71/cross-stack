---
name: cross-verification
description: Create or extend a project's Verification Layer so that it passes only when the final state of a `/cross-plan` plan or Task is reached. Turns every `final verification` item, spec and demo into Checks, proves each new Check fails now for the right reason, and has an impartial judge confirm coverage.
---

# Cross Verification

Bundled skills live at `../<name>/SKILL.md` from this file, so put this file's absolute path in every prompt to an agent that you want to use this stack.

## Goal

Update the project's Verification Layer so that it passes if and only if the project reaches the final state described by the given plan and/or Tasks. Every `final verification` item has at least one Check. Every Check, visual ones included, leaves Evidence a human or an agent can review without re-running it. Every new Check fails now, before execution, because the behavior is missing. Checks are modular, so each Task can be verified as soon as it is built.

## Non-Negotiables

* **Only the Verification Layer.** Never edit product code, plans, Sub-plans or demos. The scope is Checks, their fixtures and helpers, the Coverage Map, and the verify skill.
* **Never weaken a Check.** Follow `principle-never-weaken-a-check` on every edit to an existing Check.
* **Every item is covered.** Every `final verification` item of the plan and of every given Task is in the Coverage Map with at least one Check, or is reported as uncovered with a reason.
* **Red first.** Every new or altered Check fails now for the right reason. Every untouched existing Check still passes. The one exception is a **guard Check**: a Check for an item that is already true and must stay true (for example "no other file is changed" or "the license file stays unchanged"). A guard Check passes now and must fail if the item is broken. Never make a Check fail artificially.
* **Reviewable.** Every Check writes Evidence to a named location.
* **Impartial judge.** Coverage is confirmed by a fresh judge that sees only files, never this agent's reasoning or history.

## Input

* **Required:** the path from the project root to each plan and/or Sub-plan to cover. A plan path alone means the plan **and every Task in its Road Map**. Sub-plan paths alone mean only those Tasks, plus any plan-level item they inherit.
* **Optional:** an *agent and autonomy settings* block from the caller (normally `/cross-execute` step 0) with any of:
  * `judge model`: the model the judge runs on.
  * `judge requirements`: extra instructions for the judge.
  * `dispute rule`: what to do when the judge disputes coverage after a fix.

If no settings block is given, the judge runs on the parent model with no extra requirements, and the dispute rule is: keep fixing and re-judging until the judge is satisfied.

## Output

```
<verification home>/                      ← where the project keeps tests, else project_root/test/
├── plan-<id>-coverage.md                 ← the Coverage Map for the plan
├── <checks, grouped by Task>             ← in the project's own framework and naming
└── <evidence folder>/                    ← the project's results folder, else test/evidence/ (never committed)
    └── plan-<id>/<task-n or plan>/<check>/  ← Evidence of the latest run of each Check
```

Plus the return summary from step 7.

## 0. Setup

GOALS:
* The steps of this skill are in the to-do list.
* The settings that govern the judge and disputes are known.

STEPS:
1. **Copy steps 0–7 of this skill into the to-do list tool** before doing anything else, and keep it updated as you go.
2. Read the settings block from the input. Write down the judge model, judge requirements and dispute rule, using the defaults from the Input section for anything missing.

## 1. Identify The Verification Layer

GOALS:
* Know where Checks, Evidence and the verify skill live, and whether a layer already existed.

STEPS:
1. Skim the project for existing verification: test folders, test runner config, package scripts, CI config, a results or report folder, and any project-local verify skill (a skill whose body has Launch, Doctor and Drive sections and a feature map, for example `.claude/skills/verify-*/` or `.cursor/skills/verify-*/`).
2. Record whether a pre-existing location for verification was found. Step 3 depends on this answer as it was **at the start of this step**.
3. If no location exists, create `project_root/test/` as the verification home.
4. Pick the Evidence folder: the project's existing results folder if it has one, else `<verification home>/evidence/`. If it is not already ignored by git, put a `.gitignore` containing `*` inside the Evidence folder itself. Never edit the project's own `.gitignore`, since it is outside the Verification Layer and may be part of what the plan changes.

## 2. Read The Plan And Tasks

GOALS:
* Every requirement the layer must prove has been read.

STEPS:
1. Open every plan and Sub-plan in scope, as defined in the Input section. If only Tasks were given, also open their parent plan.
2. Read each file's `goal`, `final state`, `final verification`, `how`, `out of scope` and `problem solving philosophy`.
3. Open every demo and every `.spec.md` listed in those `final verification` lists, global and per Task.
4. Note whether the plan or any Task says to start the Verification Layer from scratch.

## 3. Run A Playbook

GOALS:
* A complete list of Checks to add, alter and remove, each tied to a `final verification` item.

STEPS:
1. Choose the playbook:
   * No pre-existing location in step 1 → `./playbooks/from-scratch.md`.
   * A pre-existing location, and the plan or Tasks do **not** say to start from scratch → `./playbooks/from-existing.md`.
   * A pre-existing location, and the plan or Tasks **do** say to start from scratch → `./playbooks/from-scratch.md`.
   * None of the above fits → `./playbooks/from-existing.md`.
2. Copy the playbook's steps into the to-do list and run them.
3. Keep the returned **Check list**: every Check to add, alter or remove, with its `final verification` item, Task, kind (automated or visual) and Evidence location.

## 4. Create The Execution Plan

GOALS:
* The Check list is broken into modular batches with known dependencies. A batch is a group of Checks written together. It is not a `Task`.
* Every batch is in the to-do list.

STEPS:
1. Apply `principle-decompose-tasks`: group the Check list into batches, normally one per Task of the plan plus one for plan-level items, split further where a Task has many Checks.
2. For each batch, list the files it creates or changes. Mark two batches **sequential** if they touch the same file or one needs a fixture or helper the other creates. Mark every other pair **parallel-safe**. Shared helpers are their own batch, done first. The Coverage Map already exists from the playbook.
3. Add every batch to the to-do list in an order that respects the sequential marks. Whether parallel-safe batches are fanned out to sub-agents is decided by the executing agent, not by this skill.

## 5. Update The Verification Layer

GOALS:
* Every Check on the list exists, runs on its own and is recorded in the Coverage Map.

STEPS:
For each Check, in to-do order:
1. Take its `final verification` item and re-read the spec row or demo it comes from.
2. Write or change the Check in the project's existing framework, folder layout and naming. Assert what a user can observe (`principle-test-behavior-not-implementation`). For a visual Check, drive the real app through the demo's flow with the project's verify skill and capture at the demo's points (`principle-prefer-visual-verification`).
3. Make the Check write its Evidence to `<evidence folder>/plan-<id>/<task-n or plan>/<check>/`.
4. Run that Check on its own (`principle-incremental-verification`). Confirm it fails, and that the failure message names the missing behavior. For a guard Check, confirm it passes now, then confirm it would fail by breaking the item in a scratch copy, never in the project. If it errors or fails for another reason, fix the Check, never by weakening it.
5. Record it in the Coverage Map: item, Check, run command, Evidence location, status (`new`, `guard`, `existing-cited`, `altered` or `removed`).
6. For a removal, delete the Check and mark it `removed` in the map with the `final verification` item that made it obsolete.

## 6. Prove Red And Judge

GOALS:
* The whole layer distinguishes the current state from the final state.
* An impartial judge confirms every item is covered.

STEPS:
1. **Run the whole layer.** Every Check that was not added or altered still passes. Every guard Check passes. Every other new or altered Check fails for the right reason. Fix anything else under step 5's rules and re-run.
2. **Run each Task's Checks on their own**, using the per-Task commands in the Coverage Map, and confirm each group runs without the others.
3. **Spawn the judge** as a fresh sub-agent on the `judge model` from step 0. Give it only paths: the plan and Sub-plans, every demo and spec, the Coverage Map, the Verification Layer's files, and the Evidence of the run from step 6.1. Add the `judge requirements`. Give it no summary, reasoning or history of your own. Ask it to return:
   * for each `final verification` item: `covered`, `partial` or `uncovered`, with a reason;
   * for each new or altered Check: `red for the right reason`, `red for the wrong reason` or `green`, with a reason, and for each guard Check whether it really is already true and would catch a break;
   * any Check that looks weakened compared with the spec.
4. **Handle disputes.** Fix every result that is not `covered` or `red for the right reason` under step 5's rules, re-run step 6.1, and spawn a **new** judge. Repeat according to the `dispute rule` from step 0.

## 7. Return

GOALS:
* The caller can run, review and trust the layer without reading this agent's history.

STEPS:
Return a summary with exactly these parts and nothing else:
1. The command that runs the whole layer, and the command for each Task's Checks.
2. The path to the Coverage Map.
3. The Checks added, altered and removed, each with its `final verification` item.
4. The red-first result: existing and guard Checks passing, other new Checks failing for the right reason.
5. Any judge dispute still open, with the judge's reason.

## Principles

Read the leaf skill in full whenever you apply one. Each entry says when it applies.

* **`principle-extensive-verification`**: steps 3 and 5. Every requirement, rule and example gets its own Check.
* **`principle-incremental-verification`**: steps 4 and 5. Every Check and every Task's group of Checks runs on its own.
* **`principle-prefer-visual-verification`**: steps 3 and 5. User-facing behavior is checked by driving the real surface.
* **`principle-never-weaken-a-check`**: every edit to an existing Check, every fix in steps 5 and 6.
* **`principle-decompose-tasks`**: step 4. Break the Check list into modular batches with dependencies.
* **`principle-test-behavior-not-implementation`**: step 5. Assert what users can observe.
* **`principle-prove-it-works`**: steps 5 and 6. Run the real Check and read its real output.
* **`principle-sequence-verifiable-units`**: steps 4 and 5. Each batch ends in a state you can check.
* **`principle-separate-before-serializing-shared-state`**: step 4. Split shared files before sequencing around them.
