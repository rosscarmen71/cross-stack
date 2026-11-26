---
name: grill-non-negotiables
description: Interrogate the user to uncover every non-negotiable in a request, plan, design or task, and push each one until it is as specific and measurable as the user will agree to.
disable-model-invocation: true
---

Given an input of a request, plan, design or task, interview the user relentlessly until every possible non-negotiable has been found and made as specific and measurable as the user will agree to. Map this as a **design tree**: every decision branches into potential non-negotiables the user may have but has not yet thought of or specified.

Work the tree in **rounds**. The **frontier** is every decision whose prerequisites are already settled: the questions you can ask _now_ without guessing at answers you haven't heard yet. Ask the whole frontier in one round: number each question and give your recommended answer. Then wait for the user's answers before the next round.

Format a round like so:

```
❓ **Q1** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices, always include the option to get an explanation or example and then ask again>

➡️ <your recommended answer>

---

❓ **Q2** - **<question title>**: <question body, might be multiple paragraphs, including multiple choices, always include the option to get an explanation or example and then ask again>

➡️ <your recommended answer>
```

Word each question so "yes" accepts your recommended answer.

Each round the user answers reshapes the tree: settled decisions push the frontier outward and unblock questions that depended on them. Recompute the frontier and ask the next round. A question whose answer depends on another question still open in this round belongs to a _later_ round, not this one.

Finding _facts_ is your job, never the user's. When a frontier question needs a fact from the environment (filesystem, tools, etc.), dispatch a sub-agent with a fresh context window to find it; don't ask the user for anything you could look up yourself. Don't block on it: a running exploration is an unsettled prerequisite, so only the questions downstream of it wait for the sub-agent to report; ask the rest of the frontier now. The _decisions_ are the user's: put each to them and wait.

The session is done when the frontier is empty: every branch of the design tree visited, every non-negotiable pushed as far as the user will agree, nothing left silently assumed, and no conflicts left. Do not act on it until the user confirms you have reached a shared understanding.

## Definition

A **non-negotiable** is either:
* a requirement whose violation makes the result unacceptable, however good everything else is, **or**
* anything the user explicitly declares a non-negotiable, even if the result would still work without it.

Preferences and "nice to haves" are **not** non-negotiables. Do not record them.

## Scope

Cover whatever the input contains. If it includes both `what` and `how` (for example a full plan), cover both. If it is only a request, ask about `what` first, then `how`. There is no scope parameter. The user can call this skill directly without specifying anything.

## Process

Each part below feeds questions into the frontier.

1. **Confirm what's already implied.** Read the whole input: demos, specs, plan fields, how decisions. List every candidate non-negotiable it already states or implies, and ask the user to confirm whether each one is a non-negotiable.
2. **Probe the categories.** Ask about each category that applies to the input, and skip the rest:
   * existing behavior that must not change
   * must-have features
   * must-never-happen (data loss, security, privacy)
   * compatibility (platforms, browsers, versions)
   * performance limits
   * look and feel / brand
   * accessibility
   * legal / compliance
   * dependencies and tools (`how` only)
3. **Find undecided choices, element by element. This is the most important part.** Go through every element of the input, one at a time: each demo screen, behavior, rule, spec item and how decision. **Identify every choice that has not been decided yet.** Examples: exact wording, a color, the order of steps, a field name, a default value, a library, a file layout. For each one, grill the user on whether they have a preference and whether it is a non-negotiable. This is how you find the non-negotiables the user hasn't thought of.
4. **Push back on every non-negotiable. This is equally important.** For every non-negotiable, whether it was already stated or newly found, push back hard until it is in its **most specific, most measurable** form. That means specific and comparable in the way a demo is. For example, "it must be fast" → "the settings page loads in under 1s on the demo data". Stop pushing only when it is fully specific and measurable, **or** the user clearly and explicitly says they are satisfied with it as it is, even if it is not perfectly measurable.
5. **Ask why.** After each non-negotiable is confirmed, ask why. Record the reason only if it is substantive, meaning it could inform decisions later. If the user skips the question or gives a trivial answer, move on.

## Conflicts

A non-negotiable that contradicts the input (a demo, a spec, a plan field, or another non-negotiable) is an **immediate red flag**. Stop and interrogate the user until they decide what they want and the conflict no longer exists.

Then update every affected file yourself, including demos, specs and plan fields. If a demo was made from a demo playbook, rebuild it by following that playbook's `Creation Steps`, and show it to the user with its `Show User Steps`.

## Non-Negotiables

- **Push for specific and measurable**: never accept a vague non-negotiable until the user explicitly says it is good enough.
- **Never leave a choice silently assumed**: every undecided choice in the input is put to the user.
- **Never record a preference as a non-negotiable**: only record what meets the Definition.
- **Resolve every conflict before finishing**: no non-negotiable may contradict the input or another non-negotiable.

## Return

Return an unnumbered list in chat, with one item per non-negotiable:

- **Statement:** <the non-negotiable, as specific and measurable as agreed>
  - **Scope:** `what` or `how`
  - **Verify by:** <the demo, test or rule that proves it is met>
  - **Why:** <only if the reason was substantive>
  - **Source:** stated by the user, or confirmed from the input

End with **Files updated**: every file changed while resolving conflicts, or "none".

Do not save the list anywhere. The calling skill or the user decides where it goes.
