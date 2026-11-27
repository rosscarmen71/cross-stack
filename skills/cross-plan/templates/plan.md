
# Plan: <title>

**id:** <plan id>  ·  **Date:** <YYYY-MM-DD>  ·  **Status:** <string status>

>Note: all plans within a project must have a unique numeric `id`.

>Note: the `date` of the plan is the date that the plan was created by the user unless otherwise specified.

>Note: the status options are as follows:
* `writing in progress`: the plan is not completed being written, you are currently in the process of writing the plan or creating the demos and verification specifications.
* `draft`: the plan is done being written, but has not been approved to be acted upon yet.
* `approved`: the plan has been approved and may be acted upon.
* `in progress`: one or more agents (or a person) are currently working to fulfill the plan. 
* `paused`: the plan was previously `in progress`, however, progress has put been put on hold for the time being. The plan has had some work towards it, but is not complete and is not currently being worked on.
* `done`: the plan has been fully executed and approved by the user as being completed either meaning that all of the requirements have successfully been met.
* `discarded`: the plan is no longer wanted. All progress towards the plan, user or agent wise should be halted and not resumed.

## Goal
<description>

>Note: the `goal` field should contain a one sentence to one paragraph description of what the goal of this plan is. The focus is on defining what is going to be accomplished, but side notes can be added for any specifications that are really central to how it is supposed to be accomplished.

## Why
- **Why the user wants this:** <reason>
- **Why this result:** <reason>
- **Why this approach:** <reason>

>Note: the `why` field records the reasons the user gave for wanting the change (filled in at step 1.1), for wanting this specific result (steps 1.3 and 1.4), and for wanting it done this way (step 3). Use these reasons to guide decisions and verification during execution. Only record reasons the user actually gave.

## Out of scope
- <item>

>Note: the out of scope section contains things that are out of scope of this plan. This can include things like files that it can't edit files, It can't read as well as things that are not part of the tasks things that are not part of the plan decisions that are not part of the plan, etc.

## Permissions
- <description> or <rule>

>Note: the `permissions` field is optional. It covers only how the code may be edited: which files may be changed, what kinds of change are allowed, installing dependencies, destructive edits and similar. Record a permission here only if the user explicitly forbids something (for example a file that must not be changed) or explicitly allows something an executor would normally have to ask for. Anything not listed falls back to the executor's default. How work is shipped (commits, branches, pushing, PRs) never goes here: it is a Run Setting that belongs to the Execution Agent.



## Final State
<description>

> Note: the `final state` field should contain a description of one sentence to one paragraph of what the final state after the request and plan is completed should be.

### Final Verification
- <spec> or <rule> or <demo> etc.

> Note: the `final verification` sub field of the `final state` field should contain a list of paths to demos of the final global state, explicit rules that defined the intended final state and any other form of specification defining what the final state after the request is met should look like. These are basically all the parameters that are needed to create a verification later once the execution agent passes over this plan.



## Current Delta
- <difference> or <image> or <video> or <path> or <code_segment> etc.

> Note: the `current delta` field contains a list of evidence and descriptions and links to how and where the code base in its current state needs to change to match the intended outcome of the plan defined by the `final state` and `final verification` layer and `goal`.



## How 
- <item> or <specification> or <rule> etc.
- <item> or <specification> or <rule> etc.
- ...



## Road Map

### Road map evolution



## Problem Solving Philosophy
- <category>: <rule> or `use the executor's default`
- Task <task id>: <rule>
- ...

>Note: the `problem solving philosophy` field contains the user's rules for handling problems tied to this plan's own work. It has one entry for each category asked about in step 5 (regression, demo can't be reached or looks wrong, scope creep, permissions), plus any rule for a risky part of the work. If the user had no preference, the entry says `use the executor's default`. The defaults themselves are not defined here. How a run behaves in general (how often it retries, when it contacts the user, how it decides an open choice) never goes here: those are Run Settings that belong to the Execution Agent. Rules apply to the whole plan unless they start with a `Task` id, in which case they apply only to that `Task`.

