# Phase 3 — Settings sheet

Decided once, after the spec and before the plan: the spec is the last thing that changes
cheaply. Ask the mode and the build choices in **one** selector call — autopilot has no later moment
to ask.

| Setting | Options | Default |
|---|---|---|
| MODE | assisted / autopilot | autopilot |
| BUILDER | one agent inline / one fresh agent per task | per task |
| CODER MODEL | roster names, or `builder chooses per task` | builder chooses per task |
| CODE KIT | requesting-code-review / ponytail-review / both | requesting |
| END | merge to main / PR / leave the branch | the git flow the contract names; with none named, no default — this row settles it |
| PUBLISH | push and tag / push only / wait for me | wait for me |
| VERSION | the version to tag, e.g. `v1.2.4` | next patch from `git tag --list 'v*'` |

The reviewers were chosen once in Phase 2 and recorded in the spec; this sheet reads that list and
never changes it (the roster is `../review-queue/references/models.md`). `CODE KIT` picks the review
skill for the code points. `CODER MODEL` lists the same roster names — coding uses the models you
already trust to read — with `builder chooses per task` as the default, because the builder scales
the model to task difficulty.

**MODE decides every gate. There are no per-review dials.**

- `assisted` — the human is asked at every gate: plan approval, the pause after plan review before
  any code, after each task, after the branch verdict, and before git. Each per-task pause shows
  that task's green summary and asks continue or stop; "stop" ends the run with the ship report and
  leaves the worktree in place.
- `autopilot` — no routine gate. The run goes to the end. The orchestrator resolves routine
  questions from the spec's intent and records `Ruling: <decision> — <why> — <cost if wrong>`.

The spec confirmation still happens in both modes: it is Phase 2, before the mode exists. `PUBLISH
wait for me` is the single pause before the git work, in either mode — there is no second
pre-git switch.

These are **setup, so they batch**: one selector call, one entry per row above. VERSION is always
in the call — it is only used when PUBLISH is `push and tag`, and its default is the repo's tag
convention, which the answer may override. Each row's Default is the recommendation, offered first.
VERSION is the one entry that may take a typed value: it offers the computed next patch plus
`Enter a version`. The rest of the table is what the agent shows before it asks, not a form the human
types into.

If the recorded reviewer list is `None`, `CODE KIT` is moot — skip it; the sheet asks MODE, BUILDER,
CODER MODEL, END, PUBLISH and VERSION.

END's default is the git flow the contract describes, not an opinion about this repo. The plan is
always checked, because every later mistake inherits it. The defaults stop you for one thing: the
push (`PUBLISH wait for me`). That gate is deliberate — a push is the one action the run cannot
take back. Choose `push only` if a default run should leave the machine on its own.

TDD is mandatory for every implementation task and is not a settings-sheet choice. Write the
failing test, watch it fail, implement the minimum code, and watch the test pass before every
task is complete.

## Autopilot

Autopilot resolves routine questions itself and never asks "should another round run?". Each reviewer
gets **10 rounds per artifact**. At the cap, findings still open are handled by severity:

- **Minor / Nice to have** — recorded under `## Known issues` in the final report as ruled leftovers,
  and the queue advances to the next reviewer.
- **Critical / Must fix and Important / Should fix** — keep the normal path: fix them, or stop. They
  are never downgraded to a note because a round count ran out.

A reviewer that disputes a ruling, a roster with no unused row left, a model that cannot be reached,
red CI, an approved-spec conflict, a destructive or security-sensitive action, and a finding only the
human can answer are **hard stops**. Everything else is a `Ruling:` recorded in the spec (Phase 1/2)
or the plan file (Phase 4 onward), carried in the resend addendum, and reported at the end.

Autopilot still obeys PUBLISH: it never pushes or tags unless PUBLISH says so, and `wait for me`
remains the one human pause before git. It never merges, pushes or opens a PR the sheet did not
name. It also pre-settles the delegated skills' gates: END is prior approval for exactly the one git
action it names.

### BUILDER

| BUILDER | Cost | Choose when |
|---|---|---|
| `executing-plans` | one context, cheapest | short, fully specified plan; later tasks get a thinner context |
| `subagent-driven-development` | a context per task and per review | long plan, tasks in different subsystems, a review on every task |

Choose by plan length and blast radius, not taste.

---

When the sheet closes, read `references/plan-and-build.md`.
