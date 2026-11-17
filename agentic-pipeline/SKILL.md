---
name: agentic-pipeline
description: Use when developing a non-trivial feature or multi-file change end to end, or when the user says "full agentic", "run the pipeline", "spec then plan", "auto review", "full auto", "autopilot". Runs a spec you confirm, external reviewers one at a time, plan, plan review, TDD implementation, code review, verification and ship, stopping where you choose or on autopilot with one report at the end. Asks every decision through the question selector, batches setup, asks the spec questions one at a time, and confirms the spec with you after the review.
---

# Agentic Pipeline

The human owns the gates; the agent owns everything between them. A gate nobody reads is not
a gate, so every gate asks a direct question and waits.

## Step 0 — Was this asked for?

If the request named this pipeline — "full agentic", "run the pipeline", "spec then plan",
"auto review", "full auto", "autopilot" — say what you are starting in one line and go to Phase 0.
Do not ask
again. A request that merely *sounds* like a spec-then-plan run has not named it: that one gets the
question below.

**The pipeline runs only when the human names it or picks `Run the pipeline`.** If the request did
not name it, you loaded this skill because its description matched. Ask once, through the selector:

- **Run the pipeline** — spec, review, plan, review, implement, review, ship (recommended)
- **Not now** — answer the request directly, with no phases

Only `Run the pipeline` enters Phase 0. `Not now` means this skill is set aside for the task, and
the answer is the task itself.

## Laws

1. **A gate is the human's answer, not yours.** "Sounds good" about the idea is not approval
   of a document they have not seen.
2. **No doubt is left behind.** An unresolved question is asked, or written down with its cost.
3. **Doubt goes to the human in review; you rule during implementation.** Review is cheap to
   pause on, execution is not.
4. **Evidence before assertions.** No task, gate or ship step is done without the command and
   its output.
5. **One artifact, one reviewer at a time, until clean.** Never advance, never parallel.
6. **Every question goes through the selector, in the right shape.** Setup questions — the sheet,
   reviewer choices, worktree consent — are asked **once each**, never dripped out: the sheet is one
   call with one entry per decision, the reviewer choice is one multi-select over the roster's names,
   and worktree consent is its own single question. Git flow is not asked separately; it is the
   sheet's END entry.
   Substance questions — a doubt about the spec, the plan, or what a requirement means — are asked
   **one at a time**: ask, wait, play it back, then the next. A text block expecting a typed reply
   is not a question, and a decision taken without one is a decision you took.

| Phase | Delegates to | The human answers |
|---|---|---|
| 0. Contract and workspace | this file, `using-git-worktrees` | worktree? |
| 1. Spec | `brainstorming` | what to build (questions, one at a time) |
| 2. Spec review | `review-queue` | reviewers; then spec ok / change |
| 3. Settings sheet | `references/settings.md` | where to stop + how to build |
| 4. Plan | `writing-plans` | plan ok / change when that stop applies |
| 5. Plan review | `review-queue` | (from the sheet) |
| 6. Implement | `subagent-driven-development` or `executing-plans` | (from the sheet) |
| 7. Code review | `requesting-code-review` / `ponytail-review` + `review-queue` | (from the sheet) |
| 8. Ship | `verification-before-completion`; `finishing-a-development-branch`'s checks | go / no-go (at the final stop) |

Phases 4–8 assume the run stops somewhere; with `autopilot`, Phase 3's rules govern. Phases 6 and 7
interleave: each task is implemented, its review runs, and only then does the next task start.

**This file holds Phases 0–2. Each later phase is its own file — read it when the phase starts,
never from memory:**

- `references/settings.md` — Phase 3: the settings sheet, the stops, the two dials, publishing and
  the builder choice.
- `references/plan-and-build.md` — Phases 4–7: the plan, its review, the build and its review.
- `references/ship.md` — Phase 8: the ship steps and the final report.

## Phase 0 — Contract and workspace

