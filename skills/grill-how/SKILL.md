---
name: grill-how
description: Interrogate the user relentlessly about how an already-defined request, plan or task should be accomplished. Never asks what.
---

Interview the user relentlessly until you reach a shared understanding. Map this as a **design tree**: every decision branches into the decisions that hang off it.

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

The session is done when the frontier is empty: every branch of the design tree visited, nothing left silently assumed. Do not act on it until the user confirms you have reached a shared understanding.

## Non-Negotiables

- **Only answer `how`**: your only goal is to define how the user wants the work to be done: methods, tools, structure, conventions and rules.
- **Never ask `what`**: `what` is assumed to be already clearly established by the input. If the input does not clearly define `what`, say so before asking anything, and offer to run **`/grill-what`** first instead of guessing.
- **Flag `what` changes**: if an answer would change `what` the user wants, that is a red flag. Stop, tell the user it changes `what`, and return it under **What changes** so the caller can re-grill `what`. Do not quietly apply it.
- **Ground recommendations in the codebase**: base every recommended answer on the project's existing patterns and conventions, found by a sub-agent. Present the real options with a one-line trade-off each, and say clearly when a recommendation breaks an existing convention.
- **Push for specific and comparable**: push back on every vague answer until it is as specific as the user will agree to. The goal is that the finished work can be clearly **compared** against it to tell whether it was followed. Measurable is good when it fits, but most `how` decisions are not numeric. Stop pushing only when it is specific, or the user explicitly says it is good enough.
- **Always ask why**: after every decision the user makes about `how`, ask why. Record the reason only if it is substantive, meaning it could inform decisions later. If the user skips the question or gives a trivial answer, move on.

## Return

Return an unnumbered list in chat:

- **Decision:** <the method, tool, structure or rule, as specific as agreed>
  - **Why:** <only if substantive>

Then:
- **What changes:** <every answer that would change `what`, or "none">

Do not save the list anywhere. The calling skill or the user decides where it goes.
