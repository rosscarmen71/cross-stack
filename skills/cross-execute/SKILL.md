---
name: cross-execute
description: execute a plan or individual task in the format of the plans and tasks created by the `/cross-plan` skill using cross agents. 
---

# Cross Execute

Bundled skills live at `../<name>/SKILL.md` from this file, so put this file's absolute path in every prompt to an agent that you want to use this stack.

GOALS:
* create an extensive verification layer for a cross style `plan` or individual `task` in the current project.
* complete a `plan` for the current project successfuly
* verify that the plan was completed successfully by a separate unbiased jury of agents and / or human.

## Non-negotiables

The Principles section below grounds every trigger. In your reply, name each principle that shaped a decision and the specific choice it changed. Cite only principles whose leaf SKILL.md you read this session.

The remaining triggers live only here:

- First message → restate the plan or task/s goal and the smallest change that meets it, then proceed. Open each subagent brief, PR `## Why`, and final report with the goal.
... # TODO
- A fork you could settle by running something → prototype it per `playbooks/prototype.md` instead of asking.
... # TODO
- Code → name the data shape first. Across a function boundary → **architect**. Fan-out → **swarm**. Bakeoff → **arena**. Contested design → **interrogate**.
... # TODO
- Prose, your reply included → **unslop**. Docs, PR descriptions, commit messages → **technical-writing**.
... # TODO
- Before commit → `/deslop` from the `cursor-team-kit` plugin. Before review → **no-comments**.
... # TODO
- Shipping UI / IDE / CLI → `control-ui` or `control-cli` from `cursor-team-kit`.
... # TODO
- A number you measured → **benchmark-checklist**.
... # TODO
- PR status → the **Babysit** playbook, not Cursor's built-in babysit skill. Landing a green stack → the **Shipping** playbook.
... # TODO
- Bot review comments → judge each on its merits.
... # TODO
- Long or unattended work → **show-me-your-work**.
... # TODO


STEPS:

## 0. Specify Agents and Atonomy

This is where you would specify the settings for like the judge agent, for example, where you would specify the model you wanted to use in any particular non-standard requirements for a cross-judge-chef agent that you want, ect. They should also be where you were explicitly asked about, for example what to do if a dispute remains or arises, if you don't specify, it'll use the default for the agent which will I'll want to make it so that just keeps trying to handle it and it uses primarily skills from potato stack which I will add to shift. It's focused towards fixing whatever bugger dispute there is. But for example, like if you wanna be really conservative involved in the number of tokens, you use you could specify at the beginning that like you don't want it to keep retry and you want to contact you the second there's a problem and take a pause, etc., rather than the standard of trying to figure it out by itself.
... # TODO
... # TODO settings step 3.1 reads, each with a default: shipping cadence (default: none in step 3, shipping happens in step 5); branching strategy (default: one integration branch per run cut from trunk, one worktree and branch per parallel stream cut from it, streams merged in one at a time with `--no-ff`); whether to wait for the user after step 3.1 (default: no); whether unfinished Tasks that selected Tasks depend on are added to the run (default: yes). Per ADR 0002, shipping and branching are set here, never in the plan.

## 1. Identify The Plan or Task

GOALS: 
* Identify if the user wants to execute a full plan and all of it sub tasks, a singular task, or a selection of tasks from a plan.
* Identify the specific plan, task, or plan and tasks by ID within the current project.
* Update the states of the plans and tasks to `in progress`.

STEPS:
1. The input for this skill should include identification of the task or full plan that is to be executed. If it was not provided in the input or was unclear in any way, ask the user for the plan or plan and task ID. Additionally, if it was unclear of the user wanted to complete an entire plan or just a task of a plan ask the user explicitly.
2. Identify and explicitly confirmed with the user the ID of the plan or task that is to be executed is correct.
3. Update the state of the plan and or task/s to `in progress`.

## 2. Update The Verification Layer

GOALS:
* Create or expand an existing cross style verification layer for the current project corresponding to the plan and or task being executed.

STEPS: 
0. get the **path/s from the root folder of the project to the `plan` and or `tasks`** of the plan and or task that is being executed.
1. Run the `/cross-verification` skill. **Input the list of path/s identifyed in step 0, and the agent and autonomy settings from step 0 of this skill.**

## 3. Execute The Plan

GOALS:
... # TODO

STEPS: 

### 3.1. Create Execution Streams

GOALS:
* The current state of the plan and every Task in scope is known: which are done, which are not, and whether the code has drifted from what the plan assumed.
* `plan/plan-<id>/execution-streams.md` lists every Execution Stream: what runs, in what order, what runs in parallel, where streams split and merge, and which Checks run when, with the result expected from each run.
* Nothing has been executed and the Verification Layer is unchanged. This step only reads, plans, and writes the streams file and, on major drift, the plan.

