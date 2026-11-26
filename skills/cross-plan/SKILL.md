---
name: cross-plan
description: Turn a request into an approved, verifiable plan. Grill the user on what they want, demo the final state, identify the delta, grill how, break the work into Tasks with their own demos, set the problem solving philosophy, and get approval. Planning only.
disable-model-invocation: true
---

# Cross Plan

## Goal

Turn the user's request into a plan that an executor agent with no other context can carry out and verify. First define exactly `what` the user wants and show it as approved demos, then define `how` to close the gap between the current state of the project and that final state.

## Non-Negotiables

* **What before how.** Never decide `how` until `what` is defined, demoed and approved.
* **Always record why.** Store why the user wants this, why this result, and why this approach in the plan's `why` field. They guide later decisions and verification.
* **Prefer visual demos.** Strongly prefer visual demos. Only use a spec-only demo when every visual playbook is impractical.
* **Everything is verifiable.** Every requirement is backed by a demo, a rule or a spec that an executor can check.
* **Modular `Tasks`.** Break the work into `Tasks` that can each be worked on and tested separately, with their own definition of done.
* **The user approves.** The user approves every demo, the `Task` breakdown, every `Task`, and the final plan. Ask the user whenever a decision about `what` or `how` needs to be made.
* **Never execute.** This skill ends at an approved plan. It never starts carrying the plan out.

## Output

```
project_root_folder/plan/
├── plan-<id>.md                      ← the plan (from ./templates/plan.md)
└── plan-<id>/
    ├── demos/                        ← global demos
    │   ├── <nn>-<slug>.<ext>
    │   └── <nn>-<slug>.spec.md
    ├── task-<n>.md                   ← one Sub-plan per Task (from ./templates/sub-plan.md)
    └── task-<n>-demos/               ← that Task's demos
```

## 0. Setup

GOALS:
* Create a new plan file in the project and fill in the generic fields.

STEPS:
0. **Copy steps 0–6 of this skill into the to-do list tool** before doing anything else, and keep it updated as you go.
1. **Scan all plan files inside the current project's plan folder.** Create a unique numerical `id` that no other plan in the project has.
2. Create a plan file following the template at `./templates/plan.md` in the `plan` folder in the root directory of the current project (`project_root_folder/plan/plan-<id>.md`). **Ask the user where the file should be placed if you are unsure.**
3. Fill in the new plan's `id` field with the unique id.
4. **Ask the user for the title of the plan.** Fill in the `title` field of the plan file.
5. Fill in the `date` field of the new plan with the current date.
6. Fill in the `status` field of the new plan with `writing in progress`.

## 1. Define The `What`

GOALS:
* Come to an agreed understanding with the user about exactly `what` the user wants to accomplish.
* Create detailed demos and specifications of what a final successful outcome for this request looks like. If these specifications are met and the behavior of the code base matches that of the demos, then the request has been fulfilled.
* Fill in the `goal`, `why` and `final state` fields of the plan created in step 0.

STEPS:

### 1.1. Get Specific

GOALS:
* Resolve a vague definition of `what` the user wants for the request.
* Fill in the `goal` field and the "why the user wants this" part of the `why` field of the plan created in step 0.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.

STEPS:
0. **Read the notes in the `permissions`, `out of scope`, `goal` and `why` sections of the plan template.**
1. Copy any parts of the request that only discuss `how` into the `how` field of the plan created in step 0, each marked `(from initial request)`. They are used in step 3.
2. Input the rest of the request to the **`/grill-what`** skill.
3. Based on the output of `/grill-what`, fill in the `goal` field and the "why the user wants this" part of the `why` field of the plan created in step 0. Additionally, if the user **explicitly** mentioned anything that should go into the `permissions` or `out of scope` sections, add that information to those fields as well. Copy every item in the returned **How notes** into the `how` field, each marked `(from initial request)`.

### 1.2. Create Global Demo

