# GPT-6 Astra (max) — ProgramBench submission

- **Model:** `gpt-6-astra`, reasoning effort **max**, max 128,000 output tokens per call
- **Scaffold:** mini-SWE-agent v2.4.5 (single-agent), open source; step limit 1,000, no cost
  limit, 6 h wall-time limit
- **Protocol:** no-internet + `task_cleanroom_v6` images. The agent sees only the compiled binary
  (source removed, `.git` reset), reimplements it from scratch by interacting with it, and never
  has network access (`--network none` at the container level).
- **Score:** ~0.869 (mean per-instance test pass-rate over the fixed 200; the leaderboard
  recomputes authoritatively from `_stats/score.json`). Only tests listed in the benchmark's
  `tests.json` are scored; tests the model wrote into its own workspace are not counted.
- **Inference dates:** 2026-10-01 to 2026-10-06

## Task accounting

All 200 tasks are evaluated and included; every rollout exited `Submitted`.

- `tinycc__tinycc`: earlier attempts ended on provider errors; the included rollout is a fresh
  re-run (2026-10-06).
- `bootandy__dust`, `zevv__duc`: the original solution archives were truncated. The solutions
  were rebuilt by re-running the model's recorded commands, in order and without model calls, in
  a fresh task container. The trajectories are the original ones. The code is what those
  commands produce, not the original bytes.
- `filosottile__age`: the standard evaluator fails on this task when a submission leaves Go or C
  sources in the workspace, because the hidden test branches build reference tools in place.
  It was re-evaluated with a fix that removes the submission's Go/C sources after compilation
  and before the test branches run; the compiled executable is unchanged.

## Extra stats

Model-call and output-token statistics are generated from the published trajectories by the
scripts under `_scripts/`.

## Redactions

Harness metadata naming internal hosts and users is replaced with `[REDACTED]` in each
trajectory's `info.config` (`safety_identifier`, `output_path`). Model and tool turns are
unmodified.

Each test is run up to 3× (pytest-rerunfailures); only the final attempt is scored (flakiness defense).
