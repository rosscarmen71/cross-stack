# Demo Playbook: Interactive Prototype

## When To Use
* The request adds or changes something the user interacts with: a UI, a page, a form, a flow, a tool.
* The user needs to click through the behavior to judge whether it is right.
* Not for back-end-only or non-visual behavior. Use `spec.md` for that.

## Output
- **Files:** one self-contained `.html` file, plus its spec file.
- **Naming:** `<nn>-<slug>.html` and `<nn>-<slug>.spec.md`
- **Location:** `plan/plan-<id>/demos/` for global demos, `plan/plan-<id>/task-<n>-demos/` for a `Task`'s demos

## Creation Steps
1. List the screens, states and interactions the prototype must show, based on `what` has been defined so far.
2. Build a **single self-contained HTML file**: all CSS and JavaScript inline, no build step, no external requests, and fake data in place of real back ends.
3. Make every interaction from step 1 work in the prototype. Show the intended final behavior, including empty, loading and error states where they matter.
4. If the project has an existing look (colors, fonts, layout), match it closely enough that the user can judge the result in context.
5. Open the file yourself and check that every interaction works and nothing is broken.
6. Write the spec file, following the `Spec File` section below.

## Show User Steps
1. Open the prototype for the user with `open <path>`.
2. Give the user a short numbered list of **things to try**, one per interaction from creation step 1.
3. Give the user the path to the spec file and summarize what it adds beyond the prototype.

## Spec File
Covers everything the prototype cannot show: real data sources, persistence, performance, permissions, edge cases not built into the prototype, and any behavior that was faked. Follows the structure in `./playbooks-demos/spec.md`.
