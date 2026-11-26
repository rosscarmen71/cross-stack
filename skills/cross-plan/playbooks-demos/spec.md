# Demo Playbook: Spec

## When To Use
* **As a demo on its own:** only when every visual playbook is impractical, such as for back-end behavior, data rules, APIs, configuration, or internal refactors with no visible change.
* **As an attached spec file:** always. Every demo from every other playbook has a `<nn>-<slug>.spec.md` that follows the structure below.

## Output
- **Files:** one Markdown file.
- **Naming:** `<nn>-<slug>.spec.md`
- **Location:** `plan/plan-<id>/demos/` for global demos, `plan/plan-<id>/task-<n>-demos/` for a `Task`'s demos

## Creation Steps
1. Create the file with exactly these sections, in this order:
   ```markdown
   # Spec: <title>

   ## Behaviors
   - **Given** <starting state>, **when** <action or event>, **then** <expected result>.

   ## Rules
   - <a rule that must always hold>

   ## Examples
   | Input | Expected output |
   |-------|-----------------|
   | <input> | <expected output> |

   ## Not shown in the demo
   - <anything the attached demo cannot show or had to fake>
   ```
2. Write each behavior, rule and example so that an executor could turn it directly into a test. Avoid vague words like "fast", "nice" or "correct" unless they are defined.
3. When the spec is attached to another demo, fill in `Not shown in the demo` with everything that demo cannot show. When the spec is the demo itself, write `n/a` there.
4. Check that every behavior described in `what` so far appears in `Behaviors`, `Rules` or `Examples`.

## Show User Steps
1. Summarize the spec for the user in chat: the behaviors, the rules, and a few of the most important examples.
2. Give the user the path to the spec file.

## Spec File
This playbook is the spec file. A spec used as a demo on its own needs no separate spec file.
