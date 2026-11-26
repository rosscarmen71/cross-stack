
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

>Note: this section is optional if it is left empty or anything that is not specified here it's assumed just to use the default by whatever executor is running this plan. Things that would be included. Here would be if the user explicitly mentions that certain permissions are not allowed maybe there's a certain thing that you're not allowed to change. For example, it can also allow extra permissions like permissions that you typically would have to ask for maybe there's an explicit bypassing that the users is going to allow this information would go here if that were the case.



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

>Note: the `problem solving philosophy` field contains the user's rules for handling problems during execution. It has one entry for each category asked about in step 5 (repeated failure, regression, demo can't be reached or looks wrong, scope creep, permissions, communication, undecided choices). If the user had no preference, the entry says `use the executor's default`. The defaults themselves are not defined here. Rules apply to the whole plan unless they start with a `Task` id, in which case they apply only to that `Task`.

