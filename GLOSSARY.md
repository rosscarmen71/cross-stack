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

**Coverage Map**:
The file in the `Verification Layer` that lists every `final verification` item of a plan and its `Tasks`, with the `Checks` that prove it and where their `Evidence` lands.
_Avoid_: traceability matrix, test plan

**Execution Stream**:
An ordered sequence of whole `Tasks`, plan-level work, `Check` runs and utility work that one executor carries out in turn. Streams run in parallel where the Road Map allows and merge where work depends on more than one of them.
_Avoid_: workstream, lane, track

**Stream Entry**:
One element of an `Execution Stream`: a whole `Task`, a `Check` run, plan-level work or utility work, which may wait on entries in other streams.
_Avoid_: step, stream task, stream item

**Entry Label**:
The single tag a `Stream Entry` may carry to name its kind of work: `plan`, `task`, `verification` or `utility`.
_Avoid_: global (use `plan`), local (use `task`), tag, category

**Actionable Item**:
One concrete piece of work that a `Task` or other stream work is broken into just before it is carried out, smaller than a `Task`.
_Avoid_: to-do, todo, sub-task, step

**Problem Solving Philosophy**:
A plan's rules for how an executor handles problems during execution, such as repeated failures, regressions or scope creep. Anything the user doesn't specify falls back to the executor's default.
_Avoid_: Emergency Strategy
