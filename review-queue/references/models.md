# Reviewer models

The reviewers, **in order of preference**. The order *is* the roster: the first row is preselected
in the menu, the queue runs the selected names in this order (never click order), and a failure in
autopilot substitutes the highest-ranked unused row — scan top to bottom, skipping rows already used
or failed.

| # | Name | Model ID |
|---|---|---|
| 1 | Beam Free (max) | `opencode-go/beam-free#max` |
| 2 | MiniMax M3.1 Flash Free (max) | `opencode/minimax-m3.1-flash-free#max` |
| 3 | GLM 5.3 Flash Free (max) | `opencode/glm-5.3-flash-free#max` |
| 4 | ERNIE 5.5 Flash Preview Free (high) | `openrouter/baidu/ernie-5.5-flash-preview:free#high` |
| 5 | Hy4 flash Free (high) | `opencode/hy4-flash-free#high` |
| 6 | MiMo-V2.6-Flash Free | `opencode/mimo-v2.6-flash-free` |
| 7 | Nemotron 3 Ultra 550B A55B Free (high) | `openrouter/nvidia/nemotron-3-ultra-550b-a55b:free#high` |
| 8 | DeepSeek V4.1 Flash | `opencode-go/deepseek-v4.1-flash` |
| 9 | Big Pickle | `opencode/big-pickle` |

Each row runs at the variant its ID names — `max` where the model offers one, `high` for rows 4, 5
and 7, and no suffix for rows 6, 8 and 9, which use the model's default. Do not drop a suffix that is
named.

**Row 6 is added on the catalogue's word, not a live call.** Both attempts returned `Free usage
limit reached` — the same class as row 3: quota, not a broken ID. Treat it as unproven until a call
answers.

Rows 1–3, 5, 6, 8 and 9 are served by `opencode` and `opencode-go`; rows 4 and 7 come from `openrouter`,
so a run that reaches them needs that provider configured. A provider without credit is just another
failed call — law 3's failure policy decides: `stop` asks the human, `substitute` takes the
highest-ranked unused row.

**Rows 4 and 7 prove the catalogue is a hint, not a verdict.** The models tool lists neither slug and
both answer. The listed ERNIE, `ERNIE-Light-DT-1.0`, does *not* work on this account — 131 072 tokens
requested against about 10 149 afforded, at every variant — which is why the roster names the free
5.5 slug instead. Do not swap either row back.

**Changing the roster is an edit to this table only** — no other file names a model. Keep the order
meaningful: the first row is what a one-reviewer run gets, so the strongest general reader goes
first.

**Before the first dispatch**: resolve each ID with the models tool. The catalogue is a starting
point, not the roster — free models rotate, do not share a provider, and get capped.

**A model missing from the catalogue can still be usable** — try it once. Only a failed call, or a
model that dies mid-run, counts as gone. Then law 3 applies with the caller's failure policy: `stop`
asks the human, `substitute` takes the highest-ranked unused row, scanning top to bottom.

**A quota that runs dry is not a bug**: the free lines cap first, and that is what law 3 is for.
