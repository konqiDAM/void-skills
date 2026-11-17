# Phases 4–7 — Plan, plan review, implement, code review

## Phase 4 — Plan

`writing-plans`, no other skill. The spec is the authority; a plan that contradicts it is a plan
bug. The execution method was already chosen, so say it was supplied — it then asks only whether
the plan captures what they want. Every task opens with a failing test; tell `writing-plans` that
its RED-GREEN order stands and no deviation is wanted.

`writing-plans` ends by asking the human to confirm the plan. The pipeline decides whether that
question surfaces, and the rule here wins: hold it until Phase 5 has run, then ask once, with the
review in hand. Two separate approvals of the same plan is how the review gets skipped: the question
is resolved by Phase 5's outcome — your approval where a stop lands on it, or where the plan review
says `wait`. The question is asked only in the modes whose stop lands on the plan: `stop at each
step` and `stop after the auto review`. Under `stop before shipping` and `autopilot` no human
approves the plan unless the plan review says `wait` — the plan review still runs either way.

## Phase 5 — Plan review

Read the spec's `Reviewers:` line and its `## Reviewers` override section: each override replaces the
named failed entry, so the resolved active list is the original selection in the same order. State
that resolved list before dispatching. Then take the sheet's plan-review after-green and run
`review-queue` with its plan brief over exactly that list, in roster order. When all are green, follow the second
answer — `continue`, or `wait`, where the green verdict goes to the selector as `Approve` /
`Change`. Present `Approve` / `Change` only where Phase 4 says the stop lands on the plan — plan-review
`wait`, or STOPS `stop at each step` / `stop after the auto review`. Otherwise record the green result
and proceed without asking. `Approve` makes the plan the execution contract. `Change` goes back to Phase 4 with
their note: the plan file is edited and the queue runs again on the new text, the same loop the
spec gets in Phase 2. Under `stop at each step` and `stop after the auto review` this is the stop,
whatever the setting says. Under `stop before shipping` and `autopilot` it is a stop only where
the plan review says `wait`. With `Reviewers: None`, only `stop at each step` and
`stop after the auto review` get a plain plan approval; `stop before shipping` and `autopilot` run on
without one.

## Phase 6 — Implement

The chosen skill, the chosen builder. Do not re-ask what the sheet answered. Both builders say
"do not pause between tasks"; the sheet overrides that — under `stop at each step` the controller
takes the per-task pause the human chose. TDD is mandatory: the plan carries test-first tasks, and
the executor follows that order. The plan also carries task → commit → verification.
`verification-before-completion` applies per task, not only at the end.

Both builders key off a plan file: workspaces, briefs and their own progress notes. Loading
either with no plan is the mistake.

END's named git action is pre-approved: an implementer that stops to ask before the merge or push
the sheet named is stuck, not careful. Every other item on that skill's stop list still stops.

## Phase 7 — Code review

Read the spec's `Reviewers:` line and its `## Reviewers` override section the same way — overrides
replace the named entries, original order stands — and take the sheet's task-review after-green. If
the recorded list is `None`, no external pass runs and there is no verdict to show; the builder's own
reviews still run. The
per-task pass sits between the builder's tasks — the next task
starts only after that reviewer is clean — and the branch pass runs once, over the whole branch, at
the end. `continue` moves to the next task, `wait` puts the green verdict to the selector. The review
row is not the only gate: under `stop at each step` the run pauses after every task and after the
branch verdict whatever the row says. Mechanics, severity mapping, receiving rules and the log are
in `review-queue`; the kit is the sheet's `CODE KIT`.

**both** runs conformance then quality over the same range: `requesting-code-review` is the
plan-and-spec check, `ponytail-review` the quality audit. `review-queue` has the order.

**The builder's own review and the external review have different jobs.** `subagent-driven-development`
requires a task review and `executing-plans` requires a whole-branch review; both run. `executing-plans`
also says it runs no implementer subagent per task, no reviewer per task, and no re-review. The
pipeline's external passes override the reviewer-per-task and re-review sentences; TASK REVIEW and
BRANCH REVIEW below are only the pause switches, not the authority to skip a pass.
the no-implementer sentence stands — that is what `one agent inline` buys, and a per-task implementer
dispatch would change the cost the human chose. Where a review point is configured, the external
`review-queue` pass is the final authority: fix or rule on
Critical/Must fix and Important/Should fix automatically, record each decision, and treat the
external clean verdict as the gate before advancing. Its fixes are applied by the controller,
overriding the builder's "resume the implementer" rule for external findings only — the builder's own
task review still follows that rule. Keep each fix RED→GREEN before resending. Minor/Nice to have findings may be ruled away
with a reason. Choose `None` at Phase 2 when you want only the builder's own reviews.

With `both`, the kit runs conformance then quality per model — `review-queue` has the order. The
recorded list runs per task and again over the whole branch.

---

When the branch review is green, read `references/ship.md`.
