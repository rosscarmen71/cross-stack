---
name: principle-never-weaken-a-check
description: Apply whenever you are about to edit, delete or skip an existing Check, or add a mock to one. A Check may change only when the plan changes the behavior it asserts, and only to become stricter or to fail now for the right reason. Never to pass more easily.
disable-model-invocation: true
---

# Never Weaken a Check

A Check exists to fail when the behavior it asserts is wrong. Any edit that makes it fail less often, for any reason other than the plan having changed the asserted behavior, is weakening, and is forbidden however the failure is explained. Weakening includes deleting an assertion, widening a tolerance or timeout, skipping or commenting out a case, loosening a selector or regex, catching and ignoring an error, replacing a real boundary with a mock the production code does not have, and turning a failing Check green before the plan is executed.

The one legitimate reason to change a Check is that the plan changes the behavior it asserts. Then the Check is rewritten to assert the new behavior, which makes it stricter or red now, and the change is recorded in the Coverage Map as `altered` with the `final verification` item that justifies it.

When a Check fails and you believe the Check is wrong, prove it: show that what the Check asserts contradicts the plan or spec, with a citation. Without that proof the Check stands and the code is what changes.
