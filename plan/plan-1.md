
# Plan: Update the README

**id:** 1  ·  **Date:** 2026-11-25  ·  **Status:** approved

>Note: all plans within a project must have a unique numeric `id`.

>Note: the `date` of the plan is the date that the plan was created by the user unless otherwise specified.

>Note: the status options are as follows:
* `writing in progress`: the plan is not completed being written, you are currently in the process of writing the plan or creating the demos and verification specifications.
* `draft`: the plan is done being written, but has not been approved to be acted upon yet.
* `approved`: the plan has been approved and may be acted upon.
* `in progress`: one or more agents (or a person) are currently working to fulfill the plan. 
* `paused`: the plan was previously `in progress`, however, progress has put been put on hold for the time being. The plan has had some work towards it, but is not complete and is not currently being worked on.
* `done`: the plan has been fully executed and approved by the user as being completed either meaning that all of the requirements have successfully been met.
* `discarded`: the plan is no longer wanted. All progress towards the plan, user or agent wise should be halted and not resumed.

## Goal
Rewrite `README.md` so a visitor browsing the repo quickly understands what the repo contains and how to find and use its skills. It covers the 6 working skills, the planned placeholder skills, how the skills relate (shown in diagrams), the repo layout, install steps, an example run, and credit for the third-party skills. Also add `plan/` to `.gitignore`.

>Note: the `goal` field should contain a one sentence to one paragraph description of what the goal of this plan is. The focus is on defining what is going to be accomplished, but side notes can be added for any specifications that are really central to how it is supposed to be accomplished.

## Why
- **Why the user wants this:** the README should be easy to understand and give an overall description of what is in the repo, so things are easy to find.
- **Why this result:** not given.
- **Why this approach:** not given.

>Note: the `why` field records the reasons the user gave for wanting the change (filled in at step 1.1), for wanting this specific result (steps 1.3 and 1.4), and for wanting it done this way (step 3). Use these reasons to guide decisions and verification during execution. Only record reasons the user actually gave.

## Out of scope
- Repeating the step-by-step contents of any `SKILL.md` or of `GLOSSARY.md` in the README. Link to them instead.
- Mentioning this repo's own `plan/` folder in the README.
- Detailed descriptions of the placeholder skill folders (they get one short "Planned" list).

>Note: the out of scope section contains things that are out of scope of this plan. This can include things like files that it can't edit files, It can't read as well as things that are not part of the tasks things that are not part of the plan decisions that are not part of the plan, etc.

## Permissions
- Don't commit or push. The user decides when changes go public.
- Never run install commands against the real `~/.claude/skills/`. Only use a temp `HOME`.
- Everything else: use the executor's default.

>Note: this section is optional if it is left empty or anything that is not specified here it's assumed just to use the default by whatever executor is running this plan. Things that would be included. Here would be if the user explicitly mentions that certain permissions are not allowed maybe there's a certain thing that you're not allowed to change. For example, it can also allow extra permissions like permissions that you typically would have to ask for maybe there's an explicit bypassing that the users is going to allow this information would go here if that were the case.



## Final State
`README.md` matches the approved demo. A visitor can tell from the first lines what the repo is and that `/cross-plan` is the main skill, and can find every working skill, see how the skills relate in two diagrams GitHub renders, understand the repo layout, install the skills, see an example run, and find third-party credits. `.gitignore` lists `plan/`.

> Note: the `final state` field should contain a description of one sentence to one paragraph of what the final state after the request and plan is completed should be.

### Final Verification
- `plan/plan-1/demos/01-readme.html`: approved mock-up of the new README as rendered on GitHub. The real README must match its sections, order and content.
- `plan/plan-1/demos/01-readme.spec.md`: behaviors, rules and examples the README must meet, including what the mock-up can't show (Mermaid diagrams, real links, `.gitignore`).
- The first 5 lines say what the repo is (Claude Code skills for planning by questioning the user), and preferably name `/cross-plan` as the main skill.
- All 6 working skills are listed, each with a short, plain-language description and a link to its `SKILL.md`.
- There is one short "Planned" list of the placeholder folders.
- Two diagrams, which skill calls which and cross-plan's steps 0–6, render as pictures on GitHub. No line overlaps text.
- A repo layout tree has a one-line note per entry.
- Following the install steps lets a reader type `/cross-plan` in a new session.
- There is an example command and a description of the `plan/plan-<id>/` folder it produces.
- A third-party section credits Matt Pocock and pstack (Lauren Tan) and links both license files.
- No term is used without a definition or a link to `GLOSSARY.md`. Skill steps aren't repeated.
- The README doesn't mention this repo's own `plan/` folder.
- `plan/` is in `.gitignore`.
- **Non-negotiables** (from step 3.2):
  - The first 5 lines say what the repo is. Naming `/cross-plan` there is preferred but not required. *Verify by:* reading lines 1–5.
  - All 6 working skills are listed, and every link resolves to an existing file. *Verify by:* checking each relative link path exists.
  - Both Mermaid diagrams render as pictures on GitHub, with no line overlapping text. *Verify by:* viewing the README on GitHub, or in a Mermaid preview.
  - The install commands run without errors on a machine with no `~/.claude/skills/`, and end with `/cross-plan` available in a new session. *Verify by:* running them in a fresh shell with `HOME` pointed at an empty temp folder.
  - Third-party credit names Matt Pocock and pstack (Lauren Tan) and links both license files. *Verify by:* reading the section and following the links.
  - `plan/` is in `.gitignore`. *Verify by:* `git check-ignore plan/plan-1.md` prints the path.
  - The README doesn't mention this repo's own `plan/` folder. *Verify by:* the only `plan/` in the README is the example output tree.
  - No local paths (`/Users/...`), email addresses or other personal details. The GitHub repo URL is the only identifying item. *Verify by:* `grep -nE "/Users/|@" README.md` finds nothing personal.
  - Every count, name and path in the README matches the repo when it's written. *Verify by:* comparing each against `ls skills/`, `ls skills/third-party/*`.