Read `AGENTS.md` first where present, then the nearest applicable `CLAUDE.md` or
`CONTRIBUTING.md` for additional rules. If those sources conflict, stop and identify the conflict
before choosing which one governs. Take build and test commands, the CI workflows, the git flow,
and which docs must ship from them, not from memory. If the contract is silent on git flow, say so
in the spec's constraints — the sheet's END row is where that decision is made, once.

Then set up the workspace, before writing anything:

- **Check isolation first.** Run `using-git-worktrees`' Step 0 detection: if the session is already
  in a linked worktree (and not a submodule), work there, create nothing new, and skip the consent
  question — there is nothing to create.
- **Ask consent for the worktree**, through the selector — only when no preference has been declared
  and the session is not already isolated; that is when `using-git-worktrees` requires it. If
  they consent, create it and move the session into it — `worktree_create` returns a directory, and
  `session_move` with no sessionID moves the current session. If they decline, create a branch in
  the primary checkout and run setup and the baseline there — never on `main`, never on whatever
  branch the checkout happened to hold. This overrides that skill's work-in-place path: declining
  isolation is not declining the branch.
- **Run the skill's project setup** (install dependencies, restore vendor). A fresh worktree has
  none, so skipping this makes the baseline fail of missing modules instead of failing tests.
- **Run the baseline suite.** Report it and continue — mention it at the next stop, or in the final
  report on autopilot. This overrides the consent skill's "ask whether to proceed": a suite that was
  already red is not a decision about this work.

When the work is in a worktree, a file written in the primary checkout never reaches the branch.

## Phase 1 — Define the spec

`brainstorming`, with three rules on top. This named-pipeline sequence replaces
`brainstorming`'s builder check, its pre-review approval gate, and its "ask only when the answer
would change what gets built" filter.

**Questions until the spec is right.** The spec is defined in conversation. Ask whatever it needs
— what it is for, what it must not do, what "done" looks like, what would make it a failure —
one at a time, in `brainstorming`'s rhythm. There is no filter on what may be asked: a doubt is
just a question you could not answer yourself.

1. When a question comes up, write it in the chat in two lines — what it changes, and why you
   cannot infer it — then put it to the selector as a **single** question, recommendation first.
2. Wait for the answer, play back in a sentence what you now understand, then take the next
   question. Batching these is not a shortcut: it produces answers written for questions you
   would no longer ask once the earlier ones landed.
3. If they say "list them all first", list them numbered in the chat, then still take them one at a
   time. Only setup questions batch (law 6).

Record each answer in the spec, or as `Ruling: <decision> — <why> — <cost if wrong>` when you had
to decide. A question that survives two rounds unanswered goes into the spec as
`Open question (blocking): ...`, and a blocking question cannot survive approval — the human
answers it at the gate.

**Offer the neighbours.** The user asked for one thing; name the three most likely adjacent
ones, one line each with cost and default:

```
1. Cache the parsed config between polls — ~20 lines, removes a visible stall  [default: yes]
2. Move the editor to its own window — half a day, removes a class of crash    [default: no]
3. Export the settings as JSON — ~1 h, nobody asked                          [default: no]
"none"
```

Ask it through the selector — one entry per neighbour plus `none`, the same shape as every other
question — not as a typed reply.

Everything declined goes into **Non-goals** with the reason. That list is the cheapest scope
control in the pipeline.

**The doc is yours, the words are not.** Observable behaviour, not tasks. Interfaces named.
Acceptance criteria specific enough to become assertions. The approaches considered and why this
one. Non-goals. Constraints. Path:
`docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`, committed with the code.

The spec file is written at the end of this phase. It is not approved yet: the human confirms it
after the external review, in Phase 2. The approved spec is frozen. Implementation must not change
it; if implementation proves the approved behavior wrong or incomplete, stop and present the
conflict. Any change requires a new explicit change request and a new review/approval cycle. The
`## Reviewers` section is the one exception: a human-named replacement under `stop` is a gate ruling,
not a change to the work, and may be written there without a new cycle.

