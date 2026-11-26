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

## 1. Identify The Plan or Task

GOALS: 
* Identify if the user wants to execute a full plan and all of it sub tasks, a singular task, or a selection of tasks from a plan.
* Identify the specific plan, task, or plan and tasks by ID within the current project.
* Update the states of the plans and tasks to `in progress`.

STEPS:
1. The input for this skill should include identification of the task or full plan that is to be executed. If it was not provided in the input or was unclear in any way, ask the user for the plan or plan and task ID. Additionally, if it was unclear of the user wanted to complete an entire plan or just a task of a plan ask the user explicitly.
2. Identify and explicitly confirmed with the user the ID of the plan or task that is to be executed is correct.
3. Update the state of the plan and or task/s to `in progress`.

## 2. Create The Verification Layer

GOALS:

NON-NEGOTIABLE PRINCIPLES:

Read the instructions for finding `principle-*` skills and engaging them [here](#principles). **Throughout the entirety of the step engage the following principles:**
* `/principle-extensive-verification`
* `/principle-increment-verification`
* `/principle-prefer-visual-verification`


STEPS: 


## 3. Execute The Plan

## 4. Verify and Improve Loop

## 5. Wrap Up





## Task Playbooks

## Principles