GOALS:
* Create one or more demos that show the intended behavior of the code base once it is in the final state that meets the request.
* Create a spec file to go along with each demo, for any specifications that cannot be shown in the selected demo format.

STEPS:
1. **Demonstrate understanding.** Match the description of `what` needs to be done from `/grill-what` in step 1.1 to one or more demo playbooks, and open each one at `./playbooks-demos/<title>.md`, with the title in kebab case. **You should have a very strong preference for visual playbooks. Only select the `spec.md` playbook if all other playbooks are impractical.**
2. Copy the selected playbook's `Creation Steps` into the to-do list tool. Run each of those steps now to create the demo. Store global demos in `project_root_folder/plan/plan-<id>/demos/`, named as the playbook's `Output` section says.
3. Run the `Show User Steps` in the playbook to show the user the demo.

>**Note: it may make more sense to split the demo into several different types of demos, especially if the request has separate parts. Some parts may be best expressed in slides or images, while a new feature's behavior may be best shown with a video of what it should do when you interact with it.**

### 1.3. Revision Loop

GOALS:
* Revise the demos that represent the intended behavior after the request is met, until the user is satisfied with each.
* Fill in the "why this result" part of the `why` field of the plan created in step 0.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.

STEPS:
1. Explicitly ask the user if they are satisfied with the demos from step 1.2, which are intended to show the final state when the request is satisfied.
2. If the user is satisfied, skip to step 1.4. **If the user is not satisfied, explicitly ask the user what they want changed about the demo. Explicitly ask the user what about the demo does not reflect the intended final state or functionality that should be present once the request is done.**
3. Sharpen the definition of the final state, and how the demo needs to change, by inputting the answers to the last 2 questions into the **`/grill-what`** skill. Add any reasons the user gives for the result they want to the "why this result" part of the `why` field.
4. Update the demo with the requested changes. Then show the updated demo to the user.

Repeat steps 1–4 of 1.3 until the user says they are satisfied with the demo. Then move on to step 1.4.

### 1.4. Build Out The Verification Layer

GOALS:
* Create a robust, detailed definition of the verification layer that will be used to determine when the request is completed.
* Define explicit rules and the explicit, detailed functionality that must be met.
* Identify all explicit, detailed requirements for what must be done.
* Fill in the `final state` field and the `final verification` sub-field of the plan created in step 0.
* Complete the "why this result" part of the `why` field of the plan created in step 0.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.
* All specifications and demos an executor agent needs to build a substantial verification layer for the plan are in the plan document, the demos, or other linked files.

STEPS:
0. **Review the notes in the `final state` field and the `final verification` sub-field of the plan template.**
1. Add the path to every demo finalized in step 1.3 to the `final verification` list in the plan created in step 0. With each path, include a 1 to 2 sentence description of what the demo shows.
2. Add all rules and specifications known so far that must be met for the request to be completed to the `final verification` list.
3. Fill in the `final state` field of the plan created in step 0 with a description.

## 2. Identify The Delta

GOALS:
* Fill in the `current delta` field of the plan created in step 0.
* Identify the difference between the behavior of the code base now and what it is intended to be once the plan is completed.
* Identify the difference in the code base between the current state and the state once the plan is completed.
* Identify what needs to happen to close the difference between the current state and the intended state.

STEPS:
1. **Identify the behavioral difference.** Read the code base and identify the differences between the demos and defined final state, and the current behavior and state of the project.
2. **Pinpoint the differences.** Identify what in the existing code base needs to be added, changed or removed to close the delta between the current and intended final state.
3. Record your findings in the `current delta` field of the plan created in step 0.

## 3. Scope The `How`

GOALS:
* Fill in the `how` field and the "why this approach" part of the `why` field of the plan created in step 0.
* Create a clear definition of how the user wants to close the delta between the current state and the intended state. It should mainly consist of any specific requirements, directions or methods the user wants.
* Identify every non-negotiable for both `what` and `how`, as specific and measurable as the user will agree to.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.

STEPS:

