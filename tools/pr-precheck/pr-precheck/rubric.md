# Rubric: is this pull request ready to submit?

<!--
THIS IS THE PART YOU WRITE (fourth week running; this is the rubric's
final form in the sandbox). Your frame in SKILL.md executes whatever
checks you define here, via your procedure.md. It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the diff read against the plan's scope, the test
     evidence read against the plan's test plan, the description read
     against the diff, the repo-facts block's template asks) or a
     location from your references/evidence-guide.md. "The PR" is not
     a source; "the diff's changed files read against the plan's
     stated boundary" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself
     (does the diff fall inside the plan plus its deviation notes? is
     the claimed evidence observable?), never the write-up's shape
     (how long the description is, how many commits there are).
     Structure-shaped checks are what make graders disagree with
     themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (submit) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. State the `unclear`
   treatment explicitly: the frame here is YOUR SKILL.md, so a rubric
   that stays silent is only covered if your frame's grading
   discipline says what happens (the contract's own default is that
   an unverifiable claim fails).

Cover what actually gets bad PRs submitted. The failure families the
lecture named ARE the harness's scoring categories, same names as the
eval README: silent drift (the diff silently does more or less than
the posted plan, or the description claims fidelity the diff
contradicts), not tested (the evidence proves nothing observable, or
the repo's own checks were never run), unreviewable (debris or
unrelated hunks bury the change), and standards wall (the repo's
stated template and disclosure asks are ignored). Your evidence
guide's four headings map onto these one to one (plan fidelity =
silent drift, test evidence = not tested, diff quality =
unreviewable, standards and comms = standards wall), and the category
floor is scored on exactly these names plus clear accept. A rubric
that ignores a category will fail the eval packages built around
that category. And remember the honest-outcome
rule, fourth week running: a PR that honestly discloses a shortfall
can be ready; a rubric that equates "less than everything" with
"hold" fails the set.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| plan-to-diff-fidelity | The plan-context block's in-scope and out-of-scope statements plus deviation notes, read against the candidate PR's changed files, unified diff, and description claims about what the change does. | Pass if every behavior-changing diff hunk is inside the plan's stated boundary or is explained by an explicit, evidence-based deviation, and the PR description makes no claim that the diff contradicts or fails to deliver. A scoped plan may intentionally leave related problems unfixed when that boundary is stated honestly. | required |
| observable-validation | The plan-context block's validation plan and prior reproduction steps, read against the PR test-evidence section's commands, inputs, outputs, and claimed result. | Pass if the evidence shows an executed command or real code path using relevant conditions and an observable result that distinguishes the intended post-change behavior from the pre-change failure. A claim that tests passed without command/output or a result tied to the intended behavior does not pass. | required |
| applicable-repo-checks | The repository facts block's required or recommended checks, read against the PR test-evidence section and any stated reason a check was not run. | Pass if each repository check established as required for this change has visible outcome evidence, or the PR honestly explains why a check could not be run and does not claim that it passed. Do not fail for a check that is not identified as applicable in the available repository facts. | required |
| reviewable-diff | The candidate PR's unified diff and commit list, read against the plan-context scope and test evidence. | Pass if the intended implementation is identifiable in the diff and no unrelated behavior changes, generated files, credentials, debug output, dead code, commented-out alternatives, formatting-only churn, or unrelated commits materially obscure review. A small necessary test, fixture, or configuration edit tied to the plan is in scope. | required |
| standards-and-comms | The PR title and description, read against the repository facts block's PR-template sections, contribution instructions, issue-thread directions, and disclosure requirements. | Pass if the PR provides the concrete information required by an applicable repository instruction, follows explicit maintainer direction, and honestly states scope, validation, and limitations. Do not fail solely because an optional heading is absent or a convention/disclosure requirement is not established in the available evidence. | required |
| reviewer-orientation | The PR title, description, diff, plan context, and test evidence. | Pass if a reviewer can identify the user-visible or system behavior changed, the intentionally excluded work, and the evidence that supports the change without reconstructing the whole issue history. This check is feedback only. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict and are reported as feedback only. An honestly disclosed limitation, skipped check, or scope deviation is not automatically a failure; it fails only when the available evidence shows that the limitation prevents the PR from supporting a required claim, violates an applicable repository requirement, or leaves a material behavior-changing diff outside the plan without an explained deviation.
