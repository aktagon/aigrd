# Changelog

All notable changes to aigrd are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions
follow [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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
