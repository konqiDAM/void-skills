# agentic-pipeline — review log

The review history of the skill itself, kept out of `SKILL.md` so the body stays short.
Newest last. Findings are numbered continuously across rounds.

**Historical.** Every entry describes the file as it stood at that moment — phase names,
row labels and wording in an old table are records of that round, not current rules. The
current rules live in `SKILL.md` and `review-queue/SKILL.md`.

## 2026-11-15 — round 1 — Beam Free (max)

VERDICT: MAJOR, 10 findings. Each verified against the source skill before the fix.

| # | Finding | Outcome |
|---|---|---|
| F1 | Phase 8 hardcoded this repo's build commands in a global skill | accepted — commands come from `AGENTS.md` and the workflows |
| F2 | END's `[leave]` default contradicted the prose | accepted — default is the repo contract |
| F3 | "ask in the settings sheet" for git flow, but no slot exists | accepted — asked before the sheet, written to `AGENTS.md` |
| F4 | Phase 7 mixed review skill and model count; severities from one skill only | accepted — count = models; severity map added |
| F5 | Phases 4 and 7 re-asked what the sheet answered | accepted — counts come from the sheet |
| F6 | Agreement table assumed parallel review | accepted — replaced with evidence signals |
| F7 | Batched questions conflict with `brainstorming`'s one-at-a-time | accepted — precedence stated |
| F8 | "Normal" style conflicts with `executing-plans`' TDD mandate | accepted — TDD still loads, order inverted |
| F9 | `writing-plans` would re-ask the execution method | accepted — declared as supplied |
| F10 | A specific repo's git flow stated as fact | accepted — removed with F2 |

Not from the review, found while fixing: the model table assumed one provider for all
three IDs. Beam is `opencode-go/beam-free#max`; the other two free IDs are under `opencode`.

## 2026-11-15 — round 2 — Beam Free (max)

All 10 prior findings ADDRESSED. VERDICT: MAJOR, 5 findings.

| # | Finding | Outcome |
|---|---|---|
| F11 | Phase 2 still offered the spec count again at Phase 4 | accepted — the sheet carries it |
| F12 | Receiving rules used one skill's severities | accepted — blocking / non-blocking |
| F13 | Branch review ran even when FINAL REVIEW is `none` | accepted — exception added |
| F14 | Cleanup omitted END = PR | accepted — PR keeps the worktree too |
| F15 | Small changes: sheet or not, unstated | accepted — sheet runs, panels and plan do not |

Q3 (does a small change get the sheet) ruled rather than asked: it does, because STYLE and
END are decided there and guessing either costs more than one message.

## 2026-11-16 — round 3 — Beam Free (max)

All 5 prior findings ADDRESSED. VERDICT: MAJOR, 5 findings.

| # | Finding | Outcome |
|---|---|---|
| F16 | Sheet's reply example still ended in "merge" | accepted — example no longer supplies END |
| F17 | Rationalization row used spec severities for code review | accepted — blocking / the rest |
| F18 | A blocking open question could pass the gate | accepted — answered at the gate or ruled |
| F19 | Queue advice could override the sheet's count | accepted — advice for filling it in |
| F20 | Auto stop list omitted red CI | accepted — fifth stop named |

Escalation fired here: three rounds on one reviewer with no CLEAN is the skill's own stop
condition, so the queue stopped. Q4 answered by the F18 fix.

## 2026-11-16 — round 4 — Beam Free (max)

**Run on the author's instruction**, overriding the three-round stop. Recorded so the file
does not contradict its own history.

All 5 prior findings ADDRESSED. VERDICT: MAJOR, 2 findings.

| # | Finding | Outcome |
|---|---|---|
| F21 | Gated-mode note covered Phases 3–5 only | accepted — widened to 3–8 |
| F22 | Phase 7 never said where the code-review log lives | accepted — plan file, or the ledger |

Q5 (plan file or PR description) ruled: the plan file. Auto can finish with no PR.

## 2026-11-16 — round 5 — Beam Free (max)

All 2 prior findings ADDRESSED. VERDICT: MAJOR, 2 findings.

| # | Finding | Outcome |
|---|---|---|
| F23 | Description claimed a human gate between each phase, untrue in auto | accepted — "gated per phase or full auto" |
| F24 | Reply example had two unlabelled "all" values | accepted — one value per row, END as a placeholder |

Q6 (keep or drop "human gate" in the description) ruled: keep it, qualified by mode.

## 2026-11-16 — round 6 — Beam Free (max)

All 2 prior findings ADDRESSED. **VERDICT: CLEAN.** No findings, no questions.

One reviewer, six rounds, 24 findings, 24 accepted, 0 rejected. With a single reviewer the
verification pass and the final round are the same read.

## 2026-11-16 — restructure

Not a review. Three changes to make the skill cheaper to load, with no rule dropped:

- The review machinery became its own skill, `review-queue`, so it is read at the first
  dispatch instead of every session, and it can be used for a spec, plan or PR review
  without the pipeline around it.
- This history moved here, out of `SKILL.md`.
- Red flags were deleted (the rationalization table already carried every one of them), the
  quick-reference block was folded into the phase table, the style and builder prose became
  tables, and the repeated statements of the five laws now appear once, at the top.

`SKILL.md`: 701 lines / 34 KB → 222 lines / 11.5 KB. `review-queue`: 179 lines / 6.7 KB,
loaded only when a review runs.

## 2026-11-16 — blind round 1 (fresh session, both files)

A reviewer with no memory of the six rounds above, reviewing the pipeline and the queue as a
pair. VERDICT: MAJOR, 10 findings, all accepted.

| # | Finding | Outcome |
|---|---|---|
| F1 | Phase 0 lost the instruction to *ask* the size, leaving the table as a decision the agent made alone | accepted — the question is back, with "not a decision you make alone". This was a regression from the trim, found because nobody had read the trimmed file |
| F2 | Auto promised no gates while the delegated skills demand consent and plan approval | accepted — the mode choice pre-settles worktree consent, stands in for the plan gate, and END is prior approval for the one git action it names |
| F3 | The resend template demanded "CLEAN", which `ponytail-review` never says | accepted — the clean verdict of whichever skill is in use |
| F4 | The severity table read as "MINOR does not block the queue" | accepted — column renamed "Blocks the ship?", with the queue rule stated under it |
| F5 | Two unlabelled "all" values survived the positional fix | accepted — values labelled by row |
| F6 | Phase 8's CI step told the agent to ask for a conclusion in a mode with no gates | accepted — auto reports unverified and names the run |
| F7 | `brainstorming`'s hard gate sends an approved spec straight to the plan | accepted — the override is stated |
| F8 | Standalone use had no way to learn the count or the log destination | accepted — ask before the first dispatch |
| F9 | "write it back into `AGENTS.md`" assumed that file exists | accepted — the contract file read, creating `AGENTS.md` only if none |
| F10 | No dispatch brief existed for `ponytail-review`, which is a procedure, not a template | accepted — load the skill and apply it to the range |

