# Sub-plan: Rewrite the README

**plan-id:** 1  ·  **task-id:** 1.1  ·  **Date:** 2026-11-25  ·  **Status:** approved

**dependencies**: none (can run in parallel with 1.2)


## Goal
Replace the 3-line `README.md` with the README shown in the approved demo, so a visitor quickly understands what the repo contains and how to find and use its skills.

## Why
- **Why the user wants this:** the README should be easy to understand and give an overall description of what is in the repo, so things are easy to find.
- **Why this result:** not given.
- **Why this approach:** not given.

## Out of scope
- Any file other than `README.md`. Do not edit skills, templates, `GLOSSARY.md`, `.gitignore` (that's Task 1.2) or license files.
- Repeating the step-by-step contents of any `SKILL.md` or of `GLOSSARY.md`.
- Mentioning this repo's own `plan/` folder.

## Permissions
- Inherited from plan 1. File scope: `README.md` only.



## Final State
`README.md` matches `task-1-demos/01-readme.html` section for section, in the same order, using the same wording, with Mermaid diagrams and real relative links.

### Final Verification
- `plan/plan-1/task-1-demos/01-readme.html`: approved mock-up. The README must match its sections, order and wording.
- `plan/plan-1/task-1-demos/01-readme.spec.md`: behaviors, rules and examples the README must meet.
- The first 5 lines say what the repo is. *Verify by:* reading lines 1–5.
- All 6 working skills are listed with short, plain-language descriptions, and every relative link resolves to an existing file. *Verify by:* checking that each link path exists.
- One short "Planned" list names the placeholder folders.
- Both Mermaid diagrams (`flowchart TD` for calls, `flowchart LR` for steps 0–6) render on GitHub with no line overlapping text. *Verify by:* GitHub view or a Mermaid preview.
- The install commands, including `mkdir -p ~/.claude/skills`, run without errors with `HOME` set to an empty temp folder, and `domain-modeling` is linked too.
- The third-party section credits Matt Pocock and pstack (Lauren Tan) and links both `THIRD_PARTY_LICENSE.md` files.
- Glossary terms link to `GLOSSARY.md` on their first use.
- No local paths, emails or other personal details. *Verify by:* `grep -nE "/Users/|@" README.md`.
- Every count, name and path matches the repo (6 working skills, 12 `principle-*` placeholders, 24 pstack skills, folder names). *Verify by:* comparing with `ls skills/` and `ls skills/third-party/*`.
- The only `plan/` in the README is the example output tree.



## Current Delta
- `README.md` is 3 lines: the title and one sentence. Every section of the demo is missing.



## How 
- Copy the demo's wording word for word. Only change the diagrams (SVG → Mermaid) and the placeholder links (→ relative paths).
- Mermaid with no custom colors, except a dotted border on the `domain-modeling` node.
- All links are relative paths.
- Each glossary term links to `GLOSSARY.md` on its first use only.

### How out of scope
- Adding sections, images or badges that aren't in the demo.



## Other Specs
- `GLOSSARY.md` terms to link on first use: Task, Sub-plan, Demo, Non-negotiable (and any other glossary term used).