> Note: the `final verification` sub field of the `final state` field should contain a list of paths to demos of the final global state, explicit rules that defined the intended final state and any other form of specification defining what the final state after the request is met should look like. These are basically all the parameters that are needed to create a verification later once the execution agent passes over this plan.



## Current Delta
- `README.md` is 3 lines: a title and "This repo contains my custom skills and my favorite skills from others." Every section of the demo is missing: intro, skills table, Planned list, both diagrams, example, install, layout, terms and third-party credit.
- `.gitignore` exists in the repo root but is empty, so `plan/` would be committed.
- The 6 working skills exist at `skills/<name>/SKILL.md`, and the link targets exist: `GLOSSARY.md` and both `skills/third-party/*/THIRD_PARTY_LICENSE.md` files.
- `domain-modeling` is at `skills/third-party/matt_pocock/domain-modeling`, which the install step must link.
- Remote is `https://github.com/rosscarmen71/cross-stack.git`, used in the install step.

> Note: the `current delta` field contains a list of evidence and descriptions and links to how and where the code base in its current state needs to change to match the intended outcome of the plan defined by the `final state` and `final verification` layer and `goal`.



## How 
- Both diagrams are Mermaid code blocks: `flowchart TD` for which skill calls which, `flowchart LR` for cross-plan steps 0–6.
- Mermaid diagrams use no custom colors, so GitHub's theme keeps them readable in light and dark mode. The only exception is a dotted border on the third-party `domain-modeling` node.
- Every link is a relative path (for example `skills/grill-how/SKILL.md`), matching how the repo's docs already link.
- The README uses the approved demo's wording word for word. Only the diagrams (SVG → Mermaid) and the placeholder links change. (The demo's install step includes `mkdir -p ~/.claude/skills`, added in step 3.2.)
- **Non-negotiable:** only `README.md` and `.gitignore` are changed. No skill, template, glossary or license file is edited.
- **Non-negotiable:** both `skills/third-party/*/THIRD_PARTY_LICENSE.md` files stay unchanged and in place.
- Add one line, `plan/`, to the existing empty `.gitignore`. Nothing else.
- Each glossary term links to `GLOSSARY.md` on its first use only.



## Road Map
| id | Task | Sub-plan | File scope | Depends on | Parallel |
|---|---|---|---|---|---|
| 1.1 | Rewrite the README | `plan/plan-1/task-1.md` | `README.md` | none | yes, with 1.2 |
| 1.2 | Ignore `plan/` | `plan/plan-1/task-2.md` | `.gitignore` | none | yes, with 1.1 |

Coverage: both `current delta` items are covered (README → 1.1, `.gitignore` → 1.2). Every `final verification` item is covered by 1.1, except "`plan/` is in `.gitignore`", which is covered by 1.2.

### Road map evolution



## Problem Solving Philosophy
- **Repeated failure:** use the executor's default.
- **Regression:** use the executor's default.
- **Demo can't be reached or looks wrong:** if Mermaid can't lay a diagram out exactly like the demo, accept GitHub's automatic layout as long as no line overlaps text and every relationship from the demo is shown. Don't ask.
- **Scope creep:** if a fix seems to need a file other than `README.md` or `.gitignore`, stop and ask the user. Don't edit it.
- **Permissions:** run the install test only with `HOME` set to a temp folder, never against the real `~/.claude/skills/`. Don't commit or push. Check the diagrams with a local Mermaid preview.
- **Communication:** use the executor's default.
- **Undecided choices:** for small wording choices the demo doesn't settle (such as link text), decide and note why in the final summary. Don't stop.

>Note: the `problem solving philosophy` field contains the user's rules for handling problems during execution. It has one entry for each category asked about in step 5 (repeated failure, regression, demo can't be reached or looks wrong, scope creep, permissions, communication, undecided choices). If the user had no preference, the entry says `use the executor's default`. The defaults themselves are not defined here. Rules apply to the whole plan unless they start with a `Task` id, in which case they apply only to that `Task`.

