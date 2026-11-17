---
name: review-queue
description: Use when reviewing a spec, plan, diff or PR with external models, or when agentic-pipeline reaches a review phase. Runs reviewers one at a time until each returns a clean verdict, triages their findings with evidence, and escalates instead of grinding.
---

# Review Queue

One artifact, several reviewers, one at a time. The reviewer finds; you decide; the human rules
when the fix is a judgement call.

## Before the first dispatch

The reviewers and the log are settled for every artifact; a code artifact settles the kit too. Who
settles them depends on how you got here:

- **Inside `agentic-pipeline`:** its settings supplied the reviewers — the spec ones in Phase 2,
  the rest in the sheet. Do not ask again, and do not ask where the log goes — the locations are
  fixed below by artifact type.
- **Standalone:** ask the human, through the selector. `one` means the first in roster order unless
  they name one. For a code artifact ask the kit as well — `requesting-code-review`, `ponytail-review`
  or `both`, default `both` — because the clean verdict depends on it. The log goes in the artifact
  itself, except a PR, where it goes to a file next to it, and a standalone diff, where it goes to a
  file beside the diff. External reviewers never edit files; the controller appends the log.

## The law

1. **One reviewer at a time, in queue order.** Never two in the same turn. Never advance because
   the next model is idle or already warm: a parallel verdict cannot be attributed to the fix
   that answered it.
2. **Only a clean verdict advances.** Every other verdict, MINOR included, keeps that reviewer at
   the head: fix what it found, then send the updated artifact back to the **same** reviewer, in
   the same session where the run allows it. It answers two things — are the old findings fixed,
   is there anything new — and the loop continues on that reviewer until it says OK. Then the next
   reviewer starts. A reviewer is **green** when its verdict is clean; the queue is green when
   every reviewer in it is. What clean is for each brief is stated below.

   The controller triages each finding by severity, not by asking: Critical/Must fix and
   Important/Should fix are fixed automatically, Minor/Nice to have is fixed or ruled away with a
   recorded reason. The conditions that need a human are in **Triage** below.
3. **A model that is missing, gone or rates out stops the run.** Retry once. If there is still no
   answer, or its one trial call fails, stop the queue and ask the human which model to use
   instead. Never substitute one on your own. It does not count as clean.

## Who reviews

**The roster lives in `references/models.md`** — the models, their IDs, and their order of preference.
Read it before the first dispatch. The order is the preference: one reviewer means the first row, a
pair the first two, all of them the whole list. Naming a model skips the order.

Resolve every ID with the models tool before dispatching — the catalogue it returns is a starting
point, not the roster. Free models rotate, do not share a provider, and get capped.

**A model missing from the catalogue can still be usable.** Try it once before stopping. Only a
failed call, or a model that dies mid-run, counts as gone — then the run stops and the human
picks the replacement (law 3).

One `subagent` call per reviewer, `agent: "general"`, passing the model **on the call** as
`provider/model` (or `provider/model#variant`). The `description` is only a label. Without that
argument the subagent inherits the session's model and the roster collapses into one voice
reviewing itself.

## Briefs

Read `references/briefs.md` before the first dispatch of a review. It holds the exact text of the
three briefs — spec, plan, code — and the resend addendum, plus the dispatch rules that apply to
all of them.

What stays here is how to read the answer back:

| Artifact | Clean is |
|---|---|
| spec | `VERDICT: CLEAN` with an empty `FINDINGS` list |
| plan | `VERDICT: CLEAN` with an empty `FINDINGS` list |
| code, `requesting-code-review` | `Ready to merge` + `Yes` with an empty `Issues` list and every Declined-to-judge line ruled on |
| code, `ponytail-review` | `Looks good. Ship.` |
| code, both | both of the above |

`requesting-code-review`'s template ends `**Ready to merge?** [Yes | No | With fixes]`. That line
is the verdict: "With fixes" is not clean, however minor the fixes are. The template's instruction
line writes `Ready to merge?` and its example writes `Ready to merge:` — read the word and the
answer, not the punctuation. Both spellings count.