Q1 (does auto ever merge or push) answered by the F2 fix: yes, when END names it. Q2 (does a
MINOR block advancement) answered by F4: yes. Q3 (which file gets the git flow) answered by
F9.

### 2026-11-16 — blind round 2 (same fresh session)

All 10 prior findings ADDRESSED. VERDICT: MAJOR, 5 findings, all accepted:

| # | Finding | Outcome |
|---|---|---|
| F11 | "or just B" made the shorthand select full auto while the LOOP default is gated | accepted — shorthand is "defaults" and says gated |
| F12 | Nothing said which reviewer "one" means | accepted — first in roster order unless they name one |
| F13 | The builder skills treat merge and push as unconditional stops | accepted — END's named action is pre-approved, the rest still stop |
| F14 | Phase 0 said small skips reviews, Phase 2 never did | accepted — Phase 2 marked skipped for small, and the size table agrees |
| F15 | The escalation trigger was unverifiable for `ponytail-review` | accepted — "not clean (CLEAN, or Ship under ponytail-review)" |

Q4 answered by F12, Q5 by F14.

### 2026-11-16 — blind round 3 (same fresh session)

All 5 prior findings ADDRESSED. VERDICT: MAJOR, 3 findings, all accepted:

| # | Finding | Outcome |
|---|---|---|
| F16 | CI fix attempts assumed a push that END=leave forbids | accepted — CI is watched only when a push was named; leave has no run to watch |
| F17 | `writing-plans` waits for a plan review the pipeline never names in auto | accepted — the panel is that review, gated approval in gated mode |
| F18 | Standalone queue left "one" undefined | accepted — first in roster order unless named |

Q6 (red CI with END=leave) answered by F16: there is no run to be red, so the local gate is
the whole verification. The queue stopped here at its three-round limit.

### 2026-11-16 — blind reviewer 3

`opencode/glm-5.3-flash-free` returned no verdict: free usage limit reached. Recorded as
**no data**, retried once, then substituted — the roster rule. The pass ran on
`opencode-go/minimax-m3.1-flash` instead (same family as the roster's MiniMax entry, on the
provider that still had quota).

### 2026-11-16 — MiniMax M3.1 Flash, round 1

VERDICT: MAJOR, 10 findings. Nine accepted, one accepted in part.

| # | Finding | Outcome |
|---|---|---|
| F1 | The roster cannot select a model; description is a label, not a selector | **partly rejected.** The mechanism claim is wrong — the `subagent` call takes a `model` argument, and all four reviews above ran on Beam and MiniMax through it. The wording was wrong: "description naming the model" described a label as if it were the selector. Fixed to pass `model` on the call and say the description is only a label |
| F2 | Auto plus defaults gives no spec review and no plan review, then three code reviewers | accepted — plan panel defaults to one, never none by default |
| F3 | `requesting-code-review`'s clean token was invented | accepted — it is `Ready to merge? Yes`; "With fixes" is not clean |
| F4 | A small change skips the plan, then Phase 6 needs builder skills that require one | accepted — small implements directly, the chat design is the brief |
| F5 | Normal and None collide with the builders' test-first mandate | accepted — style is a plan-time decision: Phase 3 writes the steps in the chosen order, so the executor follows its plan |
| F6 | The verification pass could pass with a stale CLEAN | accepted — any fix restarts the pass at the first reviewer |
| F7 | Phase 0 writes the contract file before the worktree exists | accepted — recorded in the spec, written in the worktree at Phase 5 |
| F8 | Auto and the queue's escalation rules contradict each other | accepted — escalation is now an auto stop, and the stop list is a list, not a count |
| F9 | Small change: sheet defaults run four panels; no log destination without a plan | accepted — Phase 7 skipped for small, commit message when the sheet asks for it |
| F10 | Auto can end with an unwatchable CI run | accepted — that is a stop, not a report line |

Q1 answered by rejecting F1's premise: the model was passed on the subagent call. Q2 answered
by F4. Q3 by F6. Q4 by F5.

### 2026-11-16 — MiniMax M3.1 Flash, round 2

9 of 10 ADDRESSED. **F5 not addressed**, and it was right about why: `subagent-driven-development`
never mandates TDD (its brief says "following TDD if the task says to"), so the plan governs
there — while `executing-plans` does mandate RED-GREEN and loads the TDD skill before Task 1.
My "both builders" claim was false.

| # | Finding | Outcome |
|---|---|---|
| F5 | Normal and the builders' test-first mandate still collide; the premise about `subagent-driven-development` was wrong | accepted — Normal requires the per-task builder, inline means TDD, and Phase 3 writes the plan's step order explicitly |
| F11 | STYLE None removes the plan that Phases 4 and 7 still require | accepted — None is small-change and throwaway only; a non-small change cannot pick it |
| F12 | Spec and plan are written before the worktree exists, so they never reach the branch | accepted — the worktree is created in Phase 0, before anything is written; Phase 5 is the baseline run |
| F13 | The two files disagreed on the log destination with no plan | accepted — `review-queue` now names the commit message body for a planless run |
| F14 | The sheet still showed four rows the size rule overrides | accepted — small changes get a four-row sheet |
| F15 | Auto had no rule for a finding only the human can answer | accepted — it is a stop, not a ledger ruling |

Q5 (worktree order) answered by F12: worktree first. Q6 (None on a non-small change) answered
by F11: not offered. Q7 (Normal inline) answered by F5: inline means TDD.

### 2026-11-16 — MiniMax M3.1 Flash, round 3

All 6 prior findings ADDRESSED, each with a residual — every fix opened a seam. Six new
findings, all accepted:

| # | Finding | Outcome |
|---|---|---|
| F16 | Creating a worktree does not move the session into it, so relative writes still land in the primary checkout | accepted — `session_move` is now part of Phase 0, and every later phase runs from that directory |
| F17 | Small changes still got a PLAN PANEL row, and Phase 4 has no exception | accepted — three-row sheet: LOOP, STYLE, END |
| F18 | The sheet allowed Normal with inline, which the style rule forbids | accepted — the sheet says Normal requires the per-task builder, and the agent switches and says why |
| F19 | The contract write pointed at Phase 5, which no longer holds it | accepted — written in Phase 0, where the worktree now exists |
| F20 | The baseline moved to Phase 5, after both panels | accepted — run at creation in Phase 0, reported at Phase 5; a red baseline is a fact, not a stop |
| F21 | The worktree was created before the sheet that claimed to approve it | accepted — consent is asked in Phase 0, and the sheet no longer claims to grant it |

