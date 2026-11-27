---
status: accepted
---

# Run Settings belong to the Execution Agent, never to the plan

How a run behaves in general (how many agents work at once, which models they use, how often it retries, when it contacts the user, how it decides an open choice, how it ships) is a Run Setting. Every Execution Agent starts from its default settings, the user may change any of them when starting a run, and the result lives only in that Execution Agent. A plan never holds Run Settings: its Problem Solving Philosophy covers only problems tied to its own work, such as a regression, scope creep or a risky Task. We chose this because the same plan may be run more than once, by different Execution Agents with different budgets and levels of supervision, and each of those runs must be able to behave differently without the plan being rewritten.

## Considered Options

- **The plan owns behaviour, the run owns only resources.** `/cross-plan` already questions the user about retries and communication, so nothing would be asked twice, but a cautious or cheap run of an approved plan would require editing the plan.
- **Both, with the plan taking priority.** Keeps every existing plan valid, but the same question is asked in two places and a reader has to check both to know how a run will behave.

## Consequences

- `/cross-plan` must stop asking about repeated failure, communication and undecided choices, and its plan template must drop them from the Problem Solving Philosophy.
- Nothing in a plan may say "use the executor's default" for those topics, since the plan no longer mentions them.
- Because the settings live only in the Execution Agent, a fresh Execution Agent that takes over a paused plan starts from the defaults again and the user is asked again.
- Extends ADR 0002, which made the same split for shipping.