### 3.1. Grill The `How`

GOALS:
* Fill in the `how` field and the "why this approach" part of the `why` field of the plan created in step 0.

STEPS:
0. **Review the notes in the `how` field of the plan template.**
1. **Determine `how` to close the delta.** Run the **`/grill-how-with-docs`** skill to get specifications for how the user wants to update the code base to reach the intended final result. Input all known information about `what` the plan must do, the current delta, and every `how` item marked `(from initial request)`. Add the output list of specs to the `how` list in the plan created in step 0, and the reasons the user gives to the "why this approach" part of the `why` field.
2. If the returned list has any **What changes**, go back to step 1 and apply them through `/grill-what` and the 1.3 revision loop. Then redo step 2 for anything the change affects, before continuing.

### 3.2. Interrogate For Non-Negotiables

GOALS:
* Every non-negotiable for `what` and `how` is found, made as specific and measurable as the user will agree to, and recorded in the plan created in step 0.
* No non-negotiable conflicts with a demo, spec or any other part of the plan.

STEPS:
1. Run the **`/grill-non-negotiables`** skill. Input the whole plan created in step 0 (`goal`, `why`, `final state`, `final verification`, `current delta` and `how`) and every demo.
2. From the returned list, add each `what` non-negotiable to the `final verification` list and each `how` non-negotiable to the `how` list in the plan created in step 0, each with its **Verify by**.
3. Add any substantive **Why** to the matching part of the `why` field.
4. Check the returned **Files updated**. If any demos were changed, make sure the `final verification` list still describes them correctly.

## 4. Create The Roadmap

GOALS:
* Break the plan down into modular `Tasks` that can each be worked on and tested separately.
* Create a `Sub-plan` file for every `Task`, with its own demos, verification layer, file scope and task scope.
* Identify which `Tasks` must run in sequence and which can run in parallel.
* Fill in the `road map` field of the plan created in step 0.
* Every item in the plan's `current delta` and `final verification` is covered by at least one `Task`.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.

STEPS:

### 4.1. Break Down Into Tasks

GOALS:
* Propose a list of `Tasks` that together close the full `current delta` and meet the full `final verification` layer.

STEPS:
0. **Review the `goal`, `final state`, `final verification`, `current delta` and `how` fields of the plan created in step 0.**
1. Propose a list of `Tasks`. Give each one a title and a one-sentence goal. Every `Task` must follow these rules:
   * **Own tests.** The `Task` has its own tests. A `Task` may depend on the functionality of another `Task`, but if two `Tasks` would use the exact same tests, merge them into one `Task`.
   * **Own demo.** The `Task` has at least one demo. If a visual demo is impractical, a spec file counts as the demo.
   * **File scope.** The `Task` lists the exact files it is allowed to create, change or delete.
   * **Task scope.** The `Task` states what is and is not included in it, and what it means for it to be done.
2. Give each `Task` an id of the form `<plan-id>.<n>` (for example `7.1`, `7.2`), numbered in the order the `Tasks` were created.

### 4.2. Identify Dependencies

GOALS:
* Know, for every `Task`, which other `Tasks` must finish before it can start.

STEPS:
1. For every pair of `Tasks`, mark them as **sequential** if their file scopes overlap or if one depends on the functionality the other adds.
2. Mark every other pair as **parallel**. `Tasks` can only run in parallel if they change none of the same files and neither depends on the other's changes.
3. Record each `Task`'s `depends-on` list (the ids of the `Tasks` it must wait for).

### 4.3. Approve The Breakdown

GOALS:
* The user approves the list of `Tasks` before any `Sub-plan` is written.

STEPS:
1. Show the user the `Task` list: id, title, goal, file scope, task scope and `depends-on` for each one.
2. **Explicitly ask the user if they approve the breakdown.** If not, ask what should change, update the `Task` list (repeating 4.1 and 4.2 as needed), and show it again. Repeat until the user approves.

### 4.4. Write The Sub-plans

