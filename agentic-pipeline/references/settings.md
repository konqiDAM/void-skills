# Phase 3 — Settings sheet

Decided once, after the spec and before the plan: the spec is the last thing that changes
cheaply. Ask where to stop and the rest in **one** selector call — autopilot has no later moment to
ask.

| Setting | Options | Default |
|---|---|---|
| STOPS | stop at each step / stop after the auto review / stop before shipping / autopilot | autopilot |
| BUILDER | one agent inline / one fresh agent per task | per task |
| PLAN REVIEW | reviewers + after-green | 1 Beam, continue |
| TASK REVIEW | reviewers + after-green | 1 Beam, continue |
| BRANCH REVIEW | reviewers + after-green | all three, continue |
| CODE KIT | requesting-code-review / ponytail-review / both | requesting |
| END | merge to main / PR / leave the branch | the git flow the contract names; with none named, no default — this row settles it |
| PUBLISH | push and tag / push only / wait for me | wait for me |
| VERSION | the version to tag, e.g. `v1.2.4` | next patch from `git tag --list 'v*'` |

Every review point from here on asks the same two questions, and every phase uses its own answer:

- **Reviewers** — `none`, one of Beam / MiniMax / GLM, any pair, or all three. How many and which in
  one answer, so the count cannot drift from the names.
- **After they are green** — `continue`, or `wait` for your confirmation.

The spec review's reviewers were already set in Phase 2, so these three are the future ones.
`CODE KIT` picks the review skill for the code points — settled here, because every pipeline
question is settled before the plan. Each later phase prints its setting before it runs (`Plan
review: Beam + GLM, wait`) and never asks again.

These are **setup, so they batch**: one selector call, one entry per row above that applies — VERSION
is included whenever the repo has no tag convention (it is only used when PUBLISH is `push and
tag`), and skipped otherwise. Each row's Default is the recommendation, offered first. The table is
what the agent shows before it asks, not a form the human types into.

END's default is the git flow the contract describes, not an opinion about this repo. The plan
review is never `none` by default — the plan is always checked, because every later mistake
inherits it. The defaults stop you for one thing: the push (`PUBLISH wait for me`). That gate is
deliberate — a push is the one action the run cannot take back. Choose `push only` if a default
run should leave the machine on its own.

TDD is mandatory for every implementation task and is not a settings-sheet choice. Write the
failing test, watch it fail, implement the minimum code, and watch the test pass before every
task is complete.

**Where it stops.** STOPS is one choice, not a list — the four are exclusive:

- `stop at each step` — you are asked after the plan review, after each task, after the branch
  review, and before the merge and the push. Each per-task pause shows that task's green summary
  and asks continue or stop. The spec approval already happened in Phase 2, and
  the settings sheet is itself a question, so neither is a stop.
- `stop after the auto review` — one stop: the run goes through the spec review, the plan and the
  plan review, and stops when the plan review is green, before a line of code is written. Your word
  releases it to implement and ship.
- `stop before shipping` — no stop of its own during the run; one at the end: after the code is
  finished, the tests are green and the reviewers said OK, and before anything is merged or
  pushed. You see what shipped, and your word lets it go.
- `autopilot` — no stop of its own from this sheet on; one report at the end.

**The two dials.** STOPS is where the run pauses on its own. The review rows are the other dial: a
review point set to `wait` is asked wherever it lands, in every mode; `continue` means nobody is
asked there. Where both land on the same moment, the pause happens once and the prompt carries the
review summary. `autopilot` has no pause of its own — the only pauses left are the review points
you marked `wait`, and the safety stops below.

The spec approval happens at the end of Phase 2, before the mode exists, so it happens in every
mode; STOPS governs the run from this sheet onward. The spec review has no after-green switch —
the human always confirms the spec. The sheet's three review rows carry `continue / wait` for the
plan, task and branch reviews.

**Publishing is a separate question.** END decides what integration happens — merge, PR, or leave
the branch. PUBLISH decides whether the remote action happens by itself:

- `push and tag` — push the branch, and once its run is green, create the next version tag from the
  repo's own convention and push it. State the version before creating it. If the repo has no tag
  convention, the version comes from the sheet's VERSION row, which the same call carries whenever
  this option is on the table.
- `push only` — push the branch, with no tag.
- `wait for me` — stop before any push and let them do it.

A tag publishes a release, so it happens only when PUBLISH says `push and tag`. With END=PR, the
branch push and the PR are both remote actions — `wait for me` holds the pair back and leaves it to
the human. No combination is forbidden, so a batched answer can never conflict.

**Autopilot** means one run from this sheet on with no pause of its own, and one report at the
end — what shipped, rounds per reviewer, every ruling, and the things you would have asked. Doubt
becomes a `Ruling:` in the log — the spec while it is being written, the plan file after that. The
only interruptions are the review points you set to `wait`, the `wait for me` push gate, and these
hard stops:

- destructive or irreversible;
- security-sensitive;
- a side effect outside the worktree that END did not name;
- a plan whose every available path risks breaking the spec, scope, or an irreversible boundary;
- a review queue hitting an escalation trigger (`review-queue`);
- a reviewer model that dies or cannot be reached (`review-queue` law 3);
- a finding only the human can answer — a scope change, an approved spec conflict, domain
  knowledge nobody in the run has;
- a CI run that is red after its attempts, or that cannot be watched (Phase 8).

Autopilot still obeys PUBLISH: it never pushes or tags unless PUBLISH says so.

It never merges, pushes or opens a PR the sheet did not name. It also pre-settles the
delegated skills' gates: a review point set to `continue` stands in for the human gate it would
otherwise be, and END is prior approval for exactly the one git action it names. A review point set
to `wait` stops the run wherever it lands, even on autopilot.

### BUILDER

| BUILDER | Cost | Choose when |
|---|---|---|
| `executing-plans` | one context, cheapest | short, fully specified plan; later tasks get a thinner context |
| `subagent-driven-development` | a context per task and per review | long plan, tasks in different subsystems, a review on every task |

Choose by plan length and blast radius, not taste.

---

When the sheet closes, read `references/plan-and-build.md`.
