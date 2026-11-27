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
... # TODO settings steps 3.1 and 3.2 read, each with a default:
* agent cap: ask the user explicitly how many agents may run at once and tell them the default is 4. It counts Execution Sub-agents, their helpers and judges, not the Execution Agent.
* models for the Execution Agent, the Execution Sub-agents and the judge.
* merge approval: `human, non-blocking` (default; work continues on top of a merge that is still waiting for review), `human, blocking` or `none`.
* whether this run checks merge approvals (default: no; review outcomes are handled by a later run).
* shipping cadence and branching strategy. Per ADR 0002 these are set here, never in the plan. The Execution Playbooks carry them out.
* whether to wait for the user after step 3.1 (default: no).
* whether unfinished Tasks that selected Tasks depend on are added to the run (default: yes).

## 1. Identify The Plan or Task

GOALS: 
* Identify if the user wants to execute a full plan and all of it sub tasks, a singular task, or a selection of tasks from a plan.
* Identify the specific plan, task, or plan and tasks by ID within the current project.
* Update the states of the plans and tasks to `in progress`.

STEPS:
1. The input for this skill should include identification of the task or full plan that is to be executed. If it was not provided in the input or was unclear in any way, ask the user for the plan or plan and task ID. Additionally, if it was unclear of the user wanted to complete an entire plan or just a task of a plan ask the user explicitly.
2. Identify and explicitly confirmed with the user the ID of the plan or task that is to be executed is correct.
3. Update the state of the plan and or task/s to `in progress`.
4. Hand over to the Execution Agent. Spawn `cross-execute-chef` as a background agent with the confirmed ids and the settings from step 0. It runs steps 2 to 5. The main agent runs none of them: it only passes the Execution Agent's questions to the user and resumes it with the answers.

## 2. Update The Verification Layer

GOALS:
* Create or expand an existing cross style verification layer for the current project corresponding to the plan and or task being executed.

STEPS: 
0. get the **path/s from the root folder of the project to the `plan` and or `tasks`** of the plan and or task that is being executed.
1. Spawn a sub-agent to run the `/cross-verification` skill, and keep only the summary it returns. **Input the list of path/s identifyed in step 0, and the agent and autonomy settings from step 0 of this skill.**

## 3. Execute The Plan

GOALS:
* Carry out every Task in the run as its Sub-plan describes.
* Have a fresh judge confirm that the Checks of every Task in the run pass and that nothing that passed before has stopped passing.
* Stay within the plan's `out of scope` and `Permissions`.
* Keep the Execution Queue and `road map evolution` true: what was done, what changed, and anything left unfinished with its reason.

STEPS: 

### 3.1. Create The Execution Queue

GOALS:
* The current state of the plan and every Task in scope is known: which are done, which are not, and whether the code has drifted from what the plan assumed.
* `plan/plan-<id>/execution-queue.md` lists every Queue Entry of this run: what will be carried out, what each entry waits for, who runs it, and which Checks each verification is expected to make pass.
* Nothing has been executed and the Verification Layer is unchanged. This step only reads, plans, and writes the queue file and, on major drift, the plan.

