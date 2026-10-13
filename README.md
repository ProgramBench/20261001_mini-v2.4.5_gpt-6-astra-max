# GPT 6 Astra (max) — ProgramBench submission

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

## Extra stats

Cost, model-call, and output-token statistics are generated from the published trajectories by
the scripts under `_scripts/`. Cost is mini-SWE-agent's per-task `instance_cost`, priced per
million tokens at $10 uncached input, $1 cached input, and $50 output for prompts up to 272K
tokens, and $20 / $2 / $75 above that.

## Redactions

Harness metadata naming internal hosts and users is replaced with `[REDACTED]` in each
trajectory's `info.config` (`safety_identifier`, `output_path`). Model and tool turns are
unmodified.

Each test is run up to 3× (pytest-rerunfailures); only the final attempt is scored (flakiness defense).
