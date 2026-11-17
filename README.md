# void-skills

Skills for [OpenCode](https://opencode.ai): `agentic-pipeline`, `review-queue`, `comfyui-setup`,
`game-asset-pipeline`. One folder per skill, each with a `SKILL.md`; supporting files load only when
the skill needs them.

## Install

### Agentic pipeline

Copy this into any agent. It installs `agentic-pipeline` and its required `review-queue` — nothing else.

```
Install skills from https://github.com/konqiDAM/void-skills:
fetch https://raw.githubusercontent.com/konqiDAM/void-skills/main/Agents_Install.md and follow it.
I want: agentic-pipeline.
```

<details>
<summary>All skills</summary>

Copy this instead if you want all four skills:

```
Install skills from https://github.com/konqiDAM/void-skills:
fetch https://raw.githubusercontent.com/konqiDAM/void-skills/main/Agents_Install.md and follow it.
I want: all.
```

</details>

<details>
<summary>Manual install</summary>

```sh
git clone https://github.com/konqiDAM/void-skills.git
cp -r void-skills/agentic-pipeline void-skills/review-queue void-skills/comfyui-setup void-skills/game-asset-pipeline ~/.config/opencode/skills/
```

On Windows the target is `%USERPROFILE%\.config\opencode\skills\`. A symlink works too — the structure
is the same one OpenCode reads: `<skills>/<name>/SKILL.md`, with `references/` loaded by the skill when
it needs them. `agentic-pipeline/references/` holds `settings.md`, `plan-and-build.md` and `ship.md`;
`review-queue/references/` holds `briefs.md` and `models.md`.

Dependencies: [superpowers](https://github.com/obra/superpowers) for the pipeline's delegates and
[ponytail](https://github.com/DietrichGebert/ponytail) for `ponytail-review`, one of the two `CODE KIT`
choices. The two AI skills have no code dependencies. [`Agents_Install.md`](Agents_Install.md) has the
exact install commands, the diff-before-overwrite rule and the final report an agent should give.

</details>

## agentic-pipeline

**Triggers on**: "run the pipeline", "full agentic", "spec then plan", "auto review", "full auto",
"autopilot" — or any non-trivial feature or multi-file change. If the pipeline was not named, the
skill asks once before doing anything.

**What it does**: nine phases, each with a defined human gate and a defined delegate.

| Phase | Delegates to | The human answers |
|---|---|---|
| 0. Contract and workspace | this skill, `using-git-worktrees` | worktree? |
| 1. Spec | `brainstorming` | what to build, one question at a time |
| 2. Spec review | `review-queue` | which reviewers, then spec ok / change |
| 3. Settings sheet | this skill | where to stop and how to build, one batched call |
| 4. Plan | `writing-plans` | plan ok / change when that stop applies |
| 5. Plan review | `review-queue` | from the sheet |
| 6. Implement | `subagent-driven-development` or `executing-plans` | from the sheet |
| 7. Code review | `requesting-code-review` / `ponytail-review` + `review-queue` | from the sheet |
| 8. Ship | `verification-before-completion`; the finishing checks | go / no-go at the final stop |

**How it is laid out** — deliberately split, so a declined run costs almost nothing:

| File | Holds |
|---|---|
| `SKILL.md` | the "run it or not" question, the laws, the phase map, Phases 0–2, the rationalization table |
| `references/settings.md` | Phase 3 — the sheet, the stops, the two dials, publishing, the builder choice |
| `references/plan-and-build.md` | Phases 4–7 — plan, plan review, implement, code review |
| `references/ship.md` | Phase 8 and the final report |

Reviewers are picked once at Phase 2 from the ordered roster and reused for every later review.
Defaults are autonomous: stop mode `autopilot`, all three review points on `continue`, TDD mandatory.
The one stop the defaults keep is the push (`PUBLISH wait for me`) — a push is the one action a run
cannot take back. Every question is asked once, in one place: the spec questions one at a time during
Phase 1, everything else in the single batched settings call after the spec.

## review-queue

**Triggers on**: reviewing a spec, plan, diff or PR with external models — or when
`agentic-pipeline` reaches a review phase.

**What it does**: one artifact, several reviewers, **one at a time**. A reviewer loops on itself
until it says clean — the controller fixes what it found and sends the updated artifact back, same
session where possible — then the next reviewer starts. In autopilot it substitutes the highest-ranked
unused row until one answers; in every other mode a dead model stops the run and asks.

**The roster** lives in [`review-queue/references/models.md`](review-queue/references/models.md), in
order of preference — the order *is* the roster: what a one-reviewer run gets, the order the queue runs
your picks, and the order autopilot falls through. Adding, removing or reordering a model is an edit to
that table only; no other file names one.

**Layout**: `SKILL.md` holds the three laws, the roster pointer, triage, receiving rules, escalation
and the log. `references/briefs.md` holds the exact text sent to a reviewer — the spec, plan and code
briefs plus the resend addendum — and loads only at the first dispatch.

## comfyui-setup

**Triggers on**: installing or running ComfyUI, adding or moving models, a workflow that will not
load, "missing model", out-of-memory or black-screen crashes, the video pipeline's launch flags.

**What it does**: the operating manual for one machine. It covers the folder layout and which model
type belongs in which folder; nine critical rules (update both `widgets_values` and
`widgets_values_named`, restart after adding a model, int8/FP8 builds for 28 GB of RAM, the H3
video flags, never mix model types); the install procedure for a new model, including a safetensors
header check and verifying the loader sees it; normal and video launch commands; a model/settings
table; and a common-issues table — `hostbuf_file_reader_read failed`, the H3 `Fault failed: 2`, the
TDR black screen, the "missing model" dialog, a model absent from the dropdown.

**Assumes**: Windows, ComfyUI 0.44.0 portable at `D:\AI\ComfyUI`, models at `D:\AI\models`, 24 GB
VRAM, 28 GB RAM. The paths are hard-coded — on another machine, edit them first.

## game-asset-pipeline

**Triggers on**: making sprites, textures, 3D props, video clips or skyboxes for a game.

**What it does**: maps a task to the model and the exact workflow JSON, then gives the steps per
asset type:

| Asset | Route |
|---|---|
| Sprites | SDXL + Pixel-Art-XL → nearest-neighbour downscale → palette-limit → hand cleanup → seed lock or a character LoRA |
| Textures | tileable albedo → seam fix → PBR extraction → ORM channel-pack → upscale |
| 3D props | concept image → Pixal3D / TRELLIS.2 → remesh and decimate → retopo, UV, LOD in Blender → auto-rig → export |
| Video | MiniMax H3, 5 s at 864×480, 4-step turbo |
| Skyboxes | Z-Image Turbo with the equirectangular LoRA |

Each pipeline states its honest limit — AI pixel art is not a true pixel grid, precise brick
patterns still belong in Substance Designer, characters still need manual rigging — and the skill
carries a licensing table that separates Apache/MIT (ship it), revenue-capped, non-commercial and
research-only models, plus the disclosure rules (Steam, Civitai LoRAs, US copyright).

**Assumes**: the machine above, with `comfyui-setup` already applied. It also names specific
installed models, so the "installed vs available on request" list is a snapshot.

## How the pairs fit

```
agentic-pipeline
  Phase 2  ──► review-queue (spec)   ──► you confirm the spec
  Phase 3      settings sheet (one call)
  Phase 5  ──► review-queue (plan)
  Phase 7  ──► review-queue (code, per task then the branch)
  Phase 8      ship

game-asset-pipeline  ──► comfyui-setup (the machine and its model layout)
```

`review-queue` is usable on its own: point it at a spec, a plan, a diff or a PR.
`game-asset-pipeline` assumes the install `comfyui-setup` describes.

## Requirements

- **OpenCode**. These are OpenCode skills; nothing here is Tauri, Node or a web app.
- **superpowers** ([github.com/obra/superpowers](https://github.com/obra/superpowers)) — `agentic-pipeline`
  delegates to nine of its skills. Without them the pipeline names delegates that do not exist.
- **ponytail** ([github.com/DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)) —
  provides `ponytail-review`, one of the two `CODE KIT` choices. Install commands for both are in
  [`Agents_Install.md`](Agents_Install.md).
- **ComfyUI + a 24 GB GPU** for the two AI skills. No code dependencies.
- Reviewer models: the roster in `review-queue/references/models.md`, plus a working `opencode-go`
  (or `opencode`) provider. If a model is capped, the queue stops and asks — by design.

## Principles the coding skills follow

- **The human owns the gates.** A gate is an answer, not an assumption — "sounds good" is not
  approval of a document nobody read.
- **Evidence before assertions.** No step is done without the command and its output.
- **One reviewer at a time.** A parallel verdict cannot be attributed to the fix that answered it.
- **Claims about other skills quote their text**, not their gist. Every mechanism error found in
  review was a rule written from memory about how another skill behaves.
- **Questions have shapes**: setup questions batch into one call; substance questions go one at a
  time. A text block expecting a typed reply is not a question.

## History

The public history of these skills is the [git history](https://github.com/konqiDAM/void-skills/commits/main).
An older, detailed review log is kept privately by the author.