GOALS:
* Every `Task` has a complete `Sub-plan` and demos that the user is satisfied with.
* Each `Sub-plan` stands on its own for an executor agent and does not contradict the plan created in step 0.

STEPS:
For each `Task`, in id order:
1. Create the `Sub-plan` file following the template at `./templates/sub-plan.md` at `project_root_folder/plan/plan-<id>/task-<n>.md`. Set its `status` to `writing in progress`.
2. Fill in the `Sub-plan` fields from what is already known in the plan created in step 0:
   * `goal`, `out of scope`, `final state` and `final verification`: specific to this `Task`.
   * `why`: inherited from the plan, plus any reasons specific to this `Task`.
   * `current delta`: only the part of the plan's `current delta` that this `Task` closes.
   * `how`: only the plan's `how` items that apply to this `Task`.
   * `permissions`: inherited from the plan, unless this `Task` narrows them.
3. **Grill only the gaps.** Do not ask the user about anything already decided in steps 1–3. For decisions that are new to this `Task`, run **`/grill-what`** (for what the `Task` must do and show) and **`/grill-how`** (for how it should be done). Add the answers to the `Sub-plan`.
4. Create the `Task`'s demos by following steps 1.2 and 1.4 at the scope of this `Task`. Store them in `project_root_folder/plan/plan-<id>/task-<n>-demos/`, and add each demo's path and a 1 to 2 sentence description to the `Sub-plan`'s `final verification` list.
5. Show the user the `Task`'s demos and run the 1.3 revision loop on them until the user says they are satisfied.
6. Set the `Sub-plan`'s `status` to `draft`.

### 4.5. Fill In The Road Map

GOALS:
* The `road map` field of the plan created in step 0 is a complete index of every `Task`.
* Nothing in the plan is left uncovered by the `Tasks`.

STEPS:
1. **Check coverage.** Confirm that every item in the plan's `current delta` and `final verification` is covered by at least one `Task`. Show the user anything uncovered, and add or update `Tasks` (back to 4.1) until everything is covered.
2. In the `road map` field of the plan created in step 0, list every `Task` with its id, title, `Sub-plan` path, file scope, `depends-on` and whether it can run in parallel.
3. Leave `road map evolution` empty. It is a log that the executor adds to if the road map changes during execution.

## 5. Define The Problem Solving Philosophy

GOALS:
* Fill in the `problem solving philosophy` field of the plan created in step 0.
* Record the user's rules for handling problems that come up during execution, such as failing tests, demos that can't be reached, or unexpected complexity.
* Every category in step 5.2 has either a rule from the user or the explicit line `use the executor's default`.
* Do **not** define the defaults themselves, and do not try to cover every possible situation.
* Optional: Add any scope restrictions identified in this section to the `out of scope` field in the plan created in step 0.
* Optional: Add any explicit permissions, or restrictions of permissions, mentioned by the user to the `permissions` field in the plan created in step 0.

STEPS:

### 5.1. Find Risk Areas

GOALS:
* Know which parts of the plan are most likely to cause problems, so the right questions get asked.

STEPS:
0. **Review the notes in the `problem solving philosophy` field of the plan template.**
1. Read the plan's `current delta`, the `road map`, every `Sub-plan`, and the code they touch. Identify risk areas. For example: files in many `Tasks`' file scopes, code with no tests, external services or APIs, data migrations, destructive operations, and `Tasks` with many dependents.
2. Turn each risk area into a question for step 5.2, with a suggested rule. For example: "`Task` 7.3 runs a data migration. Should the executor always stop and ask before running it?" **Do not record the risks themselves in the plan.** They only shape the questions.

### 5.2. Interrogate For Rules

GOALS:
* Get the user's rules for each category below and for each risk area from step 5.1.

