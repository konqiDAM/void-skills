# Phases 4–7 — Plan, plan review, implement, code review

## Phase 4 — Plan

`writing-plans`, no other skill. The spec is the authority; a plan that contradicts it is a plan
bug. The execution method was already chosen, so say it was supplied — it then asks only whether
the plan captures what they want. Every task opens with a failing test; tell `writing-plans` that
its RED-GREEN order stands and no deviation is wanted.

`writing-plans` ends by asking the human to confirm the plan. MODE decides what happens:

- **assisted** — hold the question until Phase 5 has run, then ask once with the review in hand.
  Two approvals of the same plan are how the review gets skipped, so ask only here and only where the
  gate lands on the plan: the plan approval and the pause after plan review before any code.
- **autopilot** — the orchestrator resolves that question from the spec's intent and records a
  `Ruling: <decision> — <why> — <cost if wrong>` in the plan file. No human plan approval.

A plan whose every path would break the approved spec is a hard stop in both modes, not a ruling.

## Phase 5 — Plan review

Read the spec's `Reviewers:` line and its `## Reviewers` override section: each override replaces the
named failed entry, so the resolved active list is the original selection in the same order. State
that resolved list before dispatching. Run `review-queue` with its plan brief over exactly that list,
in roster order. With `Reviewers: None`, no external queue runs; assisted still gets the plan gate
described in Phase 4, autopilot runs on.

When the queue is green:

- **assisted** — present `Approve` / `Change` at the plan gate. `Approve` makes the plan the execution
  contract. `Change` goes back to Phase 4 with their note: the plan file is edited and the queue runs
  again on the new text, the same loop the spec gets in Phase 2.
- **autopilot** — record the green result and proceed. A `Change`-worthy problem is resolved by a
  `Ruling:` from the spec's intent, carried in the resend addendum; an approved-spec conflict is a
  hard stop instead.

At the autopilot 10-round cap, Minor findings left open go to `## Known issues` in the final report
and the queue advances; Critical and Important are still fixed or stopped.

## Phase 6 — Implement

The chosen skill, the chosen builder. Do not re-ask what the sheet answered. Both builders say
"do not pause between tasks"; in **assisted** the controller takes the per-task pause the human
chose. In autopilot there is no pause. TDD is mandatory: the plan carries test-first tasks, and the
executor follows that order. The plan also carries task → commit → verification.
`verification-before-completion` applies per task, not only at the end.

**CODER MODEL** comes from the sheet. When it names a model, pass that model on every implementer
subagent dispatch (`model:` on the `subagent` call), overriding the builder's per-task Model
Selection section for those dispatches. When it says `builder chooses per task`, leave the builder's
selection in force. With the inline builder (`executing-plans`) there is no subagent to receive the
model, so the row is unused. If a named coder model dies in autopilot, substitute the highest-ranked
unused roster row and note it in the report; in assisted, stop and ask.

Both builders key off a plan file: workspaces, briefs and their own progress notes. Loading
either with no plan is the mistake.

END's named git action is pre-approved: an implementer that stops to ask before the merge or push
the sheet named is stuck, not careful. Every other item on that skill's stop list still stops.

## Phase 7 — Code review

Read the spec's `Reviewers:` line and its `## Reviewers` override section the same way — overrides
replace the named entries, original order stands. With `Reviewers: None`, no external pass runs and
there is no verdict to show; the builder's own reviews still run.

The per-task pass sits between the builder's tasks — the next task starts only after that reviewer
is clean, or after its 10-round cap is recorded — and the branch pass runs once, over the whole
branch, at the end. In **assisted**, the run also pauses after each task and after the branch verdict;
"stop" ends the run with the ship report and leaves the worktree in place. In **autopilot**, there is
no pause. Mechanics, severity mapping, receiving rules and the log are in `review-queue`; the kit is
the sheet's `CODE KIT`.

**both** runs conformance then quality over the same range: `requesting-code-review` is the
plan-and-spec check, `ponytail-review` the quality audit. `review-queue` has the order.

**The builder's own review and the external review have different jobs.** `subagent-driven-development`
requires a task review and `executing-plans` requires a whole-branch review; both run. `executing-plans`
also says it runs no implementer subagent per task, no reviewer per task, and no re-review. The
pipeline's external passes override the reviewer-per-task and re-review sentences; MODE is the only
pause switch, not the authority to skip a pass. The no-implementer sentence stands — that is what
`one agent inline` buys, and a per-task implementer dispatch would change the cost the human chose.
Where a review point is configured, the external `review-queue` pass is the final authority: fix or
rule on Critical/Must fix and Important/Should fix automatically, record each decision, and treat the
external clean verdict as the gate before advancing. Its fixes are applied by the controller,
overriding the builder's "resume the implementer" rule for external findings only — the builder's own
task review still follows that rule. Keep each fix RED→GREEN before resending. Minor/Nice to have
findings may be ruled away with a reason. Choose `None` at Phase 2 when you want only the builder's
own reviews.

The resolved list runs per task and again over the whole branch.

---

When the branch review is green, read `references/ship.md`.
