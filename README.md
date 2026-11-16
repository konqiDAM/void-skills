# void-skills

Skills for [OpenCode](https://opencode.ai). One folder per skill, each with a `SKILL.md` the
harness loads when its description matches a request.

Two skills today, built and reviewed together:

| Skill | What it is for |
|---|---|
| [`agentic-pipeline`](agentic-pipeline/SKILL.md) | run a non-trivial feature end to end: spec → review → plan → implement → review → ship |
| [`review-queue`](review-queue/SKILL.md) | drive external reviewer models, one at a time, until each returns a clean verdict |

## Install

```sh
git clone https://github.com/konqiDAM/void-skills.git
cp -r void-skills/agentic-pipeline void-skills/review-queue ~/.config/opencode/skills/
```

On Windows the target is `%USERPROFILE%\.config\opencode\skills\`. A symlink works too — the
structure is the same one OpenCode reads: `<skills>/<name>/SKILL.md`, with any `references/` read by
the skill itself, at the moment it needs them.

## agentic-pipeline

**Triggers on**: "run the pipeline", "full agentic", "spec then plan", "auto review", "full auto",
"autopilot" — or any non-trivial feature or multi-file change. If the pipeline was not named, the
skill asks once before doing anything.

**What it does**: nine phases, each with a defined human gate and a defined delegate.

| Phase | Delegates to | The human answers |
|---|---|---|
| 0. Contract and workspace | this skill, `using-git-worktrees` | worktree? |
| 1. Spec | `brainstorming` | what to build, one question at a time |
| 2. Spec review | `review-queue` | reviewers, then spec ok / change |
| 3. Settings sheet | this skill | where to stop and how to build, one batched call |
| 4. Plan | `writing-plans` | plan ok / change when that stop applies |
| 5. Plan review | `review-queue` | from the sheet |
| 6. Implement | `subagent-driven-development` or `executing-plans` | from the sheet |
| 7. Code review | `requesting-code-review` / `ponytail-review` + `review-queue` | from the sheet |
| 8. Ship | `verification-before-completion`; the finishing checks | go / no-go at the final stop |

**How it is laid out** — deliberately split, so a declined run costs almost nothing:

| File | Holds |
|---|---|
| `SKILL.md` | the "run it or not" question, the six laws, the phase map, Phases 0–2, the rationalization table |
| `references/settings.md` | Phase 3 — the sheet, the stops, the two dials, publishing, the builder choice |
| `references/plan-and-build.md` | Phases 4–7 — plan, plan review, implement, code review |
| `references/ship.md` | Phase 8 and the final report |
| `REVIEW-LOG.md` | every review round of the skill itself, with findings and rulings |

**Defaults are autonomous**: stop mode `autopilot`, all three review points on `continue`, TDD
mandatory. The one stop the defaults keep is the push (`PUBLISH wait for me`) — a push is the one
action a run cannot take back. Every question is asked once, in one place: the spec questions one at
a time during Phase 1, everything else in the single batched settings call after the spec.

## review-queue

**Triggers on**: reviewing a spec, plan, diff or PR with external models — or when
`agentic-pipeline` reaches a review phase.

**What it does**: one artifact, several reviewers, **one at a time**. A reviewer loops on itself
until it says clean — the controller fixes what it found and sends the updated artifact back, same
session where possible — then the next reviewer starts. Three rounds on one reviewer with no clean
verdict stops the queue and asks the human.

**The roster** (all at `max`):

| Name | Model ID |
|---|---|
| Beam Free | `opencode-go/beam-free#max` |
| MiniMax M3.1 Flash Free | `opencode/minimax-m3.1-flash-free#max` |
| GLM 5.3 Flash Free | `opencode/glm-5.3-flash-free#max` |

A model that is missing, capped or gone stops the run and the human picks the replacement — never an
automatic substitute.

**Layout**: `SKILL.md` holds the three laws, the roster, triage, receiving rules, escalation and the
log. `references/briefs.md` holds the exact text sent to a reviewer — the spec, plan and code
briefs plus the resend addendum — and loads only at the first dispatch.

## How the two fit

```
agentic-pipeline
  Phase 2  ──► review-queue (spec)   ──► you confirm the spec
  Phase 3      settings sheet (one call)
  Phase 5  ──► review-queue (plan)
  Phase 7  ──► review-queue (code, per task then the branch)
  Phase 8      ship
```

`review-queue` is usable on its own: point it at a spec, a plan, a diff or a PR.

## Principles both skills follow

- **The human owns the gates.** A gate is an answer, not an assumption — "sounds good" is not
  approval of a document nobody read.
- **Evidence before assertions.** No step is done without the command and its output.
- **One reviewer at a time.** A parallel verdict cannot be attributed to the fix that answered it.
- **Claims about other skills quote their text**, not their gist. Every mechanism error found in
  review was a rule written from memory about how another skill behaves.
- **Questions have shapes**: setup questions batch into one call; substance questions go one at a
  time. A text block expecting a typed reply is not a question.

## History

Every design decision, review round, finding and ruling for these skills is in
[`agentic-pipeline/REVIEW-LOG.md`](agentic-pipeline/REVIEW-LOG.md). It is long on purpose: it records
what was tried, what broke, and why the current text says what it says.
