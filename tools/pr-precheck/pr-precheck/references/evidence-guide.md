# Evidence guide: where evidence lives in a PR package

<!--
THIS IS THE PART YOU WRITE (third week running: the map stays in your
hands). Your tool uses this guide as its map: for every kind of
evidence a rubric check names, this file says WHERE to find it in a PR
package and WHAT GOOD LOOKS LIKE when you do.

The four families below are the harness's failure categories under
the names the eval README uses: plan fidelity = silent-drift, test
evidence = not-tested, diff quality = unreviewable, standards and
comms = standards-wall. A package that fails none of them is a
clear-accept. Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the plan-context block's scope pair and test
  plan, the candidate PR's diff, commits, description, or
  test-evidence section, the repo-facts block's template asks and
  stated policy). In live mode (where in your working copy and on
  GitHub: your plan.md and its deviation notes, your branch's diff,
  your draft title and description, your captured test output, the
  repo's PR template and CONTRIBUTING.md).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("every changed file falls inside the
  plan's stated boundary or a deviation note") over adjectives ("the
  diff is clean").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts three ways: your
procedure says WHEN to gather each family, this guide says WHERE, and
your SKILL.md says the tool reads both. Write the map you wish your
executor had.
-->

## Plan fidelity (harness category: silent-drift)

**Where it lives:** In an eval bundle, read the plan-context block's goal, in-scope/out-of-scope boundary, planned changes, validation intent, and deviation notes; then compare them with the candidate PR's changed-file list, unified diff, commit list, and claims in the title/description. In live mode, read `beat-1-sandbox/unit-3/plan.md`, the feature branch diff against its base branch, the draft PR title/body, and any issue-thread update that records a material change of intent.

**What good looks like:** Every behavior-changing edit delivers the planned bounded change or is tied to an explicit deviation that says what changed and why. The description claims exactly what the diff delivers; adding a necessary focused test or fixture is part of the planned change, while an unrelated cleanup, refactor, feature, or unmentioned removed work is silent drift.

## Test evidence (harness category: not-tested)

**Where it lives:** In an eval bundle, compare the plan-context test plan and prior repro evidence with the candidate PR's test-evidence section, including commands, inputs, output excerpts, test names, and results; read repository-required checks from the repo-facts block. In live mode, read the Unit 2 reproduction report, Unit 3 before/after evidence, the captured terminal output or CI results, relevant test files, and the draft PR validation section.

**What good looks like:** Evidence identifies what was run, under what relevant input or condition, and what observable result changed or passed. A decisive result ties the exact behavior from the prior repro to its expected post-change result; generic “tests pass” language is not evidence. Every applicable repository-required check has a visible result, or a truthful limitation explaining why it could not run without claiming success.

## Diff quality (harness category: unreviewable)

**Where it lives:** In an eval bundle, inspect the candidate PR's unified diff, changed files, and commits against the plan-context scope. In live mode, inspect `git diff <base>...HEAD`, `git diff --check`, `git status`, and the feature branch's commits.

**What good looks like:** The intended change is easy to trace from added/changed code to its behavior, and each changed file belongs to the planned fix or focused verification. The diff contains no secrets, generated artifacts, personal working files, debug prints, dead/commented-out code, broad formatting churn, or unrelated drive-by edits that obscure the review.

## Standards and comms (harness category: standards-wall)

**Where it lives:** In an eval bundle, compare the candidate PR title and description with the repo-facts block's stated PR template, contribution guide, policy/disclosure requirements, and issue-thread instructions. In live mode, read the repository's PR template, `CONTRIBUTING.md`, applicable policies, the issue thread, and the draft PR body.

**What good looks like:** The PR follows every repository-specific requirement that the available evidence establishes as applicable, including required sections, test-reporting expectations, linked issues, and AI-use disclosures where required. The description provides real change and validation information rather than boilerplate and acknowledges concrete limitations. Optional headings, invisible conventions, or a disclosure requirement not shown to apply do not create a failure.