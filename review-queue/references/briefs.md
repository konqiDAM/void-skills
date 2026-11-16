# Review briefs

The exact text to send: one block per artifact type, plus the resend addendum. Read this before
the first dispatch of a review; the rest of the queue's machinery stays in `SKILL.md`.

Rules that apply to the first-round blocks below (spec, plan, code):

- Fresh context, read-only, no knowledge of the author's reasoning, and no sight of the other
  reviewers.
- Never add the author's rationale, a preferred answer, or another reviewer's findings.
- The reviewer never edits a file. The controller applies the fixes and writes the review log.
- If an artifact already carries a `## Review log`, ignore it — it is a record of the review, not
  part of the document.

The resend block at the bottom is not one of these: it goes to the same reviewer, in the same
session where the run allows it, and it is meant to see its own previous findings.

## Spec

```
You are reviewing a design spec you did not write. Judge the document alone.

Spec: <absolute path>
Codebase: <repo path> — read the code this spec touches before judging.
Original request, verbatim: <the user's words>

Report exactly this shape, nothing else:

VERDICT: CLEAN | MAJOR | BLOCKER
FINDINGS:
- F1 [BLOCKER|MAJOR|MINOR] <one line>
  Evidence: "<quote from the spec>"
  Why it matters: <one concrete failure it causes>
  Suggested change: <one sentence>
QUESTIONS:
- Q1 <question only the human can answer>

Rules: no file edits, no rewritten spec, no praise, no summary of the spec. Max 10 findings. A
speculative finding is MINOR. If you would approve it as-is: VERDICT: CLEAN and an empty
FINDINGS list.
```

## Plan

```
You are reviewing an implementation plan you did not write.

Spec: <absolute path>
Plan: <absolute path>
Codebase: <repo path> — read the code this plan touches.

Judge only: files that will break and are not named; task order that cannot work; tasks too big
to review or too small to matter; missing tests for a stated acceptance criterion; hidden coupling
between tasks; risk with no mitigation.

Report exactly this shape, nothing else:
VERDICT: CLEAN | MAJOR | BLOCKER
FINDINGS:
- F1 [BLOCKER|MAJOR|MINOR] <one line>
  Evidence: "<quote from the plan>"
  Why it matters: <one concrete failure>
  Suggested change: <one sentence>
QUESTIONS:
- Q1 <question only the human can answer>

Rules: no file edits, no rewritten plan, no praise, max 10 findings. A speculative finding is
MINOR. If you would approve it as-is: VERDICT: CLEAN and an empty FINDINGS list.
```

## Code

There is no template of our own here; the chosen review skill brings it.

- **`requesting-code-review`** ships its own reviewer template. Use it as-is, with `BASE_SHA`,
  `HEAD_SHA`, a description of the change, and the spec as the requirement source. The plan is
  supporting context only and cannot override the spec.
- **`ponytail-review`** is a procedure, not a template. Its brief is:

```
Load the ponytail-review skill and apply it to <BASE_SHA>..<HEAD_SHA>, with the spec as the
requirement source and the plan as supporting context. Report in the skill's own format.
```

- **both** — conformance first, then quality over the same range. A clean quality report does not
  excuse an Important from the conformance pass.

## Resend (round 2 onward)

Append the reviewer's **own** previous findings — never another reviewer's. A finding the
controller ruled away carries that ruling on its line, or the reviewer re-raises an unchanged
finding every round:

```
Your findings from round N-1:
- F1 <one line>
- F2 <one line> — Ruling: <the decision and why, where the controller ruled it away>
For each, answer: "F1: ADDRESSED | NOT ADDRESSED — <why>". A finding with a Ruling is ADDRESSED
when the ruling holds; answer NOT ADDRESSED when it does not. Then list only new findings.
Reply with the clean verdict of the brief in use, only if all of them are ADDRESSED and you have
nothing new.
```
