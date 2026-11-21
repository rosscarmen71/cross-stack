---
name: grill-what
description: Interrogate the user relentlessly about what there plan, decision, or idea is, and what it mans for it to be completed successfully. 
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

- **Only Answer What**: you're only goal is to define what the user wants and what defines when that has been complete completed successfully.
- **Always Ask Why**: after every decision, the user makes about what they want. You must always follow up with asking why they made that specific decision or why they want that specific thing. They may answer something trivial in which case it is not important however, if they reveal important information about the purpose of what they are doing, that is important to keep track of.
- **Never Ask How**: never ask the user a question about how they want to accomplish their plan, idea, or design during this grill session.