**A verdict over an open finding is not clean.** Law 2 governs the tokens above: a reviewer is green
only once every finding it raised is fixed or ruled away and it says so with nothing left open.
`Verdict: Ship.` over a Nice-to-have is not clean; `Looks good. Ship.` is.

## Resend

Round 2 is the same reviewer again: pass the child `sessionID` back to `subagent` so it keeps its
own context, and hand it the updated artifact plus the addendum.

**Every resend carries the addendum**, same session or fresh: that reviewer's own previous
findings — never another reviewer's, and nothing else from this session — with a `Ruling:` line on
any finding the controller ruled away. Without the ruling the reviewer re-raises an unchanged
finding every round and the queue escalates on a decision that was already made. The exact
addendum is in `references/briefs.md`.

## Triage

Decide each finding out loud: **fix**, **reject**, or **ask**. Severity sets the default: Critical
and Important are fixed, Minor is fixed or ruled away with a reason. Ask the human when it

- changes scope or contradicts intent they stated,
- needs domain knowledge nobody in the room has,
- contradicts a fix an earlier reviewer already forced,
- would rewrite a section they already approved, including an approved spec,
- or crosses an irreversible boundary.

Everything else you decide, and a rejection needs a visible `Ruling: <decision> — <why> — <cost
if wrong>`. Silently dropping a finding is the failure this skill exists to prevent.

Confidence is not agreement — later reviewers read text earlier ones already fixed:

| Signal | Meaning |
|---|---|
| the evidence quote is a real line of the artifact | a finding, not an impression |
| the next reviewer re-raises it on the fixed text | the fix did not settle it |
| it is still raised after two rounds of fixes | it was not taste |
| it is only a QUESTION | the human answers it — one at a time, and never mixed with another reviewer's questions |

Severity is one idea in two vocabularies:

| `requesting-code-review` | `ponytail-review` | Blocks the ship? |
|---|---|---|
| Critical | Must fix | yes |
| Important | Should fix | yes |
| Minor | Nice to have | no — log or report |

Non-blocking still blocks the queue: the reviewer returns clean only once you have fixed that
finding or ruled it away with a reason. "Blocks the ship" is about what happens after.

External reviewers are read-only. The controller applies fixes and appends the review log after
each reviewer round; the reviewer never edits the artifact.

## Receiving

The reviewer's job ends when it reports. The agent acting on the findings uses
`receiving-code-review`, not the reviewer:

- Verify before acting: re-read the line, reproduce the case, run the test. A finding nobody can
  reproduce is a `Ruling:`, not a fix.
- No performative agreement, no silent compliance. Cite what you checked.
- Push back with technical reasoning, and say what proves it.
- Critical and Important before the next task; Minor is fixed or ruled away with a reason, like
  the rest — the queue law governs, not the severity table.

## Stop and escalate

Stop and report when any of these holds:

- three rounds with the same reviewer and still no clean verdict,
- that reviewer raises in a later round what it could have raised in round 1 — the target is
  moving instead of converging,
- a later reviewer demands the opposite of a fix an earlier one forced,
- a fix would push past the artifact's scope.

Report rounds per reviewer, each verdict, every rejected finding with its ruling, and what is
still open. Then the human rules. A fourth round happens only if they ask.

## The log

Append to the artifact under `## Review log`: round, reviewer, verdict, findings, outcome. The
controller appends the log; external reviewers remain read-only.

- Spec log in the spec, plan log in the plan.
- Code log in the plan file; with no plan, a file beside the diff.
- Never a PR description: a pipeline can finish with no PR at all.

Write the log once the queue is green, not between rounds: a dispatched artifact must not carry an
earlier reviewer's findings, or the next reviewer reads a list to agree with instead of a document
to judge. If the queue stops on an escalation, the report carries the rounds.
