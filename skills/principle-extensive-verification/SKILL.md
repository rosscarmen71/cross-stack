---
name: principle-extensive-verification
description: Apply when making changes to a project verification layer. Every requirement, rule and example gets its own Check, and coverage is proven by a Coverage Map, not asserted.
disable-model-invocation: true
---

# Extensive Verification

Every requirement, rule and example row in a plan or Task becomes its own Check. Nothing is "obviously fine", "covered by the integration test" or "too small to test". If the plan says it, a Check proves it. 

Coverage is a claim, so it needs evidence. Keep a Coverage Map that pairs every requirement with the Check that proves it and the Evidence that Check produces. An item with no Check is uncovered, whatever anyone remembers testing by hand. A Check with no item is either noise or a missing requirement; find out which.

Breadth beats cleverness. Ten plain Checks that each prove one thing are worth more than one ingenious Check that proves several, because when the clever one fails nobody knows which requirement broke.
