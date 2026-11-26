# Demo Playbook: Slides

## When To Use
* The final state is best explained as a sequence: before and after, a structure or architecture, a process, or a set of options.
* Parts of the request are about organization or design rather than interaction.
* Not for behavior the user needs to click through. Use `interactive-prototype.md` for that.

## Output
- **Files:** one self-contained `.html` slide deck, plus its spec file.
- **Naming:** `<nn>-<slug>.slides.html` and `<nn>-<slug>.spec.md`
- **Location:** `plan/plan-<id>/demos/` for global demos, `plan/plan-<id>/task-<n>-demos/` for a `Task`'s demos

## Creation Steps
1. Outline the deck: one slide per section of the final state you need to show. Start with a summary slide and end with a "what done looks like" slide.
2. Build a **single self-contained HTML file**: all CSS and JavaScript inline, no external requests, one slide visible at a time, and the left and right arrow keys move between slides. Show the slide number on each slide.
3. Use diagrams, mockups, before/after comparisons and short text. Keep each slide to one idea.
4. Open the file yourself and check that every slide renders and the arrow keys work.
5. Write the spec file, following the `Spec File` section below.

## Show User Steps
1. Open the deck for the user with `open <path>`, and tell them to use the arrow keys.
2. Give the user a one-line summary of each slide so they know what to look for.
3. Give the user the path to the spec file and summarize what it adds beyond the slides.

## Spec File
Covers the detailed rules, behaviors and examples behind what the slides summarize. Follows the structure in `./playbooks-demos/spec.md`.
