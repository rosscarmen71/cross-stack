# Demo Playbook: Video

## When To Use
* The final behavior is a flow over time: a sequence of interactions, an animation, or a multi-step process the user should watch end to end.
* Watching it is clearer than clicking through it, or the flow is long enough that a recording saves the user time.
* A video **requires an interactive prototype first**. The video is a recording of that prototype, because the real code does not exist yet.

## Output
- **Files:** a `.webm` or `.gif` recording, plus its spec file. Fallback: a folder of numbered, captioned screenshots.
- **Naming:** `<nn>-<slug>.webm` (or `.gif`) and `<nn>-<slug>.spec.md`. Fallback: `<nn>-<slug>-frames/01-<caption-slug>.png`, `02-…`.
- **Location:** `plan/plan-<id>/demos/` for global demos, `plan/plan-<id>/task-<n>-demos/` for a `Task`'s demos

## Creation Steps
1. Create the interactive prototype by following `./playbooks-demos/interactive-prototype.md`, if one does not already exist for this demo.
2. Write the script of the flow: each step the viewer should see, in order, with a one-line caption.
3. If Playwright is available, write a short Playwright script that opens the prototype, performs each step of the flow with brief pauses, and records it to `<nn>-<slug>.webm`. Convert it to `.gif` only if the user prefers it.
4. If Playwright is not available, take one screenshot per step of the flow instead, and save them in `<nn>-<slug>-frames/` numbered in order, with each file name ending in its caption.
5. Watch the recording or check the frames yourself to confirm the flow is complete and correct.
6. Write the spec file, following the `Spec File` section below.

## Show User Steps
1. Open the recording for the user with `open <path>`. For the fallback, open the frames folder.
2. Give the user the step-by-step captions from creation step 2 so they can follow along.
3. Give the user the path to the spec file and summarize what it adds beyond the video.

## Spec File
Covers everything the video cannot show: timing and performance requirements, variations of the flow not recorded, error cases, and any behavior that was faked in the prototype. Follows the structure in `./playbooks-demos/spec.md`.
