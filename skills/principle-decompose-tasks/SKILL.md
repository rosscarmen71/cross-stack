---
name: principle-decompose-tasks
description: Apply when a unit of work is large enough that one pass would be hard to verify or review (a whole verification layer, a plan, a sweep). Break it into modular sub-tasks and mark which can run in parallel and which must run in sequence. The breakdown is not a decision to spawn sub-agents.
disable-model-invocation: true
---

# Decompose Tasks

Large work done in one pass is hard to verify, hard to review and easy to drift. Before starting, break it into modular sub-tasks, each with its own definition of done, its own files and its own Check. "Write the verification layer for plan 7" becomes one sub-task per Task of the plan, or per feature, each finished and verified on its own.

Then mark the dependencies. Two sub-tasks are **sequential** when they touch the same files or one depends on what the other produces. Every other pair is **parallel-safe**. Record this with the breakdown, because it is what lets an executor, or several, work independent pieces at the same time without stepping on each other. Where two sub-tasks would share a file, split the file or the sub-task first (see `principle-separate-before-serializing-shared-state`).

The breakdown describes what *could* run in parallel. Whether anything actually fans out to sub-agents is a separate decision that belongs to the executing agent and its autonomy settings, not to this principle.