STEPS:
1. Run the **`/grill-how`** skill. Input the categories below, the risk-area questions from step 5.1, and all known context about `what` the plan must do and `how`. Ask about every category:
   * **Repeated failure:** how many attempts at a failing test or demo before stopping or escalating.
   * **Regression:** what to do when a previous `Task`'s tests or demos break.
   * **Demo can't be reached or looks wrong:** what to do when the final state seems impossible, or a demo itself seems mistaken.
   * **Scope creep:** what to do when a fix needs files outside a `Task`'s file scope.
   * **Permissions:** installing dependencies, network access, destructive commands, and similar.
   * **Communication:** how often to notify or ask the user.
   * **An undecided choice comes up during execution:** stop and ask, or decide and record why.
2. If the user has no preference for a category, record `use the executor's default` for it. Do not invent a rule.
3. Rules apply to the whole plan by default. Only create a rule for a single `Task` if the user explicitly asks for one.

### 5.3. Record The Philosophy

GOALS:
* The `problem solving philosophy` field is complete and readable by an executor agent with no other context.

STEPS:
1. In the `problem solving philosophy` field of the plan created in step 0, write one entry per category from step 5.2, then one entry per rule from the step 5.1 risk areas.
2. Start any rule that applies to only one `Task` with that `Task`'s id. For example: `Task 7.2: always stop and ask before changing billing code.`

## 6. Finalize

GOALS:
* Every field in the plan and in every `Sub-plan` is complete, consistent and answered.
* The user has reviewed the full plan, every `Sub-plan` and every demo.
* The user explicitly approves the plan.
* The plan and every `Sub-plan` have their `status` set to `approved`.
* Do **not** start executing the plan.

STEPS:

### 6.1. Sweep For Loose Ends

GOALS:
* No empty fields, placeholders, contradictions or unanswered decisions are left before the user reviews the plan.

STEPS:
1. Scan the plan created in step 0 and every `Sub-plan` for:
   * fields that are empty or still contain `<placeholder>` text,
   * `Sub-plans` that contradict the plan created in step 0,
   * decisions that were never answered.
2. Resolve each one with the user. Run **`/grill-what`** for anything about `what`, and **`/grill-how`** for anything about `how`. Update the affected files.
3. Set the `status` of the plan created in step 0 and of every `Sub-plan` to `draft`.

### 6.2. Review The Global Plan

GOALS:
* The user has reviewed and accepted the plan created in step 0 and its global demos.

STEPS:
1. Show the user the plan created in step 0.
2. Show the user every global demo by running the `Show User Steps` in each demo's playbook.
3. **Explicitly ask the user if they want any changes.** If they do, follow step 6.4, then continue from here.

### 6.3. Review Each Task

GOALS:
* The user has reviewed and approved every `Task`.

STEPS:
For each `Task`, in id order:
1. Show the user the `Task`'s `Sub-plan`.
2. Show the user every demo for the `Task` by running the `Show User Steps` in each demo's playbook.
3. **Explicitly ask the user if they approve this `Task`.** If they want changes, follow step 6.4, then show the `Task` again. Move to the next `Task` only once the user approves this one.

### 6.4. Apply Changes

GOALS:
* Every change requested during review goes through the same questions and checks as the original.

STEPS:
1. Return to the step that owns the part being changed, and redo it for that change only:
   * a change to `what` (goal, final state, demos, final verification) → step 1,
   * a change to `how` → step 3,
   * a change to the `Task` breakdown → step 4.3, then the coverage check in step 4.5,
   * a change to a `Sub-plan` → step 4.4 for that `Task`,
   * a change to the rules for handling problems → step 5.
2. Update every file affected by the change, then go back to the review step you came from.

### 6.5. Final Approval

GOALS:
* The user has explicitly approved the whole plan, and the executor can start from it.

STEPS:
1. **Explicitly ask the user if they approve the whole plan.** If not, ask what should change and follow step 6.4.
2. Once the user approves, set the `status` of the plan created in step 0 and of every `Sub-plan` to `approved`.
3. Tell the user the path to the plan, and that it is ready for an executor. **Do not start executing the plan.**