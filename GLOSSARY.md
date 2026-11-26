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

**Problem Solving Philosophy**:
A plan's rules for how an executor handles problems during execution, such as repeated failures, regressions or scope creep. Anything the user doesn't specify falls back to the executor's default.
_Avoid_: Emergency Strategy
