# Phase 8 — Ship

1. **Local gate** — `verification-before-completion`'s rule. Run the commands the repo declares — `AGENTS.md` and `.github/workflows/*.yml`
   — before pushing, and paste the output. CI confirms; it is not where you discover a broken
   build. Red only in tests that were already failing at baseline: report it as the baseline was,
   and treat a CI run red for that same reason as a stop.
2. **Docs in the same commit, only when they changed.** Update `AGENTS.md` only when repository
   operating rules or the code map changed; update `README.md` only when user-facing behavior or
   setup changed; never edit the approved spec during shipping. Always add `Status: done` to the
   plan and include any required release notes.
3. **The final stop, then integrate and publish.** If STOPS stops here — `stop at each step` or
   `stop before shipping` — show what shipped: the tasks, the commits, the green test run, the
   rounds per reviewer. This is also PUBLISH's `wait for me` gate: "continue" here authorizes the
   merge and the push together, so do not ask a second time. In a mode with no final stop, the
   `wait for me` gate is itself a pause before any push.
   Then integrate, as far as PUBLISH allows: END=leave has nothing to integrate, and END=PR is a
   remote action, so `wait for me` stops before the push and hands you the branch. END=merge moves
   to the primary checkout (`session_move`), pulls the base branch, merges there, then re-runs the
   local gate on the merged result before anything goes remote — the branch was green, the merged
   tree is a different thing.
   Then publish, exactly as PUBLISH says: `push and tag` pushes the branch — the tag is created and
   pushed in step 4, once the run is green, so a tag never sits on a commit CI rejected; `push only`
   pushes the branch; `wait for me` leaves the remote untouched and asks when there was no final
   stop — with one, it was already authorized.
   With END=PR a push does not create the PR: after it, open the pull request with the forge's CLI,
   or print its creation URL when there is no CLI. Never force-push, never push a branch nobody
   named, and never create a tag unless PUBLISH named it — a tag publishes a release.
4. **CI green when you pushed.** The rule follows the push, not the END. If something was pushed,
   not done until that run ends green; watch it with the CLI when it exists. If nothing was pushed,
   the local gate is the whole verification and the report says so. No CLI and no way to poll: every
   mode hands the URL over and asks, except autopilot, which stops there and reports it. Red: fix
   and push again, two attempts. Once the run is green and PUBLISH is `push and tag`, create the tag
   from the repo's convention and push it — the release is published only now. That tag push starts
   its own run: watch it too, and a red one is a stop — report it and name the tag rather than
   deleting or force-pushing it without the human's word.
5. **Clean up.** `finishing-a-development-branch`'s checks are already met: the local gate ran in
   step 1, the merged result was re-tested in step 3, END answered its menu. Skip its cleanup step
   outright — it decides ownership by path, not by creator, and could remove the pipeline's own
   worktree. The pipeline removes that one instead: on the worktree path, move the session to the
   primary checkout first
   (`session_move`), then `worktree_remove`. On the decline path the work is already in the primary
   checkout and there is nothing to move or remove. If the removal refuses because the worktree still holds
   uncommitted files, stop and show them: move or delete them on the human's word, never force —
   and after a merge do not commit them onto the branch, because that commit is not in the mainline
   and the branch cleanup below would refuse it. After a merge, delete the merged branch as well
   (`git branch -d <branch>`). Keep the worktree when END=leave, and keep it for END=PR — the PR is
   where feedback lands, and the worktree is where you address it. Keep its name in the report.

Report: what shipped, tasks and commits, rounds per reviewer, every ruling, every model substitution
(who was asked for, who ran, and why), the CI conclusion, and the list autopilot would have asked
about.

---

The pipeline ends here. If the run stopped for an escalation, the report carries the rounds and
what is still open — `review-queue`'s **Stop and escalate** says what that report contains.