STEPS:
1. **Read the state.** Open the plan, every Sub-plan in its Road Map, the Coverage Map, and the red-first result returned by step 2. For each Task, note its `Status`, its `depends-on`, whether its Checks passed in step 2's run, and anything about it in `road map evolution`. When only Tasks were selected, still read the parent plan.
2. **Fix the scope.** Start from the plan or Tasks identified in step 1. Add every Task they depend on, directly or through other Tasks, that is not done, unless step 0 or the input says otherwise. A Task counts as done only if its status is `done` and its Checks passed in step 2's run. Leave done Tasks out of the streams, but count their Checks as passing in every later expected result.
3. **Check for drift.** Skim the commits since the plan's `Date` that touch the file scope of any Task in scope. Only if a remaining Task's `How` can no longer be followed as written, because files, modules or APIs it names were removed, moved or rewritten, update that plan's or Sub-plan's `Current Delta` and add a dated line to `road map evolution` saying what changed. Ignore any smaller drift.
4. **Check the Road Map.** Treat each Task's `depends-on` and parallel marking as given. If the Road Map contradicts itself or the state from step 1 (a dependency cycle, a Task with no Sub-plan, a status its Checks contradict), stop and handle it under the plan's `Problem Solving Philosophy`. Never redesign the plan in this step.
5. **Build the streams.** Apply `principle-decompose-tasks` to the Road Map, not to the Tasks: every Task in scope becomes one whole `task` entry, and breaking a Task into Actionable Items happens later. Order the entries so that each one comes after everything it depends on, and put Tasks that can run in parallel in separate streams. Then add the other entries:
   * **`verification`:** apply `principle-incremental-verification` and `principle-sequence-verifiable-units`. Add one after every `task` entry, wherever streams merge, and once at the end of all streams. Each names its run command from the Coverage Map (one Task's Checks, everything merged so far, or the whole layer) and the expected result: which Checks should pass and which should still fail.
   * **`plan`:** for a full-plan run, add a reconcile point where streams merge (do the merged Tasks' notes in `road map evolution` change any later Task?) and one closing reconcile after the last `verification` entry (is every Task done and is `road map evolution` complete?). Add or drop `plan` entries wherever that makes more sense for this plan. A run of selected Tasks has no `plan` entries; anything plan-level it runs into goes into `road map evolution` from within its `task` entry.
   * **`utility`:** only for work the step 0 settings ask for that is not plan, Task or verification work. Do not add entries for branches, worktrees, commits or PRs; the playbooks decide those.

   Give every entry at most one Entry Label (`plan`, `task`, `verification` or `utility`). Never name a playbook; step 3.2 matches entries to playbooks.
6. **Write the streams file** to `plan/plan-<id>/execution-streams.md`. Every entry has an id, its label if any, what it covers, and a `waits-for` list of the entry ids it must wait for. A split is several streams whose first entries wait for the same entry. A merge is one entry that waits for entries in several streams. For example:

   ```md
   ## Stream A
   - A1 · task · Task 7.1
   - A2 · verification · `npm test -- task-7.1` → expect: 7.1 passes, guards pass, 7.2–7.4 fail

   ## Stream B
   - B1 · task · Task 7.2
   - B2 · verification · `npm test -- task-7.2` → expect: 7.2 passes, guards pass, 7.3–7.4 fail

   ## Stream C
   - C1 · verification · `npm test -- task-7.1 task-7.2` → expect: 7.1 and 7.2 pass, guards pass · waits-for: A2, B2
   - C2 · plan · reconcile Tasks 7.1 and 7.2 with the Road Map · waits-for: C1
   - C3 · task · Task 7.3 · waits-for: C2
   - ...
   ```
7. **Report and continue.** Give the path to the streams file and a one-paragraph summary of the streams, then go on to step 3.2. Wait for the user first only if the step 0 settings say to.

### 3.2. Run High Level Execution Streams

OK, so in the section like you can create your actual agents that are gonna be handling execution streams for each stream that's like that's like a parallel thing like you can have your own guy doing its own thing again like you gotta be very careful making sure that like the second that they do actually like two streams merge and like they have a dependent tasks basicall you wanna make sure they hold off weight and then do their thing after. And here is basically like where you're getting into completing the chunks of global tasks, then local tasks and then global verifications local verifications as I described in the previous step this is like this is where you were actually like getting just the more instructions and like how you're actually gonna start executing. For every single chunk on the to-do list, you should be mapping it to a playbook. If there isn't a playbook that really fits like there just isn't one that's maybe for like a specific type of task or not type of task like maybe I don't know. There's just something in the to-do list that doesn't have a playbook then you can that's fine. It's more just like certain tasks that they come up really frequently like executing a tas that's maybe to fix a bug or executing a task that's to add a new feature or examining the global state of a plan or running a local verification or like running a certain verifications like those are all things that are gonna come up a lot obviously so those are gonna have play books that can be utilized. For every to do that is marked as a verification to do you only need to examine the verification play books. You don't have to look at any others and again if one doesn't fit, you should try to find one that fits cause there should be one that fits, but like if there is genuinely not a single one then it's fine. Same goes for one's marked as global. You should be looking at the global playbook and one's marked as local you should be looking at the local play books local play books mind you are like the same as task play books so I'm not sure if I'm gonna call the playbook play books local or playbook tasks probably gonna call it playbook tasks cause I think well no no that doesn't make sense because like a task again like you want to if a task is still quite large you still want to decompose it so also we should be utilizing the decompose task principle, but that should probably be done when we're creating the execution streams in the previous step but yeah, they'll be a bunch of principles that we should be applying, especially in the last step like anytime you're planning or making lots of decisions they'll probably be quite a few principles that need to be applied when like deciding how to go about. 


## 4. Verify and Improve Loop

## 5. Wrap Up





## Task Playbooks

## Principles

Read the leaf skill in full whenever you apply one. Each entry says when it applies.

* **`principle-decompose-tasks`**: step 3.1. Turn the Road Map into streams, marking what runs in sequence and what in parallel. Tasks themselves stay whole in 3.1.
* **`principle-incremental-verification`**: step 3.1. Place a `verification` entry wherever a modular piece is finished and wherever streams merge, each with its expected result.
* **`principle-sequence-verifiable-units`**: step 3.1. Each `task` entry ends in a state its `verification` entry can check before anything builds on it.




