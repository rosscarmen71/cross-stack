# Cross Skills

Custom planning and grilling skills. These terms are the language the skills (starting with `/cross-plan`) use for plans and their parts.

## Language

**Task**:
One unit of work listed in a plan's Road Map. It is separately testable, has its own demo, file scope and task scope, and is identified as `<plan-id>.<n>`.
_Avoid_: sub-task, sub task

**Sub-plan**:
The file that fully describes one Task, stored at `plan/plan-<id>/task-<n>.md` and created from `templates/sub-plan.md`.
_Avoid_: task file, task plan

**Demo**:
A file that shows the intended final behavior of a plan or `Task`, always paired with a `.spec.md` for anything it cannot show.
_Avoid_: mockup, demo object

**Demo Playbook**:
The instructions for creating and showing one format of `Demo`, kept in `playbooks-demos/`.
_Avoid_: demo type

**Non-negotiable**:
A requirement whose violation makes the result unacceptable, or one the user explicitly declares mandatory even if the result would still work without it.
_Avoid_: hard requirement, must-have

**Verification Layer**:
Everything runnable in a project that proves whether its current state matches the `final verification` of a plan or `Task`. It passes only when every item is met.
_Avoid_: test suite, tests folder

**Check**:
One runnable verification tied to exactly one requirement, such as a test, a script, or a scripted drive of the real app. A requirement listed in both a plan and a `Task` is still one requirement.
_Avoid_: verification, comparison, test (as the generic word)

**Guard Check**:
A `Check` for a requirement that is already true before execution and must stay true, so it passes from the start instead of failing first.
_Avoid_: regression test, invariant test

**Evidence**:
The stored output of one `Check` run that lets a human or an agent judge it without running it again.
_Avoid_: artifact, log

**Baseline**:
The set of `Checks` that pass at a given moment, recorded before work starts so that the next run can show whether anything that passed has stopped passing.
_Avoid_: snapshot, known-good set

**Target Checks**:
The `Checks` that a piece of work is expected to make pass once it is finished. A piece of work may have none.
_Avoid_: expected passes, goal tests

**Coverage Map**:
The file in the `Verification Layer` that lists every `final verification` item of a plan and its `Tasks`, with the `Checks` that prove it and where their `Evidence` lands.
_Avoid_: traceability matrix, test plan

**Execution Queue**:
The list of everything one run of a plan will carry out, where each entry names the entries it must wait for. An entry may start as soon as everything it waits for is done.
_Avoid_: execution stream, workstream, lane, track

**Queue Entry**:
One element of an `Execution Queue`: a whole `Task`, a `Check` run, plan-level work or utility work.
_Avoid_: stream entry, step, queue task, queue item

**Entry Label**:
The single tag a `Queue Entry` may carry to name its kind of work: `plan`, `task`, `verification` or `utility`.
_Avoid_: global (use `plan`), local (use `task`), tag, category

**Actionable Item**:
One concrete piece of work that a `Queue Entry` is broken into just before it is carried out, smaller than a `Task`.
_Avoid_: to-do, todo, sub-task, step

**Execution Playbook**:
A pre-set method for breaking one kind of `Queue Entry` into `Actionable Items`, kept in the folder for its `Entry Label` and chosen by its `When To Use` section.
_Avoid_: entry playbook, task playbook, verification playbook (these collide with `/cross-verification`'s own playbooks)

**Execution Agent**:
The agent that runs one plan's execution: it works through the `Execution Queue`, starts each entry when it is ready and is the only writer of the plan's shared files.
_Avoid_: execute agent, orchestrator, stream coordinator

**Execution Sub-agent**:
An agent spawned by the `Execution Agent` to carry out one `task` entry together with its `verification` entry.
_Avoid_: execute sub-agent, stream agent, worker

**Judge**:
A fresh agent that knows nothing of the reasoning of the agent whose work it examines. It runs `Checks` or reviews coverage and reports a verdict, and never changes what it examines.
_Avoid_: reviewer, verifier, grader

**Run Settings**:
The settings one `Execution Agent` runs with: which agents do the work, how many at once, how the run behaves when it is stuck, and how it ships. Each `Execution Agent` starts from its defaults and holds any changes the user asked for. They are never part of the plan.
_Avoid_: agent and autonomy settings, settings block, config

**Problem Solving Philosophy**:
A plan's rules for handling problems tied to its own work, such as regressions, scope creep or a risky `Task`. How a run behaves in general when it is stuck (retries, when to contact the user, undecided choices) belongs to the `Run Settings`.
_Avoid_: Emergency Strategy
