# Notes for agents working on bubblequiz

bubblequiz is an R package for paper multiple-choice quizzes. It prints shuffled, QR-coded forms from `exam.yml` + `questions.md`, marks scanned answer sheets with local image processing, and writes a Moodle grade import. [README.md](README.md) is the user guide; read it before changing behaviour.

## Layout

- `R/`: the package. Entry points users call are in the README's quick start (`init_course()`, `check_config()`, `generate_versions()`, `make_quiz_forms()`, `combine_quiz_forms()`, `mark_quiz()`).
- `inst/templates/`: the LaTeX preamble the forms are built from. Marking depends on the exact geometry of the printed page, so a change here can break reading of scans.
- `tests/testthat/`: testthat suite, including synthetic scan fixtures. CI runs R CMD check (`.github/workflows/R-CMD-check.yaml`).
- `example/six-question-quiz/`: a worked example whose outputs are committed.

System tools are needed at run time and in tests: XeLaTeX, pandoc, poppler, zbar, ImageMagick (see the README's installation table).

## Rules that are easy to break

- **Never guess a mark.** Anything not read cleanly goes to the review file for a person to decide. Don't add heuristics that resolve ambiguous marks silently.
- **Stay offline.** Marking must not send scans, zIDs or answers to any outside service.
- **No real student data in the repo.** This repo is public. Test fixtures are synthetic (an earlier real scan fixture was replaced with synthetic sheets); keep it that way. `output/` and `responses/` are gitignored.

## Cross-repo context (maintainers)

bubblequiz is a shared resource in UNSW's BEES quantitative teaching family (moved from `wcornwell/bubblequiz` to `nicercode` on 2026-11-04; the old URL redirects). Family conventions, such as labels, `[prefix]` issue titles, the project board and commit messages, live in the hub, `nicercode/UNSW_BEES_quantitative_curriculum`. The hub is **private**: it needs nicercode access, and its content must not be copied into this public repo. Each course keeps its own `exam.yml`, questions, scans and outputs in its own (private) course repo, not here.
