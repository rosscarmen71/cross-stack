---
name: grill-what
description: Interrogate the user relentlessly about what they want from a request, plan or idea, and what it means for it to be done successfully. Never asks how.
disable-model-invocation: true
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

- **Only answer `what`**: your only goal is to define what the user wants, and what it means for it to be done successfully.
- **Never ask `how`**: never ask the user how they want to accomplish their plan, idea or design. If the user volunteers an answer about `how`, do not grill it further. Keep it and return it under **How notes**.
- **Push for specific and comparable**: push back on every vague answer until it is as specific as the user will agree to. The goal is that a result can be clearly **compared** against it to tell whether it is met. Measurable is good when it fits, but most decisions are not numeric. Stop pushing only when it is specific, or the user explicitly says it is good enough.
- **Always ask why**: after every decision the user makes about what they want, ask why. Record the reason only if it is substantive, meaning it could inform decisions later. If the user skips the question or gives a trivial answer, move on.
- **Confirm done**: before finishing, read back the **Done when** list (the criteria that mean the request has succeeded) and get the user's explicit agreement.

## Return

Return an unnumbered list in chat:

- **Decision:** <what the user wants, as specific as agreed>
  - **Why:** <only if substantive>

Then:
- **Done when:** <each agreed success criterion>
- **How notes:** <every `how` answer the user volunteered, or "none">

Do not save the list anywhere. The calling skill or the user decides where it goes.
