# Changelog

All notable changes to aigrd are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions
follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