Q8 answered by F16. Q9 by F18: the builder changes. Q10 by F20.

### 2026-11-16 — MiniMax M3.1 Flash, round 4

5 of 6 ADDRESSED. **F19 not addressed**: the new bullet existed but the old sentence still
sent the write to Phase 5. Three new findings, all accepted:

| # | Finding | Outcome |
|---|---|---|
| F19 | The git-flow write still pointed at Phase 5, and the "create `AGENTS.md` if none" clause was lost | accepted — one instruction, in the Phase 0 bullets, with the create-if-none clause back |
| F22 | Moving worktree creation to Phase 0 dropped the project-setup step, so a fresh worktree has no dependencies and the baseline fails on missing modules | accepted — project setup is now a Phase 0 bullet ahead of the baseline, and the phase table names `using-git-worktrees` again |
| F23 | The session moved into the worktree and never back, so cleanup deletes the directory the session is in | accepted — `session_move` back to the primary checkout before removal |
| F24 | A red baseline was "not a stop" but the ship gate had no matching rule | accepted — Phase 8 says how to report baseline-red failures, and a CI run red for the same reason is a stop |

Q11 answered by F22. Q12 by F19. Q13 by F23.

### 2026-11-16 — MiniMax M3.1 Flash, round 5, then the author stopped the queue

All 4 prior findings ADDRESSED, 4 new ones found (F25 MAJOR, F26–F28 MINOR), all fixed:

| # | Finding | Outcome |
|---|---|---|
| F25 | Consent for the worktree had no refusal path, and the "never on main" guard had been deleted in an earlier fix | accepted — declining isolation means a branch in the primary checkout, never main |
| F26 | A red baseline had two rules: the consent skill says ask, the pipeline says report | accepted — the pipeline's rule wins, and says so |
| F27 | The small-change sheet offered STYLE Normal, which was defined as needing a builder the small path does not use | accepted — Normal on the small path means implement then test, same commit |
| F28 | "The plan's status" named no field | accepted — `Status: done`, added by the pipeline since `writing-plans` has none |

**Then the author stopped the reviews.** Five rounds of patching had produced about 70 findings
across three reviewers, and each round's fixes opened a seam for the next round. That is a
process failure by the author, not by the reviewer: a document was being changed faster than it
could be read back.

## 2026-11-16 — editorial rewrite

One full read, then a rewrite, instead of more patches. Removed or merged:

- the separate baseline phase, whose content was a report of something Phase 0 had already done;
- Phase 2.5, and the numbering with it: 0 workspace, 1 spec, 2 spec review, 3 sheet, 4 plan,
  5 plan review, 6 implement, 7 code review, 8 ship;
- five separate statements of the small path, now one block plus a sentence per phase;
- three separate definitions of STYLE, now one table and two constraints;
- the duplicated log destinations between the two files.

Contradictions fixed in the rewrite: phase cross-references that pointed at the old numbering,
"only CLEAN advances" against code briefs that emit other tokens, and the sheet offering
combinations the STYLE section forbade.

Self-check run, not asserted: every `Phase N` reference grepped against the headings; every
statement about the small path grepped and compared; `clean` versus `CLEAN` grepped in
`review-queue`. Nine phases, headings and references agree, the small path is stated once and
referenced three times.

## 2026-11-16 — human-owned spec gate

A change the human asked for, not a review round. The order moved: the spec is now confirmed
**after** the external review, not before it.

- Phase 1 asks whatever the spec needs — questions, not only doubts — and the file it writes is
  not approved yet.
- Phase 2 asks one question (which reviewers), then always asks the human: is this spec right as
  it stands, or does something change? The spec has no after-green switch.
- The sheet stays where it was: after the spec confirmation, before the plan.
- A small change has no spec review, so its chat design is confirmed at the end of Phase 1.

Stated in the file instead of hidden: the mode (then called `full auto`, now `autopilot`) governs
the run from the sheet onward. The spec
questions and the spec confirmation happen before the mode exists, so they happen in every mode.

Self-check run, not asserted: every `Phase N` reference grepped against the headings; every
`approve`, `after-green` and `spec approval` mention grepped for leftovers from the old order.
Nine phases, headings and references agree; no reference points at the old Phase 1 approval.

## 2026-11-16 — autopilot and the final stop

A change the human asked for, not a review round. Two things:

**`full auto` is now `autopilot`.** The old name promised no stops anywhere; the mode has never
been that, because the spec needs the human until it is confirmed. The new name says what it is:
from the settings sheet on, nothing stops the run. "full auto" stays in the description as a
phrase that triggers the skill, because that is what people say.

**A fourth stop, before shipping.** `stop before shipping` runs everything — plan, code, tests,
reviews — and stops once, after the code is finished, the tests are green and the reviewers said
OK, before anything is merged or pushed. At that stop the human is asked END and PUBLISH again;
the sheet's answer is the default, the answer there rules.

Two ship-phase holes closed with it:

- The build-watching rule follows the push, not the END. Something pushed → watch that run until
  green. Nothing pushed → the local test run is the whole verification. Before, "leave the
  branch" + "push only" ignored a running build, and "merge" + "wait for me" waited for a build
  that would never start.
- END=PR pushes the branch as part of opening the PR, so PUBLISH=`wait for me` is not offered
  with END=PR. There would be no remote action to wait for.

Also: the finishing skill's integration step is no longer repeated (step 3 does the merge), and
the local gate runs once more on the merged result before anything is pushed.

