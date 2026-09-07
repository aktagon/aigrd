# aigrd

aigrd is a quality gate for agent output, judged by a second model. It
scores each file against a rubric written from the project's best work.
Each criterion gets a pass or fail, a quoted span as evidence, and a fix
note.

Below the bar, the Claude Code Stop hook blocks the turn, and the agent
redoes the work. aigrd judges once per call. The harness owns the loop.

aigrd is the agent-output member of the `*grd` family of single-binary
guards. Each guards a different source of truth with the same command
shape, config format, exit codes and JSON output.

| Tool        | Guards                                                                        |
| ----------- | ----------------------------------------------------------------------------- |
| `ctxgrd`    | Markdown records: architecture decisions (ADRs), product requirements (PRDs), specs, handoffs |
| `trtlgrd`   | Resource Description Framework (RDF) graphs in Turtle                         |
| `wrkgrd`    | code: build, tests, staged secrets, commit messages                           |
| `mlgrd`     | machine-learning artifacts                                                    |
| **`aigrd`** | **prose and code only a reader can judge**                                    |

## How it works

1. You write a rubric: five to ten criteria, each one decidable claim about
   the file alone, marked `must` or `should`. `aigrd rubric derive` drafts
   one from your best pieces, and you edit it by hand.
2. `aigrd judge` sends each matching file with its rubric to the judge. The
   judge is `claude -p`, so it uses your existing login. It returns one pass
   or fail per criterion with evidence and a fix.
3. A failed `must` is an error. Failed `should`s are errors only when their
   pass ratio falls below the genre's threshold.
4. In harness mode the errors become a Stop-hook block, with the failed
   criteria as the reason. aigrd counts attempts per session and file. After
   `max_attempts` it gives up visibly, so the loop cannot run forever.

Verdicts are cached on the file content, the rubric bytes and the judge
command, so an unchanged file costs nothing. A check a regex could decide,
such as a word count, belongs to ctxgrd or wrkgrd. `aigrd rubric check`
warns when a criterion reads that way.

## Install

Download a pre-built binary from the
[releases page](https://github.com/aktagon/aigrd/releases/latest): Linux
x86_64, macOS Intel and macOS Apple Silicon. You also need the `claude`
command-line tool, logged in.

Asset names embed the version, so set it once and copy the rest.

```sh
VERSION=v0.1.0
TARGET=aarch64-apple-darwin         # macOS Apple Silicon
# TARGET=x86_64-apple-darwin        # macOS Intel
# TARGET=x86_64-unknown-linux-gnu   # Linux x86_64

BASE="https://github.com/aktagon/aigrd/releases/download/$VERSION"
curl -fsSLO "$BASE/aigrd-$VERSION-$TARGET.tar.gz"
curl -fsSLO "$BASE/checksums.txt"
```

Verify before you extract. `checksums.txt` covers every platform and you
downloaded one, so `--ignore-missing` is required. Expect one `OK` line.

```sh
shasum -a 256 -c checksums.txt --ignore-missing    # macOS
sha256sum   -c checksums.txt --ignore-missing      # Linux
```

Then install:

```sh
tar -xzf "aigrd-$VERSION-$TARGET.tar.gz"
mkdir -p ~/.local/bin
mv "aigrd-$VERSION-$TARGET/aigrd" ~/.local/bin/aigrd
aigrd --version
```

The archive is a directory that also carries `LICENSE`, `CHANGELOG.md`
and this README. Make sure `~/.local/bin` is on your `PATH`.

## Quick start

    aigrd init                     # writes aigrd.toml, docs/rubrics/ and the .gitignore line
    aigrd rubric derive --genre POST content/posts/a.md content/posts/b.md
    aigrd rubric check             # 0 warnings before you trust it
    aigrd judge                    # every file that matches a genre
    aigrd hooks claude             # prints the Stop-hook entry to paste

A genre in `aigrd.toml` maps paths to a rubric:

    [judge]
    model = "haiku"            # a CLI alias or model id, passed to claude --model
    max_thinking_tokens = 0    # extended thinking in the judge; 0 is off

    [POST]
    paths = ["content/posts/**/*.md"]
    rubric = "docs/rubrics/001-post.md"
    threshold = 0.75           # minimum should-pass ratio

`aigrd docs` prints the full guide: every option, command and diagnostic
code, and the measured cost per call.

## The Claude Code Stop hook

`aigrd judge --harness claude` reads the Stop payload on stdin and judges
the files git reports as modified or untracked. When a file has errors it
prints a block decision. The reason lists each failed criterion, its
evidence, its fix and the attempt count. On a pass it prints nothing.

`aigrd hooks claude` prints the `settings.json` entry with an explicit
timeout and reports whether it is wired. It never writes settings.

Keep the per-file time under the hook timeout. On `haiku` with extended
thinking off, a 1,500-word article against a twelve-criterion rubric took
26 seconds and cost $0.027.

## The pre-commit hook

`aigrd hooks install` writes the tracked fragment
`.githooks/pre-commit.d/40-aigrd`, which judges the staged files that match
a genre. It sits beside the fragments the sibling tools install and fails
closed: with the hook active and no `aigrd` on the path, the commit aborts.
A clone activates the tracked hooks with one setting:

    git config core.hooksPath .githooks

## Rubrics

A rubric is a Markdown record under `docs/rubrics/`:

    ---
    id: RUBRIC-001
    title: Blog post
    genre: POST
    ---
    ## Criteria
    - **C-001** (must): Opens with the claim, not a preamble.
      - hint: the first paragraph
    - **C-002** (should): Every number names its source.

The judge sees the file and the criteria and nothing else. So each
statement says what must be present and what is exempt, and the hint says
where to look.

Three worked rubrics with a passing and a failing sample live under
[examples/](examples/): an article, a landing page and an investment
thesis. Copy the one nearest your genre and edit it against your own best
pieces.

## Measuring the judge

Every judged file appends one line to `.aigrd/runs.jsonl`: model, thinking
cap, source, cost, duration and verdict. `aigrd runs` summarises the log.

`aigrd label FILE C-001 pass` records your own verdict as a portable
evaluation case beside the rubric. `aigrd agreement` compares your labels
with the judge's verdicts, overall, per criterion and per model. Ten labels
on the verdicts you overruled turn "the judge seems harsh" into a number.

## Exit codes

`0` clean, `1` at least one error diagnostic, `2` config error or misuse.
Harness mode always exits `0`; the block rides the stdout object.
`--format json` prints one `{exit_code, diagnostics, summary}` object.

## Changelog and licence

Releases are listed in [CHANGELOG.md](CHANGELOG.md). The licence is in
[LICENSE](LICENSE).