STEPS:
1. **Read the state.** Open the plan, every Sub-plan in its Road Map, the Coverage Map, and the red-first result returned by step 2. For each Task, note its `Status`, its `depends-on`, whether its Checks passed in step 2's run, and anything about it in `road map evolution`. When only Tasks were selected, still read the parent plan.
2. **Fix the scope.** Start from the plan or Tasks identified in step 1. Add every Task they depend on, directly or through other Tasks, that is not done, unless step 0 or the input says otherwise. A Task counts as done only if its status is `done` and its Checks passed in step 2's run. Leave done Tasks out of the queue.
3. **Check for drift.** Skim the commits since the plan's `Date` that touch the file scope of any Task in scope. Only if a remaining Task's `How` can no longer be followed as written, because files, modules or APIs it names were removed, moved or rewritten, update that plan's or Sub-plan's `Current Delta` and add a dated line to `road map evolution` saying what changed. Ignore any smaller drift.
4. **Check the Road Map.** Treat each Task's `depends-on` and parallel marking as given. If the Road Map contradicts itself or the state from step 1 (a dependency cycle, a Task with no Sub-plan, a status its Checks contradict), stop and handle it under the plan's `Problem Solving Philosophy`. Never redesign the plan in this step.
5. **List the entries.** Apply `principle-decompose-tasks` to the Road Map, not to the Tasks: every Task in scope becomes one whole `task` entry, and breaking a Task into Actionable Items happens in step 3.2. Then add the other entries:
   * **`verification`:** apply `principle-incremental-verification` and `principle-sequence-verifiable-units`. Add one directly after every `task` entry, one before any Task that depends on two or more Tasks (covering those Tasks together), and one final entry after everything else (covering every Task in the run, which is the whole layer for a full-plan run). Each names its Target Checks by Task. The expected result is always the same: the Target Checks pass and nothing in the Baseline has stopped passing. Never write that a Check should still fail, because Tasks that run in parallel finish in no fixed order.
   * **`plan`:** for a full-plan run, add a reconcile after each verification entry that covers several Tasks (do the finished Tasks' notes in `road map evolution` change any later Task?) and one closing reconcile after the final verification entry (is every Task finished and is `road map evolution` complete?). Add or drop `plan` entries wherever that makes more sense for this plan. A run of selected Tasks has no `plan` entries; anything plan-level it runs into is reported from within its `task` entry.
   * **`utility`:** only for work the step 0 settings ask for that is not plan, Task or verification work. Do not add entries for branches, worktrees, commits or PRs; the Execution Playbooks decide those.

   Give every entry at most one Entry Label (`plan`, `task`, `verification` or `utility`). Never name a playbook; step 3.2 matches entries to playbooks.
6. **Set the order.** Give every entry a `waits-for` list of the entries that must be done before it may start. Build it from the Road Map's `depends-on`, then add any other ordering the plan, the step 0 settings or the Execution Playbooks require. Step 3.2 starts an entry as soon as everything it waits for is done and follows no other order, so every ordering rule must be written here.
7. **Write the queue file** to `plan/plan-<id>/execution-queue.md` as one table. An id is a letter for the label (`T` task, `V` verification, `P` plan, `U` utility, `E` no label) and a number. `run by` is `Execution Agent` for every `plan` entry and every `verification` entry that does not directly follow a `task` entry, and `sub-agent` for everything else. Every `status` starts as `pending`. For example:

   ```md
   | id | label        | covers                              | waits-for | run by          | status  |
   |----|--------------|-------------------------------------|-----------|-----------------|---------|
   | T1 | task         | Task 7.1                            |           | sub-agent       | pending |
   | V1 | verification | Target Checks: Task 7.1             | T1        | sub-agent       | pending |
   | T2 | task         | Task 7.2                            |           | sub-agent       | pending |
   | V2 | verification | Target Checks: Task 7.2             | T2        | sub-agent       | pending |
   | V3 | verification | Target Checks: Tasks 7.1 and 7.2    | V1, V2    | Execution Agent | pending |
   | P1 | plan         | reconcile Tasks 7.1 and 7.2         | V3        | Execution Agent | pending |
   | T3 | task         | Task 7.3                            | P1        | sub-agent       | pending |
   | V4 | verification | Target Checks: Task 7.3             | T3        | sub-agent       | pending |
   | V5 | verification | Target Checks: every Task in the run | V4       | Execution Agent | pending |
   | P2 | plan         | closing reconcile                   | V5        | Execution Agent | pending |
   ```
8. **Report and continue.** Give the path to the queue file and a one-paragraph summary of it, then go on to step 3.2. Wait for the user first only if the step 0 settings say to.

### 3.2. Run The Execution Queue

GOALS:
* Every Queue Entry is `done`, or is reported with the reason it is not.
* Every verification was run by a fresh judge. No agent that did the work read a Check or its output.
* The queue file and the plan files show the true state of the run at every moment.

RULES:
* **One branch per Task.** Every `task` entry is executed on its own branch. How and when branches are created, merged and shipped is decided by the Execution Playbooks, never by this step.
* **One writer.** Only the Execution Agent writes the queue file, the plan and Sub-plan files, and `road map evolution`. Every other agent reports to it instead.
* **Statuses.** An entry is `pending`, `running`, `waiting`, `done`, `failed` or `skipped`. Tasks and the plan stay `in progress` throughout this step; `done` is set in step 5.
* **Independent verification** (ADR 0003). An agent that does the work never runs Checks and never opens the Verification Layer, the Coverage Map or the Evidence. It only receives a judge's verdict.
* **Agent cap.** Never have more agents running at once than the cap from step 0. Execution Sub-agents, their helpers and judges all count. The Execution Agent does not.

STEPS, for the Execution Agent:
1. **Start what is ready.** An entry is ready when it is `pending` and every entry in its `waits-for` is `done`. Start ready entries in Road Map order, then queue order, as far as the agent cap allows, and mark each `running`. Follow no order other than `waits-for`.
2. **Run each entry by its `run by` and label.**
   * A `task` entry and the `verification` entry directly after it: spawn one fresh Execution Sub-agent for the pair. Its brief holds the absolute path to this file, the instruction to follow "The Execution Sub-agent" below, the paths to the plan and the Sub-plan, and the two entries.
   * A `verification` entry run by the Execution Agent: follow "Running A Verification" below. Nothing is excused in these runs.
   * A `plan` entry: run it yourself, with the playbook chosen under "Choosing An Execution Playbook" below.
   * Any other entry: spawn a fresh sub-agent with the entry and the instruction to follow "Choosing An Execution Playbook".
3. **Record every report.** When an agent reports, set the entry's status in the queue file and add its notes to `road map evolution`.
4. **Handle a question for the user.** When a report is `needs-human`, mark the entry `waiting` and keep starting every entry that does not wait for it. Once nothing else can start or is running, finish with one report to the main agent that lists every open question. When the main agent resumes you with the answers, resume the same sub-agent with its answer.
5. **Handle a problem.** When a verdict or report shows a problem:
   ... # TODO
6. **Repeat** steps 1 to 5 until no entry is `pending` or `running`, then go on to step 4.

#### Choosing An Execution Playbook

Done by whichever agent runs the entry, just before running it.

1. Read the `When To Use` section of every playbook in `playbooks/<label>/` for the entry's label. For an entry with no label, read them in every label folder.
2. Use the one playbook that clearly fits the entry.
3. If none clearly fits:
   * **`task`:** use `playbooks/task/decompose-task.md`. Every `task` entry runs a playbook.
   * **`verification`:**
     ... # TODO
   * **`plan`:**
     ... # TODO
   * **`utility`:**
     ... # TODO
   * **No label:**
     ... # TODO

#### The Execution Sub-agent

1. **Read** the Sub-plan and its parent plan.
2. **Choose** the Execution Playbook for the `task` entry.
3. **Break the Task into Actionable Items** in the to-do list, following the playbook. Place a verification point wherever a meaningful piece of the Task is complete, and for each one list the Sub-plan's `final verification` items that should be met by then, which may be none. Where the points go is the breakdown's decision. The only fixed one is the Task's own `verification` entry at the end, which covers every `final verification` item of the Sub-plan.
4. **Execute the Actionable Items** in order. You may hand a larger one to a helper agent within the agent cap. A helper cannot spawn agents of its own.
5. **At each verification point**, follow "Running A Verification" below and act on the verdict. A failure marked `expected: unfinished work` needs no action yet. For a failure marked `problem`:
   ... # TODO
   At the Task's own `verification` entry nothing is excused: every failure is a problem.
6. **Report** to the Execution Agent: the result of each of the two entries (`done`, `failed` or `needs-human`), the files and commits changed, the last verdict, notes for `road map evolution`, and any question for the user.

#### Running A Verification

Apply `principle-incremental-verification`. Done by whichever agent has reached the verification.

1. **Spawn a fresh judge** (`cross-judge-chef`, on the judge model from step 0). Use a new judge for every run.
2. **Brief it with facts and paths only**, never an explanation, reasoning or opinion of your own:
   * the paths to the plan, the Sub-plans involved, the Coverage Map and the Evidence folder;
   * the branch to run on;
   * the `final verification` items expected to be met by now, or the Tasks named in the `verification` entry;
   * inside a Task, the list of Actionable Items, each marked executed or not yet executed.
3. **The judge** reads the Baseline from the Evidence of the last run on this branch, runs the whole layer, and finds the Target Checks for the listed items in the Coverage Map. It then replies with a verdict:
   * each Check as pass or fail, marked as a Baseline Check or a Target Check;
   * each failure as `expected: unfinished work` or `problem`, with the reason.

   The judge never changes code or Checks.
4. **Read only the verdict.** The Evidence of this run is the Baseline for the next run on this branch.

## 4. Verify and Improve Loop
... # TODO

## 5. Wrap Up
... # TODO





## Task Playbooks
... # TODO

## Principles

Read the leaf skill in full whenever you apply one. Each entry says when it applies.

* **`principle-decompose-tasks`**: step 3.1, to turn the Road Map into Queue Entries with their `waits-for`. Step 3.2, when an Execution Sub-agent breaks its Task into Actionable Items.
* **`principle-incremental-verification`**: step 3.1, to place the `verification` entries. Step 3.2, at every verification point: Baseline, Target Checks, run, compare.
* **`principle-sequence-verifiable-units`**: steps 3.1 and 3.2. Each `task` entry ends in a state its `verification` entry can check before anything that waits for it starts.
* **`principle-guard-the-context-window`**: step 3.2. The Execution Agent hands every long piece of work to a fresh agent and keeps only its report.
* **`principle-separate-before-serializing-shared-state`**: step 3.2. One branch per Task and one writer for the shared files.
* **`principle-never-weaken-a-check`**: step 3.2. A failing Check is fixed in the code, never in the Check.
* **`principle-never-block-on-the-human`**: step 3.2. An entry that waits for the user never stops the entries that do not depend on it.




