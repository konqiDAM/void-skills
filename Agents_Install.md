# Agent install instructions

You are installing the OpenCode skills from https://github.com/konqiDAM/void-skills. Follow these
steps in order. Do not improvise a different layout.

## Which skills

| The human said | Install these folders |
|---|---|
| nothing, "default", or "agentic" | `agentic-pipeline` + `review-queue` |
| "all" | all four |
| a skill name | that folder, plus `review-queue` if the name is `agentic-pipeline` |

If the request does not say which, install the default — most people want only the pipeline. The
queue goes with it because the pipeline calls it at three review points; without it the pipeline
names a delegate that does not exist. Do not ask again; report what you chose.

## What gets installed

| Skill folder | Purpose | Needs |
|---|---|---|
| `agentic-pipeline` | spec → review → plan → implement → review → ship | superpowers + ponytail |
| `review-queue` | external reviewer models, one at a time | ponytail for its code kit |
| `comfyui-setup` | local ComfyUI model layout, flags, failure modes | nothing |
| `game-asset-pipeline` | sprites, textures, 3D props, video, licensing | nothing |

## Steps

1. Get the repo: `git clone https://github.com/konqiDAM/void-skills.git` into a temporary folder, or
   fetch the raw files if git is unavailable.
2. Find the OpenCode skills directory: `~/.config/opencode/skills` — on Windows,
   `%USERPROFILE%\.config\opencode\skills\`. Create it if missing. Do not install into another
   harness's skills folder; these are OpenCode skills (`SKILL.md` format).
3. Copy the selected skill folders from step 1 into that directory, keeping their structure. A
   symlink works too. `agentic-pipeline` and `review-queue` each carry a `references/` subfolder —
   bring it with them, or the skills will point at files that do not exist.
4. Verify the install:
   - each folder has `SKILL.md` with frontmatter containing `name` and `description`;
   - `agentic-pipeline/references/` has `settings.md`, `plan-and-build.md` and `ship.md`;
   - `review-queue/references/` has `briefs.md` and `models.md`.
5. **If a skill with the same name is already installed, show a diff and ask before replacing it.**
   It may have been edited locally, and overwriting loses that work.
6. Check the dependencies **only for the skills you selected**:
   - **superpowers** — repo `https://github.com/obra/superpowers`. Its own OpenCode instruction is:
     `Fetch and follow instructions from https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.opencode/INSTALL.md`.
     `agentic-pipeline` delegates to `brainstorming`, `writing-plans`, `executing-plans`,
     `subagent-driven-development`, `requesting-code-review`, `using-git-worktrees`,
     `finishing-a-development-branch`, `verification-before-completion` and
     `test-driven-development`. Without them the pipeline names skills that do not exist.
   - **ponytail** — repo `https://github.com/DietrichGebert/ponytail`; its OpenCode steps are in
     `INSTALL.md` there. It provides `ponytail-review`, one of the two code-review skills (the other
     is superpowers' `requesting-code-review`); the repo's own instruction for other agents is to
     install `skills/ponytail-review/SKILL.md` as a skill. Without it, the `CODE KIT` choices
     `ponytail` and `both` cannot run. Needed by `agentic-pipeline` and by `review-queue` when used
     standalone on code. Install from that repo or `@dietrichgebert/ponytail` on npm only — the
     author warns that copies shipping `.exe` or `.dll` files are not his.
   - The two AI skills assume Windows, ComfyUI at `D:\AI\ComfyUI`, models at `D:\AI\models\`, a 24 GB
     GPU and 28 GB RAM. On another machine, edit those paths in the copied files only. They have no
     other dependencies.
7. Do not push anything, and do not change the skill text beyond the path fixes in step 6. This repo
   is a mirror of the installed copies: a change in one does not reach the other.

When done, report which skills were installed, which dependencies were already present, any files you
skipped, and any path edits you made.
