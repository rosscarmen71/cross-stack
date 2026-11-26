# Spec: New README

## Behaviors
- **Given** a visitor opens the repo on GitHub, **when** they read the first 5 lines of the README, **then** they know it is a set of Claude Code skills for planning by questioning the user, and that `/cross-plan` is the main skill.
- **Given** the README, **when** a reader looks for a working skill, **then** each of the 6 working skills has a short, plain-language description and a link to its `SKILL.md`.
- **Given** the README on GitHub, **when** it renders, **then** both diagrams (which skill calls which, and cross-plan's steps 0–6) are drawn as pictures.
- **Given** a reader follows the Install section, **when** they finish, **then** they can type `/cross-plan` in a new Claude Code session.
- **Given** a reader is looking for a file, **when** they read the repo layout tree, **then** each entry has a one-line note saying what it holds.

## Rules
- All 6 working skills are listed: `cross-plan`, `grill-what`, `grill-how`, `grill-non-negotiables`, `grill-what-with-docs`, `grill-how-with-docs`.
- The placeholder folders appear only in one short "Planned" list.
- No term is used without a definition or a link to `GLOSSARY.md`.
- Skill steps are not repeated in the README. It links to each `SKILL.md`.
- Third-party credit names Matt Pocock and pstack (Lauren Tan) and links both `THIRD_PARTY_LICENSE.md` files.
- The README does not mention this repo's own `plan/` folder.
- `plan/` is listed in `.gitignore`.

## Examples
| Input | Expected output |
|-------|-----------------|
| Reader clicks the `/grill-how` link | Opens `skills/grill-how/SKILL.md` |
| Reader clicks "license" under Matt Pocock | Opens `skills/third-party/matt_pocock/THIRD_PARTY_LICENSE.md` |
| `git status` after creating a plan | Nothing under `plan/` is shown as untracked |

## Not shown in the demo
- In the real file, the diagrams are Mermaid code blocks that GitHub renders. The demo draws them with SVG instead.
- The links in the demo are placeholders. In the real README they point to the real files.
- The `.gitignore` change.
