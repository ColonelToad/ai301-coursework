---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Determine whether a candidate pull request is ready to submit: its implemented diff must match the bounded plan and any recorded deviations, its evidence must show that the relevant change ran, its title and description must claim no more than the diff and evidence establish, and it must meet applicable repository and thread requirements.

A PR package consists of the issue context; the plan context, including scope, test plan, and deviations; the candidate branch's diff and commits; the draft PR title and description; test evidence; and repository facts such as required checks, templates, contribution instructions, and disclosure rules. Grade the implemented PR package, not the original issue report or an imagined future implementation.

## Inputs and modes

### Live mode

Read all available inputs before grading:

- The issue URL and issue thread, including relevant maintainer directions.
- The student's Unit 3 `plan.md`, including its in-scope/out-of-scope boundary, validation plan, and deviations section.
- The checked-out feature branch and its complete diff against the repository default branch. From the repository working copy, obtain this with `git diff main...HEAD`; if the repository’s actual default branch is not `main`, use that default branch in place of `main`.
- The feature branch commit list, using `git log --oneline <default-branch>..HEAD`.
- The draft PR title and description.
- Captured before/after reproduction evidence, focused test output, and any applicable repository-check output.
- Repository documentation, PR templates, contribution instructions, and policies that apply to the proposed PR.

For a house-chain student, read the linked or supplied house issue, prior plan, reproduction/test evidence, branch diff, draft PR text, and repository facts provided for that chain. Do not assume the student authored upstream evidence; judge only the evidence and claims present in the package.

### Eval mode

Treat the supplied evaluation bundle as the complete world. Do not fetch GitHub pages, inspect a local repository, infer missing facts, or apply external repository conventions. Read only the bundle and the four evaluated tool files. Run every rubric check and apply the full verdict rule to every package.

## The scope seam (live mode only)

Before reading live repository or issue material, read `scope.md`. Follow its repository, issue, chain, and house-work rules exactly. Refuse to grade or approve work outside the allowed repository, issue scope, or contribution path; explain which scope rule prevents grading and what input or correction is needed.

If `scope.md` has an unfilled repository placeholder, do not treat the placeholder as a valid scope. Stop and ask the student to complete or provide the authorized repository scope before grading live work.

Ignore `scope.md` entirely in eval mode. Eval bundles define their own complete scope.

## The voice seam (live mode only)

Before evaluating outgoing PR text, read `voice-guide.md`. Apply it to the draft PR title and description only. Report each broken voice rule by naming the rule and quoting the relevant title or description text; suggest a revision that keeps the same evidence-backed meaning.

Voice-guide results are feedback only. They never change the rubric check grades or the binary accept/reject verdict on their own.

Ignore `voice-guide.md` entirely in eval mode.

## Component reads

Read `rubric.md`, `references/evidence-guide.md`, and `procedure.md` before grading the package.

- Use `rubric.md` as the source of check names, evidence sources, pass conditions, weights, and verdict rule.
- Use `references/evidence-guide.md` to locate the plan-fidelity, test-evidence, diff-quality, and standards/comms evidence families.
- Execute `procedure.md` in the stated order. Use its evidence-gathering and check-execution steps to decide each grade, then use its verdict-assembly steps to produce the result.

If `rubric.md` has no real check rows or no usable verdict rule, refuse to grade and explain that the rubric is incomplete. If `procedure.md` has no concrete read, gather, check, and verdict steps, refuse to grade and explain that the procedure is incomplete.

If the procedure is silent on a step necessary to execute a check, report the procedure gap and grade that required check `unclear`; do not invent a replacement workflow or fill the gap with assumptions.

## Verdict and output

Use only two verdicts:

- `accept` means the PR is ready to submit.
- `reject` means the PR should be held and revised before submission.

Give a short per-check summary before the final output. For a rejected PR, identify the required check or checks holding it and the narrowest evidence-based revision needed.

End the reply with the following valid fenced JSON block. It must be the last content in the reply, with nothing after it.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Grade from evidence first. Treat the diff, commands, outputs, repository facts, and plan boundaries as evidence; do not accept a description’s assertion as proof that code changed or tests ran.

Grade the outcome, not polish or document shape. Do not require a particular number of commits, headings, words, tests, or lines of explanation unless an applicable repository requirement explicitly establishes it.

Apply the rubric’s pass conditions and weights exactly. Use the procedure to decide the read order, evidence gathering, check execution, and verdict assembly. Do not substitute personal preferences, unshown project conventions, or external facts for the package evidence.

Use `pass` only when the required outcome is supported by available evidence. Use `fail` when the evidence contradicts a pass condition. Use `unclear` when required evidence is genuinely missing or the procedure cannot execute the check. If the rubric’s verdict rule does not explicitly state how to treat `unclear`, treat `unclear` on a required check as a failure and return `reject`; preferred checks never alter the binary verdict.

An honestly disclosed limitation, skipped check, or deviation is not automatically a failure. Evaluate whether the disclosure is supported, whether it contradicts a required claim, and whether an applicable repository requirement makes the missing work a submission blocker.