## Phase 2 — Spec review

The spec file exists. Ask which reviewers the whole run uses — once, here, never again:

- **Which reviewers** — one multi-select through the selector (`multiple: true`), with every name
  from `../review-queue/references/models.md` as an option in roster order, the first row preselected,
  plus a `None` option. `None` must be the only tick — deselect the preselected row first. A submit
  with nothing ticked, with `None` alongside another name, or a dismissed picker, is an error: ask
  the question again.

This one selection covers the spec review, the plan review, every task review and the branch pass.
Record it in the spec's constraints as `Reviewers: <names in roster order>` before approval. Later
phases read that line, never the sheet. Every substitute — autopilot or human-named — goes in one
canonical place: a `## Reviewers` section under the spec's Constraints, exempt from the freeze as a
gate ruling. One line per substitution — `Reviewers override: <failed name> → <replacement>` —
replaces that entry and keeps the original order. The plan file may mirror the note, but the spec
section is authoritative. The final report names every substitution too. "Already used" means
anywhere in the run, not just this queue, so a dead model is never retried at a later artifact.

`None` means no external queue runs: note it, go straight to the human confirmation below, and
carry `None` forward to the later review points.

That is the only question here: the spec has no after-green switch, because the human answers
after the reviewers are green in every mode. Green is `review-queue`'s word for it: every reviewer
in that list returned its own clean verdict. The stop mode is not chosen until the sheet, so this
first review runs with the `stop` failure policy — a failed model here asks you, never substitutes.
`substitute` applies from the plan review onward, if autopilot was chosen.

Then `review-queue` runs exactly that list with its spec brief. This overrides `brainstorming`'s
hard gate that sends an approved spec straight to the plan; it does **not** override the
one-at-a-time rhythm for the spec questions — see Phase 1.

**Green is not the approval.** When every reviewer is green, summarise in the chat — rounds per
reviewer, findings, rulings — and ask the human, through the selector:

```
Green: 3 reviewers, 6 rounds between them, every verdict clean.
5 findings, 4 accepted, 1 rejected (Ruling in the log).

Is this spec right as it stands, or does something change?
```

`Approved` moves the run to the sheet and freezes the spec. `Change` goes back to Phase 1 with
their note: the spec file is edited and the queue runs again on the new text. The freeze starts
at approval, so this edit is allowed. If the spec still carries an `Open question (blocking)`, the
gate is that question first — one at a time, with its recommendation — and its answer is written
into the spec; the queue then re-runs on the answered text, and approval is offered only when none
is left and the queue is green again. This question is asked in every mode, because the mode does
not exist yet.

**When the spec is approved, read `references/settings.md` — the sheet is the next thing you ask.**

## Rationalization table

| Thought | Reality |
|---|---|
| "The spec is obviously clear, skip the questions" | The questions you did not ask come back as rework. |
| "I'll mention the nearby features later" | Later never comes. The menu now, Non-goals after. |
| "Running all three at once saves time" | It costs attribution: you cannot tell which fix answered which finding. |
| "This model only raised MINORs, fold it into the next" | MINOR still blocks that reviewer. One at a time. |
| "Two of three are clean, close enough" | Only a clean verdict advances, and each reviewer keeps the last word until it says OK. |
| "A fourth round will converge" | Three on one reviewer is the escalation trigger. The human rules. |
| "Three clean verdicts mean the spec is good" | On a vague spec they mean it was too vague to attack. Check what they quoted. |
| "Autopilot means the reviews are optional" | It removes the stops, not the queue. |
| "Autopilot is fast, I'll skip the plan" | The plan is where autopilot is cheapest to correct. |
| "CI will catch it, or it'll be green in a minute" | CI confirms, and a queued or red run is not done. The same commands run locally first. |
| "Reordering a phase is a small doc edit" | Every reordering so far has opened two seams. Re-read the whole path it touches. |

