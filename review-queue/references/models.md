# Reviewer models

The reviewers, **in order of preference**. The order *is* the roster: one reviewer means the first
row, a pair the first two, "all" the whole list. Naming a model skips the order.

| # | Name | Model ID |
|---|---|---|
| 1 | Beam Free (max) | `opencode-go/beam-free#max` |
| 2 | MiniMax M3.1 Flash Free (max) | `opencode/minimax-m3.1-flash-free#max` |
| 3 | GLM 5.3 Flash Free (max) | `opencode/glm-5.3-flash-free#max` |
| 4 | ERNIE-Light-DT-1.0 (max) | `openrouter/baidu/ernie-light-dt-1.0-intl#max` |
| 5 | Hy4 flash Free (high) | `opencode/hy4-flash-free#high` |

Each row runs at the variant its ID names — `max` where the model offers one, `high` for row 5,
which has no `max`. Do not drop the suffix.

All but row 4 are served by `opencode` and `opencode-go`; row 4 comes from `openrouter`, so a run
that reaches it needs that provider configured. A provider without credit fails like a capped
model — law 3 stops the run and the human chooses.

**Changing the roster is an edit to this table only** — no other file names a model. Keep the order
meaningful: the first row is what a one-reviewer run gets, so the strongest general reader goes
first.

**Before the first dispatch**: resolve each ID with the models tool. The catalogue is a starting
point, not the roster — free models rotate, do not share a provider, and get capped.

**A model missing from the catalogue can still be usable** — try it once. Only a failed call, or a
model that dies mid-run, counts as gone. Then the run stops and the human picks the replacement
(`review-queue` law 3). Never substitute one on your own.

**A quota that runs dry is not a bug**: the free lines cap first, and that is what law 3 is for.