Self-check run, not asserted: every `full auto` mention grepped — the only two left are the
trigger phrases in the description and Step 0; every `stop before shipping` mention grepped and
counted (the sheet row, the stop's own bullet, the overlap rule, Phase 8); the STOPS list and the
sheet row agree on four options.

## 2026-11-16 — the pipeline only, nothing smaller

A change the human asked for. Seven things, one pass.

**The small path is gone.** A small change does not use this pipeline, so the size question, the
size table, the four-row sheet and every "a small change…" sentence are deleted. Every run now
gets the spec file, the spec review, the plan, the plan review, the code review and the ship
steps. The one thing that stays is Step 0: if the pipeline was not named in the request, the
skill always asks whether it should run.

**The git-flow answer goes in the spec.** The second destination — the contract file — is gone.

**The git questions are set once.** END and PUBLISH are settled in the sheet. The final stop shows
what shipped and waits; it does not ask them again. That also removes the dead pair: with only
one place to choose them, "END=PR + wait for me" cannot be picked at a second stop.

**A missing model stops the run.** In `review-queue`: retry once, then stop and ask the human
which model to use. The automatic substitution onto another provider is deleted — the human
picks.

**The word "ledger" is gone** from both files. Rulings go in the spec while it is written and in
the plan file after that; the code review log goes in the plan file, or in the commit message
body when there is no plan.

**The verdict marker matches.** The `requesting-code-review` template's example wrote
`Ready to merge: With fixes` while the instruction line said `Ready to merge?`. The example is
fixed, and `review-queue` now reads the verdict as `Ready to merge` + `Yes`, so both spellings
count. The template lives in the installed superpowers package, so the second half is the one that
survives a reinstall.

**STYLE is decided in the sheet** and Phase 4 writes the order into the plan — the old sentence
said it was decided when the plan was written, which read as a second question.

**The excuse table is 11 rows, down from 18.** Cut: the two rows that only repeated a law, the
three that repeated a rule stated in the body, and the two the delegated `review-queue` already
carries. The CI pair became one row. What is left is the trap that exists nowhere else in the
file.

Self-check run, not asserted: `small`, `size`, `ledger` and `planless` grepped across both files —
none left; every `Phase N` reference grepped against the headings; the sheet's rows counted
against the STOPS options.

## 2026-11-16 — autonomous by default

A change the human asked for: the pipeline is autonomous, so the defaults must not stop.

**The defaults changed.** STOPS defaults to `autopilot`, and all three review rows default to
`continue`. A default run asks the human three things and then goes: the worktree consent, the
git flow if the repo is silent on it, and the spec confirmation. Everything after the sheet runs
without a pause.

**The two dials are now explicit.** STOPS is where the run pauses on its own. The review rows are
the other dial: a review point set to `wait` is asked wherever it lands, in every mode, including
`autopilot`. That is what the sheet is for — you cannot know which steps need a human until the
spec says what the work is. The file contradicted itself here ("no review wait applies in
autopilot" against "a review point set to `wait` stops the run, even on autopilot"); the second
one won, because it is the one the human wants.

**Nobody approves the plan** under `stop before shipping` and `autopilot` unless the plan review
says `wait`. Stated in Phase 4 instead of left to be inferred.

**Three smaller fixes.** The setup questions are asked once each, not dripped (law 6). The
git-flow answer is remembered in Phase 0 and written into the spec when the spec is written — the
spec does not exist yet at that point. END=leave no longer makes PUBLISH meaningless: it means no
merge and no PR, and PUBLISH still says whether the branch is pushed.

Self-check run, not asserted: the sheet's defaults grepped against the prose that follows them;
`wait` and `continue` read through every phase that mentions them; the autopilot paragraph
compared against the dials paragraph.

## 2026-11-16 — the roster, checked against the catalogue

Not a review round: a usability check before calling the skill usable. The models tool was asked
for the roster, and one entry was gone.

- `opencode-go/beam-free#max` — exists, variants include `max`.
- `opencode/minimax-m3.1-flash-free` — exists.
- `opencode/glm-5.3-flash-free` — **no longer in the catalogue.** The free GLM is not listed at
  all; the catalogue has `opencode/glm-5.3-flash` and `opencode-go/glm-5.3-flash`. The roster now
  lists `opencode/glm-5.3-flash#max` and no longer calls it free.

Law 3 covers the case where one disappears mid-run; this was the same case caught before the
first dispatch instead of during it.

**Then: the catalogue was wrong.** The models tool listed no `glm-5.3-flash-free`, and the roster
was changed twice on the strength of that — first to `opencode/glm-5.3-flash#max`, then to a
different model entirely, which the human did not ask for and was rightly called out. The human
then used the original ID and it worked: `opencode/glm-5.3-flash-free` (provider `opencode`,
model `glm-5.3-flash-free`). The roster keeps the original ID. The lesson is now in the file: a
model missing from the catalogue can still be usable — try it once before stopping, and only a
failed call counts as gone.

**All three reviewers run at `max`.** The roster carries `#max` on every one of them, and the file
says not to drop it. Verified by a live call, not by the catalogue: `opencode/glm-5.3-flash-free#max`
answered `hi` on request — the free GLM takes the `max` suffix even though the models tool does not
list it at all. The catalogue is a hint, not a verdict.

## 2026-11-16 — one reviewer, one loop, no second pass

A change the human asked for. The verification pass was mine, not theirs.

**The second pass is gone.** `review-queue` had a fourth law: when the last reviewer went clean,
run the whole queue again over the final text, and any fix in that pass restarted it from reviewer
one. That is not what was asked for. The queue is now what the human described:

- the review runs;
- the main agent fixes everything the reviewer found;
- the same reviewer gets the updated artifact again, in the same child session, and answers two
  things — are the old findings fixed, is there anything new;
- that reviewer loops until it says OK;
- then the next reviewer starts, and loops the same way.

Consequence, stated: a reviewer that went clean does not see the text a later reviewer changed.
That is the gap the second pass covered.

**The superpowers template is back to its original** (`Ready to merge: With fixes` in the example).
The mismatch was never the template's fault — its instruction line says `Ready to merge?` and its
example says `Ready to merge:`. `review-queue` now reads the verdict as the word `Ready to merge`
plus `Yes`, so both spellings count, and nothing in an installed package is patched.

Self-check run, not asserted: `verification pass` and `final text` grepped across both skill files
— none left; the clean-token table and the template's two spellings compared by hand.

## 2026-11-16 — the standalone log

A change the human asked for. The standalone case said the log goes in the artifact, and the
artifact for a PR review is the PR — while the same file forbids a PR description. One clause
settles it: the artifact, except a PR, where the log goes to a file next to it. The "never a PR
description" rule now holds in both cases.

## 2026-11-16 — one owner for review, one workflow for tests

A change the human asked for, after a review of this file and `review-queue` as a pair. Nine
findings, all accepted.

| # | Finding | Outcome |
|---|---|---|
| F1 | The builder's own review and the external review applied different severities, so the same finding could be done or outstanding | accepted — `review-queue` is the final authority: fix or rule Critical/Important automatically, rule Minor with a reason, and use the external clean verdict before advancing |
| F2 | Autopilot's "plan broken past guessing" stop had no boundary, against the builders' rule-and-continue rule | accepted — the stop is a plan whose every available path risks the spec, scope, or an irreversible boundary |
| F3 | The phase table and the stop prose disagreed about plan approval | accepted — one rule: the question is asked only where a stop lands on it |
| F4 | The `STYLE` row offered Normal, which needed a builder the sheet did not guarantee | accepted — `STYLE` is gone; TDD is mandatory and is not asked |
| F5 | `brainstorming`'s builder check and pre-review approval gate were not clearly excluded | accepted — the named-pipeline sequence replaces them, stated in Phase 1 |
| F6 | The log was written into the artifact the reviewers were told not to edit | accepted — reviewers are read-only, the controller appends the log |
| F7 | Code review accepted "the plan or spec" as the requirement source | accepted — the spec is the requirement source, the plan is supporting context |
| F8 | Shipping updated the spec, `AGENTS.md` and `README.md` unconditionally | accepted — each document is updated only when its content changed; the approved spec is never edited during shipping |
| F9 | `push and tag` pushed first and tagged after, so the tag never reached the remote | accepted — create the tag, push the branch, push the tag |

Also settled: the approved spec is frozen. Implementation must not change it; a conflict stops
the run, and any change needs a new explicit change request and a new review/approval cycle.
Repository instructions have a precedence: `AGENTS.md` first, then the nearest applicable
`CLAUDE.md` or `CONTRIBUTING.md`, and a conflict between them stops the run instead of guessing.

Self-check run, not asserted: `STYLE`, `Normal`, `code, then the test`, `invert the order`,
`approve / change (unless autopilot)`, `plan or spec`, `ledger` and `verify` grepped across both
skill files; `Create the version tag`, `push the tag`, `AGENTS.md first`, `approved spec is
frozen` and `new explicit change request` confirmed present; every `Phase N` reference grepped
against the headings; a scripted check of the fifteen rules above run red before the edits and
green after. Each matching passage was read in context.

## 2026-11-16 — GLM 5.3 Flash Free, round 1

VERDICT: MAJOR, 5 findings, 2 questions. Four accepted, one clarified; both questions ruled.

| # | Finding | Outcome |
|---|---|---|
| F1 | `Receiving` let Minor findings "go to the log", against law 2 and the severity rule | accepted — Minor is fixed or ruled away with a reason; the queue law governs |
| F2 | Phase 5 had no path for `Change` at the plan gate, and "the plan becomes the contract either way" implied the answer had no effect | accepted — `Change` returns to Phase 4 with the note and the queue re-runs, mirroring Phase 2 |
| F3 | "the list" meant two different lists, with opposite instructions for a model missing from it | accepted — the models-tool catalogue is a starting point; only a failed call counts as gone |
| F4 | Reviewer QUESTIONS batched "one message per reviewer" against law 6's one-at-a-time | clarified — one at a time, never mixed with another reviewer's |
| F5 | The review log sits in the artifact between rounds, so the next reviewer reads the author's rulings | accepted — the briefs now say the log is a record, not evidence; judge the document itself |

Q1 (plan `Change`: loop back, or stop and let the human drive) ruled: loop back, same as the spec.
Q2 (reviewer questions: batched or one at a time) ruled: one at a time.

**Round 2 did not run on GLM.** Both attempts returned "Free usage limit reached". Per law 3 the
queue stopped and the human chose the replacement: Beam Free (max) took the round. A substitute
reviewer gets a clean brief — never the previous reviewer's findings — so Beam read the current
text cold.

## 2026-11-16 — Beam Free (max), round 2

**VERDICT: CLEAN.** No findings, no questions. Beam read the three files cold, against the
delegated skills and this log, with the human's seven requests as the standard. Nothing to change.

The queue is green: GLM's five findings were fixed before this read, and the cold pass found
nothing in the current text.

## 2026-11-16 — DeepSeek V4.1 Flash 1017, round 1

VERDICT: BLOCKER, 8 findings, 3 questions. All eight accepted after checking each against the
files and the delegated skills; three questions ruled.

| # | Finding | Outcome |
|---|---|---|
| F1 | Step 0 let "plainly asked for a spec-then-plan run" skip the consent question | accepted — only an explicit trigger phrase skips it; a paraphrase gets the question |
| F2 | The default `PUBLISH wait for me` guarantees a stop, against "the defaults stop you for nothing" | accepted — that one stop is now named, and the push gate stays |
| F3 | `ponytail-review` never emits `Verdict: Ship.` on a clean review | accepted — its clean output is `Looks good. Ship.`; both tokens count now |
| F4 | Moving the session out before the finishing skill defeats its worktree detection | accepted — run it from the worktree, move out when its cleanup removes it |
| F5 | `stop at each step` promised a per-task pause Phase 7 overrode | accepted — the STOPS mode pauses whatever the review row says |
| F6 | The batched sheet can return END=PR with `wait for me` | accepted — END wins the push, PUBLISH then decides only the tag |
| F7 | The autopilot interruption list omitted the missing-model stop | accepted — added, with the push gate named beside it |
| F8 | Law 3's "cannot be resolved at all" against "try it once" | accepted — the trial call is the test, not the catalogue |

Q1 (should the default PUBLISH push?) ruled: no — a push is the one action the run cannot take
back, and it is now the single named pause in the defaults.
Q2 (does a paraphrase count as naming the pipeline?) ruled: no — explicit trigger phrase or nothing.
Q3 (does TASK REVIEW `continue` cancel the per-task stop?) ruled: no — STOPS and the review row
are separate dials, which the file now says in Phase 7.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, round 2

All 8 prior findings ADDRESSED. VERDICT: MAJOR, 2 new findings, both accepted.

| # | Finding | Outcome |
|---|---|---|
| N1 | The default END + PUBLISH pair can itself be the forbidden pair, deleting the one default pause in PR-flow repos | accepted — the special case was the bug. PUBLISH now governs every remote action, so `END=PR` with `wait for me` stops before the push and hands the branch over. No pair is forbidden and the conflict rule is gone |
| N2 | "Autopilot obeys PUBLISH" contradicted "END wins the push" | accepted — with one rule the sentence is true again; the "END wins" clause is deleted |

Q1 (in a PR-flow repo, pause before the PR or push because END named it?) ruled: pause. The PR is
a push, so the default `wait for me` holds it.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, round 3

N1 and N2 ADDRESSED. VERDICT: MAJOR, 1 new finding, accepted.

| # | Finding | Outcome |
|---|---|---|
| N3 | Phase 8 step 5 said "take the option END named", which for END=PR is the push-and-PR action step 3 just held back | accepted — step 5 takes the cleanup only: no menu, no second merge, no second push, no PR created twice. On `wait for me` the remote action stays with the human |

**Escalation.** Round 3 is the limit this skill sets for one reviewer. Every round found something
real — 8 then 2 then 1 — but the rule is the rule: the queue stops here and the human decides
whether a fourth round runs.

**The human ruled: accepted.** No fourth round. N3 is fixed and the file stands as it is. The
queue for this artifact ends green-by-decision, not by a clean verdict from DeepSeek.

## 2026-11-16 — Beam Free (max), post-DeepSeek read

VERDICT: MAJOR, 3 findings. All three accepted; each claim checked against the delegated skill's
own text first.

| # | Finding | Outcome |
|---|---|---|
| F1 | On declining the worktree, the pipeline says "create a branch" while `using-git-worktrees` says "work in place" — with no override stated | accepted — the bullet now says it overrides that skill's work-in-place path, so declining isolation is not declining the branch |
| F2 | `finishing-a-development-branch` removes only worktrees under `.worktrees/` or `worktrees/` ("everything else belongs to the host"), so the pipeline's native-tool worktree survived it | accepted — the skill is run for its branch checks only, and the pipeline removes its own worktree with `worktree_remove` after `session_move` |
| F3 | `writing-plans` ends by asking the human to confirm the plan; Phase 4 only implied the pipeline could suppress it | accepted — Phase 4 now says the pipeline decides whether that question surfaces, and that its rule wins |

## 2026-11-16 — Beam Free (max), post-DeepSeek read, round 2

All 3 prior findings ADDRESSED. **VERDICT: CLEAN.** No findings, no questions.

Beam is green on the current text, and the three mechanism errors it found — the work-in-place
path, the worktree removal the finishing skill never does, and the `writing-plans` approval
question — are closed.

The pattern worth keeping: every reviewer that found a *mechanism* error found it in a claim about
a delegated skill, and the document now states each of those overrides explicitly (Phase 1's
`brainstorming` gates, Phase 0's work-in-place path, Phase 4's approval question, Phase 8's
worktree ownership). Claims about someone else's behaviour need their sentence quoted, not
remembered.

## 2026-11-16 — DeepSeek V4.1 Flash 1017, fresh read

VERDICT: MAJOR, 10 findings, 3 questions. All ten accepted; each mechanism claim checked against
the delegated skill's own text first; three questions ruled.

| # | Finding | Outcome |
|---|---|---|
| F1 | `stop at each step`'s per-task pause against both builders' "Do not pause to check in between tasks" | accepted — Phase 6 says the sheet overrides that sentence |
| F2 | END=PR shipped on a push that cannot create a PR | accepted — a push does not create a PR; after it the run opens the pull request with the forge's CLI, or prints the creation URL |
| F3 | The cleanup override assumed the worktree's path, but the skill tests the path, not the creator | accepted — the skill's cleanup step is skipped outright and the pipeline removes its own worktree |
| F4 | "Named or not, the pipeline only runs when it is named" read as no question for unnamed requests | accepted — rewritten as "runs only when the human names it or picks Run the pipeline" |
| F5 | Phase 5 did not say `stop at each step` stops for plan approval | accepted — named beside `stop after the auto review` |
| F6 | Step 5 had both "remove after a merge" and "keep when the remote action was left" true at once | accepted — remove after a merge; keep when the worktree holds work that has not reached the remote |
| F7 | Phase 0 never ran the worktree skill's existing-isolation check | accepted — its Step 0 detection runs before creating anything |
| F8 | The neighbours menu expected a typed reply, against law 6 | accepted — it goes through the selector, one entry per neighbour plus `none` |
| F9 | Phase 1's "no filter" against `brainstorming`'s "ask only when the answer would change what gets built" | accepted — the filter is now named among the replaced rules |
| F10 | Standalone `review-queue` never chose the code kit, though the clean verdict depends on it | accepted — the kit is asked too, default `both` |

Q1 (per-task pause or the builders' continuous execution?) ruled: the sheet wins — the controller
takes the pause.
Q2 (does END=PR mean the pipeline creates the PR?) ruled: yes, with the forge's CLI or the creation
URL; a push alone is not a PR.
Q3 (force the worktree outside `.worktrees/`, or skip the cleanup?) ruled: skip the cleanup — the
pipeline owns its worktree and removes it with `worktree_remove`.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, fresh read, round 2

All 10 prior findings ADDRESSED. VERDICT: MAJOR, 7 new findings, all accepted.

| # | Finding | Outcome |
|---|---|---|
| N1 | The new isolation check made creation unconditional, so a declined worktree could still be created | accepted — creation is conditional on consent; a decline runs setup and baseline in the primary checkout on the branch |
| N2 | `push and tag` pushed the tag before CI was green, leaving a release on a rejected commit | accepted — the tag is created, but pushed only after the run is green |
| N3 | A pushed END=PR removed the worktree the finishing skill says to keep for PR feedback | accepted — the worktree is kept for END=PR; only a merge removes it |
| N4 | "Two things must be settled" then a third for code | accepted — reviewers and log for every artifact, the kit for a code artifact |
| N5 | Standalone code reviews had two log destinations | accepted — a file beside the diff, and the log section says the same |
| N6 | The unwatchable-CI rule named two of the four STOPS modes | accepted — every mode hands the URL over; only autopilot stops and reports |
| N7 | After a merge the worktree went but the merged branch stayed | accepted — `git branch -d` after the merge, which is the skill's own Option 1 cleanup |

### 2026-11-16 — DeepSeek V4.1 Flash 1017, fresh read, round 3

6 of 7 ADDRESSED. **N2 NOT ADDRESSED**, and correctly: the tag's push waited for green but its
creation did not, so a red run still left the tag on the rejected commit. VERDICT: MAJOR,
3 further findings, all accepted.

| # | Finding | Outcome |
|---|---|---|
| N2 | The tag was created before the green run | accepted — the tag is neither created nor pushed until the run is green; step 3 pushes the branch only, and Phase 3's `push and tag` bullet says the same |
| N8 | The isolation check ran after the consent question, though it says "first" | accepted — detection moves ahead of consent, and an already-isolated session is not asked at all |
| N9 | The pipeline's own `worktree_remove` had no refusal path for a dirty worktree | accepted — stop and show the files; commit, move or delete on the human's word; never force |
| N10 | Phase 6 pre-approved "the merge the sheet named" while END can name a push | accepted — it now says "the merge or push the sheet named" |

**Escalation.** Round 3 with this reviewer and still no clean verdict. The queue stops; the human
decides whether a fourth round runs.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, fresh read, round 4

All 4 prior findings ADDRESSED. VERDICT: MAJOR, 4 new findings, all MINOR and all accepted — the
severity has dropped every round.

| # | Finding | Outcome |
|---|---|---|
| N11 | "A file written in the primary checkout never reaches the branch" was false after a decline, where the branch is the primary checkout | accepted — the sentence is now scoped to the worktree case |
| N12 | The tag's release run was never watched; a red one would leave the tag public with no rule | accepted — the tag push starts its own run and a red one is a stop that names the tag |
| N13 | The refusal's "commit" option after a merge leaves the branch unmerged, so `git branch -d` would refuse | accepted — after a merge only move or delete is offered |
| N14 | The version was asked at the publish, a later gate the sheet said would not exist | accepted — a VERSION row joins the sheet when `push and tag` is chosen and the repo has no tag convention |

**Escalation again.** Round 4, no clean verdict. The human asked for this round knowingly; a fifth
needs the same call.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, fresh read, round 5

All 4 prior findings ADDRESSED. VERDICT: MAJOR, 1 new finding (MINOR), accepted.

| # | Finding | Outcome |
|---|---|---|
| N15 | The VERSION row was described in the publishing bullet but never joined the sheet table, so "one entry per row above" would omit it | accepted — VERSION is a row of the table, marked conditional, and the batching rule says when the same call carries it |

**The class has not changed in five rounds.** Every finding since round 3 is the same shape: one
statement in one place, a different statement about the same thing in another. That is not a
reasoning error any more, it is a document-wide editorial problem, and the log's own history says
what fixed it last time — one full read and a rewrite, not another patch.

**Escalation.** Round 5, no clean verdict. The human rules.

## 2026-11-17 — pick once, substitute top-down

The reviewer question moved from a count to a single multi-select, chosen at Phase 2 and reused for
the whole run.

| Decision | Where it landed |
|---|---|
| The menu | Phase 2: all roster names in order, first preselected, plus `None`; zero ticked means `None`, a dismissed picker is re-asked |
| One selection for the whole run | recorded in the spec as `Reviewers: …`; the sheet keeps only `continue / wait`, and Phase 5/7 read the spec line |
| Failure, non-autopilot | `stop`: retry once, then ask the human |
| Failure, autopilot | `substitute`: retry once, take the next unused roster row top-down, one substitute per failed slot so the count does not shrink, then stop if the roster is exhausted |
| Audit | the final report names every substitution: who was asked for, who ran, why |

The plan review surfaced seven gaps before implementation, all closed: the frozen-spec problem
(substitutions are reported, not written into the spec), the queue not knowing the mode (the policy
is now passed in), `None` at Phase 2 (straight to the human confirmation), the count after a
substitution (unchanged), Phase 5/7 reading the old source, two stale statements, and the empty-menu
case. One more was caught while re-reading: **the spec review happens before the sheet chooses the
mode**, so it must use `stop`; `substitute` begins at the plan review.

## 2026-11-16 — editorial pass, then a cold read

The pass first, no reviewer: one full read of all three files, five reconciliations. The autopilot
paragraph's broken wrap; "END decides what integration happens" stated twice in ten lines; the
VERSION condition written so it could not be evaluated in the batched call; Phase 8 step 5 running
a skill whose checks the pipeline had already performed; the phase table implying that skill is
invoked.

Then one cold read — fresh session, standard brief, no knowledge of the rounds above.
VERDICT: MAJOR, 10 findings, all accepted; 3 questions ruled.

| # | Finding | Outcome |
|---|---|---|
| F1 | Plan approval was forbidden under autopilot *and* required when the plan review says `wait` | accepted — Phase 4 gains "unless the plan review says `wait`" |
| F2 | "Clean" was a verdict token in the table and zero open findings in the law, so a Minor could pass as green | accepted — clean needs an empty findings list; the tokens now follow law 2 |
| F3 | The briefs' fresh-context rule covered the resend block, forbidding the same-session round 2 | accepted — the rule is scoped to the first-round briefs, and the resend is named as the exception |
| F4 | Phase 6 built every task before Phase 7's per-task review, so that gate gated nothing | accepted — phases 6 and 7 interleave; the next task waits for the review |
| F5 | `executing-plans` says not to dispatch a re-review; Phase 7 dispatched one with no override | accepted — the sheet's BRANCH REVIEW is named as the override |
| F6 | "Skip a phase only with the human's word at Phase 0" contradicted law 6 and the sheet | accepted — deleted; the sheet's `none` rows are the mechanism |
| F7 | The plan brief lacked the two rules that make `VERDICT: CLEAN` mean no findings | accepted — both copied in |
| F8 | The VERSION condition was stated two ways, one of them unevaluable at call time | accepted — the table carries the value, the batching rule carries the condition |
| F9 | `PLAN REVIEW = none` had no defined behaviour and removed the `stop after the auto review` stop | accepted — defined: a plain plan approval |
| F10 | The log lived in the artifact between rounds, so a later reviewer read an earlier one's findings | accepted — the log is written once the queue is green |

Q1 (approve the plan under autopilot when the review says `wait`?) ruled: yes.
Q2 (does the per-task review gate the next task?) ruled: yes — it interleaves.
Q3 (does the default `wait for me` push stop stand?) ruled: yes, it is the one named default stop.

## 2026-11-16 — progressive disclosure

The file was one piece: 414 lines, 27 KB, all of it loaded before the human had even answered the
Step 0 question. Split into four, so a declined invocation and the spec phase cost a fraction:

| File | Lines | Holds |
|---|---|---|
| `SKILL.md` | 194 | Step 0, the laws, the phase map, Phases 0–2, the rationalization table |
| `references/settings.md` | 117 | Phase 3 — the sheet, the stops, the two dials, publishing, the builder |
| `references/plan-and-build.md` | 75 | Phases 4–7 — plan, plan review, implement, code review |
| `references/ship.md` | 50 | Phase 8 and the report |

Each later file is read when its phase starts, with an explicit pointer at the end of the phase
before it — the same pattern `review-queue` uses for its briefs.

Verified by comparison, not by eye: every line of the pre-split file was looked up in the new files.
Two did not match, and both are the intentional heading change — "Phase 3" and "Phase 8" became the
H1 titles of their own files. Nothing else was lost.

Cost: the first load drops from 27 KB to 11.7 KB, and a full run pays 7 KB, 4.9 KB and 4 KB at the
three boundaries. A run that is declined at Step 0 pays only the 11.7 KB.

## 2026-11-16 — Beam Free (max), on the split

VERDICT: MAJOR, 1 finding, accepted.

| # | Finding | Outcome |
|---|---|---|
| F1 | The phase table's row 3 still said "this file" after Phase 3 moved to `references/settings.md` | accepted — the row now names the file, so the map and the pointer agree |

The split's own failure mode, and the reviewer went straight at it: an agent using the phase table
as its map would look for Phase 3 in `SKILL.md` and find nothing.

### 2026-11-16 — Beam Free (max), on the split, round 2

F1 ADDRESSED. **VERDICT: CLEAN.** No findings, no questions. The split is green as it stands.

## 2026-11-16 — DeepSeek V4.1 Flash 1017, on the split

VERDICT: MAJOR, 7 findings, 3 questions. All accepted; three ruled.

| # | Finding | Outcome |
|---|---|---|
| F1 | Git flow was asked twice — Phase 0 for the spec's constraints and the sheet's END row | accepted — the sheet owns it; Phase 0 only records that the contract is silent |
| F2 | `decide later` deferred a sheet question past the plan, into modes that promise no pause there | accepted — `decide later` is gone; the kit is settled on the sheet |
| F3 | A blocking open question had no path at the gate, which offered only Approved/Change | accepted — the gate is that question first, and approval is not offered until none is left |
| F4 | `executing-plans`' "no implementer subagent per task, no reviewer per task" was not among the overridden clauses | accepted — named beside the re-review override |
| F5 | Ship step 5 assumed a worktree, though Phase 0 has a legal decline path | accepted — the removal is gated on a worktree existing |
| F6 | Phase 1 dropped `brainstorming`'s mandatory Approaches part while claiming to replace only three of its rules | accepted — "the approaches considered and why this one" is in the doc's content list |
| F7 | The per-task pause of `stop at each step` had no defined question | accepted — it shows the task's green summary and asks continue or stop |

Q1 (who owns the git-flow answer?) ruled: the sheet, once.
Q2 (should the reviewer gap be a hard stop?) ruled: accepted as-is — the loop reviews the text it is
shown, which is what the human asked for.
Q3 (does `decide later` survive?) ruled: no, it is dropped.

### 2026-11-16 — DeepSeek V4.1 Flash 1017, on the split, round 2

All 7 prior findings ADDRESSED. VERDICT: MAJOR, 3 new findings, all accepted.

| # | Finding | Outcome |
|---|---|---|
| F8 | Answering a blocking open question at the gate edited the spec after the queue went green, with no re-run | accepted — the answer is written into the spec, the queue re-runs on the answered text, and approval waits for both |
| F9 | Law 6 still listed git flow as its own question, rebuilding the double ask F1 closed | accepted — law 6 says git flow is the sheet's END entry |
| F10 | The override sentence claimed the review rows override "no implementer subagent per task", which no row touches | accepted — scoped: TASK REVIEW overrides the reviewer sentence, BRANCH REVIEW the re-review one, and the no-implementer sentence stands |

### 2026-11-16 — DeepSeek V4.1 Flash 1017, on the split, round 3

All 3 prior findings ADDRESSED. **VERDICT: CLEAN.** No findings, no questions.

Both reviewer families are now green on the split: Beam in two rounds, DeepSeek in three. The file
set stands as it is.

## 2026-11-16 — Beam Free (max), after the DeepSeek fixes

VERDICT: MAJOR, 1 finding, accepted.

| # | Finding | Outcome |
|---|---|---|
| F1 | Step 0's list of trigger phrases omitted "auto review", which the frontmatter description counts as naming the pipeline | accepted — the two lists now name the same six phrases, so a request that loads the skill by that phrase goes straight to Phase 0 instead of asking |

### 2026-11-16 — Beam Free (max), after the DeepSeek fixes, round 2

F1 ADDRESSED. **VERDICT: CLEAN.** No findings, no questions.

Both families are green on the same text again: Beam in two rounds, DeepSeek in three.

### 2026-11-16 — GLM 5.3 Flash Free: no data

Two attempts, both `Free usage limit reached`. Per law 3 the queue stops and the human picks the
replacement. Recorded as **no data**: it does not count as clean, and it changes nothing about the
text.

### 2026-11-16 — GLM 5.3 Flash (Go), on the split

The free line caps out and the pay-as-you-go line has no credits; the Go provider serves the same
model, so `opencode-go/glm-5.3-flash#max` took the round. VERDICT: MAJOR, 5 findings, 1 question.
All accepted.

| # | Finding | Outcome |
|---|---|---|
| F1 | A finding the controller ruled away could never produce a clean verdict: the reviewer never saw the ruling, so an unchanged finding was re-raised every round into the escalation stop | accepted — the resend addendum carries the controller's `Ruling:` on that finding's line, and the reviewer answers ADDRESSED when the ruling holds |
| F2 | The `requesting-code-review` clean condition ignored the template's mandatory Declined-to-judge list | accepted — every declined line needs a ruling before the review is green |
| F3 | "END=leave stops" read as the run halting, against the publish step that follows | accepted — "has nothing to integrate", with "stops" kept for real pauses |
| F4 | The batching rule referenced a `(Recommended)` label the table never carried | accepted — the Default column is the recommendation, offered first |
| F5 | The phase map named `verification-before-completion` for Phase 8; ship.md never did | accepted — step 1 now states that it is that skill's rule |

Q1 (does a passed-back `sessionID` resume the reviewer?) answered from this session's own rounds:
yes — every resend that carried the ID continued with its earlier findings in context.

### 2026-11-16 — GLM 5.3 Flash (Go), on the split, round 2

4 ADDRESSED. **F1 NOT ADDRESSED**, and correctly: the `Ruling:` line went into the fresh-session
paragraph, while the path that actually runs is the same-session one. The rule is now on the primary
resend: every resend carries the addendum, same session or fresh.

| # | Finding | Outcome |
|---|---|---|
| F1 | The ruling line was scoped to the fallback path only | accepted — "Every resend carries the addendum", with the `Ruling:` line on any ruled-away finding |

### 2026-11-16 — GLM 5.3 Flash (Go), on the split, round 3

F1 ADDRESSED. **VERDICT: CLEAN.** No findings, no questions.

Three families have now read this artifact, and all three ended green — Beam, DeepSeek and GLM. The
last five fixes (GLM's) landed after the other two had signed off, so GLM's is the only clean
verdict on the exact current text; the other two are green on the text that preceded it.

Note for the record: the two skill files were edited by someone else between rounds — the spec
freeze, `TDD is mandatory`, the contract-conflict rule, the publish order and the read-only
clauses are theirs. None of it was reverted; only the five findings above were touched.
