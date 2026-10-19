---
name: code-review
description: >
  FlyDSL pull request review for Copilot code review on GitHub. Applies the review-pr skill in
  .claude/skills/review-pr, with rules derived per diff by its triage.py, to catch
  silent-correctness bugs, JIT cache-key mistakes, architecture-capability holes, index-width
  overflow, unbounded buffer descriptors, layout-algebra errors, dead flags, and tests or
  benchmarks that cannot fail.
---

# FlyDSL code review on GitHub

This review runs the `review-pr` skill. Read `.claude/skills/review-pr/SKILL.md` to its last line;
one view shows only part of it. That file is the standard; this one changes only what a Copilot
code review cannot do as written there, and wins where the two disagree.

## Evidence

The setup steps in `.github/workflows/copilot-code-review.yml` run `review-pr`'s evidence step
before this review starts, and leave its files in `.review/` at the repository root. Read
`.review/STATUS` first. If it is missing or does not read `evidence exit=0`, the evidence did not
build: review from the diff alone, and post a 📝 comment on the first changed line saying so.

| file | use |
|---|---|
| `.review/rules_expanded.txt` | the only rules to work (Steps 3 and 5) |
| `.review/scan.txt`, `.review/bounds.txt` | candidate sites to clear or fire (Step 5) |
| `.review/core_expected.txt` | the backbone files and their tiers (Step 4) |
| `.review/ci_coverage.txt`, `.review/bench_compare.txt` | where added tests run; changed kernels no benchmark reaches |
| `.review/merge-target/` | the base-branch versions of the changed files |

Read CI checks and human review comments through the GitHub MCP server. If the
`test (linux-flydsl-*)` job logs are not readable there, the Perf line is `NOT MEASURED`.

## Steps

This review does not run commands. Work Steps 2 to 8.6 as written, keeping their answers in your
reasoning rather than in files; the gates of Step 9 and the independent refutation of Step 8.7 do
not run here.

## Output

Unlike a `review-pr` card, this review is posted. Post the findings as review comments on their
`file:line`, keeping the three parts, severity and `[verified]` / `[inferred]` tag. No rule ids.
