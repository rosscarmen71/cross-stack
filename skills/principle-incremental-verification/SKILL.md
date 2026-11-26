---
name: principle-incremental-verification
description: Apply when designing or writing Checks for work that will be built in pieces. Make every Check modular and runnable on its own, so each Task or feature can be verified the moment it exists, without waiting for the whole plan.
disable-model-invocation: true
---

# Incremental Verification

A Verification Layer is for use while building, not only at the end. Design Checks so that any subset runs on its own: one Task's Checks, one feature's Checks, one Check. An executor who has finished Task 3 of 7 must be able to run Task 3's Checks and learn whether that piece is right before touching Task 4.

That requires modularity. A Check depends only on the behavior it asserts and the fixtures it sets up, never on another Check having run first or on the whole plan being complete. Group Checks by the Task and the `final verification` item they prove, name them so that group is selectable from the runner, and record the per-group run command in the Coverage Map.

The order in which Checks are written does not matter. What matters is that at every point during execution there is something runnable that answers "is what I have built so far correct?" for exactly the part that exists.
