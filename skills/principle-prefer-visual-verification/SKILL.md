---
name: principle-prefer-visual-verification
description: Apply when choosing how to verify user-facing behavior (a UI, a CLI, an API response a person reads). Drive the real surface and capture what the user would see; fall back to internal Checks only when the surface cannot be driven, and say why.
disable-model-invocation: true
---

# Prefer Visual Verification

When behavior is user-facing, the strongest Check drives the real surface the way a user would and captures what the user would see: a screenshot, a recording, a terminal transcript, a rendered response. A Check on internals (a function's return value, a store's contents) can pass while the screen is blank, so for user-facing behavior it is the weaker proof.

Capture the action and the resulting state, not just the final screen, and store the capture as Evidence a human or an agent can review without re-running. Where a plan has a visual demo, the visual Check walks the same flow and captures at the same points, so the two can be compared side by side.

Fall back to non-visual Checks only when the surface cannot be driven (no harness, no display, behavior with no visible effect), and record the reason in the Coverage Map so the gap stays visible instead of forgotten.
