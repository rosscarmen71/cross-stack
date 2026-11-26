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

STEPS:

## 0. Specify Agents and Atonomy

This is where you would specify the settings for like the judge agent, for example, where you would specify the model you wanted to use in any particular non-standard requirements for a cross-judge-chef agent that you want, ect. They should also be where you were explicitly asked about, for example what to do if a dispute remains or arises, if you don't specify, it'll use the default for the agent which will I'll want to make it so that just keeps trying to handle it and it uses primarily skills from potato stack which I will add to shift. It's focused towards fixing whatever bugger dispute there is. But for example, like if you wanna be really conservative involved in the number of tokens, you use you could specify at the beginning that like you don't want it to keep retry and you want to contact you the second there's a problem and take a pause, etc., rather than the standard of trying to figure it out by itself.
... # TODO

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

STEPS: 

## 4. Verify and Improve Loop

## 5. Wrap Up





## Task Playbooks

## Principles




