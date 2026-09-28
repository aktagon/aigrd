# Changelog

All notable changes to aigrd are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions
follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.3.0] - 2026-09-28

**Upgrade note.** `aigrd agreement` now counts runs, not labels. A script
that divided `agreements` by `labels` must divide by `samples`.

### Added

- `aigrd judge --repeat N` judges each file N times and passes it only when
  every run passes. It reports how many runs failed each criterion, for
  example `C-008 failed 2 of 3`. A run that returns no verdict counts as a
  failed run. With N above 1 the verdict cache is neither read nor written,
  and each call costs a full judge call. `--format json` gains a `repeat`
  key, and each run-log line of a repeat carries `repeat: {run, of}`.
  Without the flag, or with `--repeat 1`, output is unchanged.
- `--repeat` takes 1 to 10. A value of 0, above 10, or a non-number exits
  `2`. So does `--repeat` above 1 with `--harness`, and `--repeat` with any
  command other than `judge`.

### Changed

- `aigrd agreement` counts every judge verdict for a label's text as a
  sample, from a `--repeat` batch or a separate run. It used to read only
  the most recent verdict. Cache hits are not samples. Text rows now read
  `agreed in 7 of 9 runs`.
- In `aigrd agreement --format json`, `agreements`,
  `judge_fails_owner_passes` and `judge_passes_owner_fails` now count
  samples, not labels. `labels` and `unmatched` still count labels. Two keys
  are new: `samples` and `unmatched_samples`. A script that divided
  `agreements` by `labels` must divide by `samples` instead.
- After a criterion's statement is edited, verdicts judged against the old
  statement still count for its labels. Verdicts on the new statement are
  unmatched samples.

## [0.2.0] - 2026-09-09

**Upgrade note.** aigrd can now fail where 0.1.0 passed. A misconfigured
judge exits `1` instead of warning. Fix the judge, or set
`on_unavailable = "warn"` under `[judge]` to keep the old behaviour.

### Changed

- A judge that could not run no longer reports a pass. A `claude` that ran
  and returned no usable verdict is now `judge.failed`, an error. It exits
  `1` and blocks the Claude Code turn. Version 0.1.0 warned and exited `0`,
  so a broken judge passed every file it was meant to check.
- `judge.failed` covers a crash, a timeout, spending past
  `max_budget_usd`, an `is_error` result, and a verdict outside the
  schema. A file aigrd cannot read joins it.
- `judge.unavailable` now means one thing: `claude` is not on `PATH`. It
  stays a warning and never blocks.
- A `judge.failed` file takes an attempt, so `[judge].max_attempts` bounds
  it. Three stops name the cause, then `judge.gave-up` releases the turn.
  A broken judge cannot trap the agent.

### Added

- `[judge].on_unavailable`, either `"error"` (the default) or `"warn"`.
  `"warn"` restores the 0.1.0 soft skip. Any other value is `cfg.invalid`
  and exits `2`.

### Fixed

- `aigrd init` and `aigrd docs` gave no range for `max_thinking_tokens`.
  The `claude` command-line tool accepts every value and raises a cap below
  1024 to 1024. Extended thinking is billed as output and spends against
  `max_budget_usd`, so a higher cap can exhaust the budget and lose the
  verdict. Both documents now say so.

## [0.1.0] - 2026-09-07

### Added

- `aigrd judge`: one pass or fail per rubric criterion, with a quoted span
  as evidence and a fix note, from a second model run through the `claude`
  command-line tool.
- `--harness claude`: the Claude Code Stop-hook decision, with bounded
  attempts per session and file and a visible give-up.
- `aigrd init`, `rubric derive`, `rubric check`, `rules` and `docs`.
- `aigrd hooks install`, the `40-aigrd` pre-commit fragment, and
  `hooks claude`.
- The run log `.aigrd/runs.jsonl` and `aigrd runs`; `aigrd label`, which
  writes portable evaluation cases beside the rubric; `aigrd agreement`.
- A verdict cache keyed on the file content, the rubric bytes and the
  judge command.
- `[judge].max_thinking_tokens`, a cap on extended thinking in the judge.
- Three example projects: an article, a landing page and an investment
  thesis, each with a rubric and a passing and a failing sample.
