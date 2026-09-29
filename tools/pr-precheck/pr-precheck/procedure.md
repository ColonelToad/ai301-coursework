# Procedure: how this tool grades a PR package

<!--
THIS IS THE PART YOU WRITE (second week running for the procedure).
Week 3 you wrote these steps for a plan package; this week the graded
object is a PR package, and the read that matters most is a
side-by-side: the diff against the plan, the evidence against the test
plan, the description against both. Your week-3 procedure is the
pattern; do not paste it unchanged, because its read order was built
for a different object.

Your rotation is the design brief again, and this week friction routes
three ways: a stall on WHAT to decide is a rubric gap, a stall on
WHERE to look is a procedure gap (this file), and a stall on what the
tool even reads or outputs is a frame gap (your SKILL.md). A complete
procedure lets someone who has never seen a PR package before grade
one exactly the way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
plan's scope pair before opening the diff, and list the files the plan
names" is a step; "understand the change" is a wish.
-->

## Read order

1. Read the plan-context block before opening the diff. Record the issue goal, the in-scope and out-of-scope boundaries, the planned implementation areas, validation plan, and any deviation notes. This establishes the baseline against which the final branch must be judged.
2. Read the prior reproduction evidence and the plan's expected post-change observation. Record the trigger, relevant conditions, original failure, and exact behavior the change is intended to alter.
3. Read the repository facts block and any cited template or policy. Record only requirements that are explicitly established as applicable: required checks, PR-body fields, contribution instructions, disclosure requirements, and maintainer directions.
4. Read the candidate PR's changed-file list, unified diff, and commit list. Record each behavior-changing edit, each test/fixture/configuration edit, and any apparent debris or unrelated change.
5. Read the PR title, description, and test-evidence section last. Record every claim about scope, behavior, validation, skipped checks, and limitations. Compare these claims against the previously gathered plan, diff, and evidence rather than treating the description as proof.
6. This order prevents the PR description from redefining the plan, overstating the diff, or turning a generic test claim into evidence before the evaluator has read the relevant code and prior repro.

## Evidence gathering

1. For plan fidelity, gather the plan scope pair, planned source/test locations, and deviation notes. Pair each changed file and behavior-changing hunk with the applicable plan item or explicit deviation; separately record every PR-description claim about what changed.
2. For observable validation, gather the original repro trigger and expected-after state from the prior repro evidence and plan test plan. Gather each test-evidence command, input/fixture, output/result, and claimed conclusion from the PR package. Record whether the evidence demonstrates the intended behavior rather than only an unrelated suite result.
3. For applicable repository checks, gather only checks that the repo-facts block identifies as required or applicable. Record each check's command or condition, whether it was run, its visible outcome, and any stated limitation.
4. For diff quality, gather the changed-file list, unified diff, and commit list. Record debug artifacts, generated files, secrets, personal files, commented-out/dead code, broad formatting changes, unrelated hunks, or commits that make the planned change harder to review.
5. For standards and comms, gather the title/body text, explicit issue-thread directions, and repository requirements that are shown in the repo-facts block. Record whether each applicable requirement has a real corresponding response in the PR.
6. If a required evidence source is absent, record it as absent rather than inferring that a command ran, a test passed, a rule is optional, or a deviation was approved.

## Check execution

1. Grade `plan-to-diff-fidelity` first. Compare each behavior-changing hunk against the plan boundary and deviation notes, then compare every PR-description scope claim against the diff. Pass only if all material changes and claims align; fail silent additions, unexplained omissions that contradict a claim, or description claims the diff does not support.
2. Grade `observable-validation` second. Map the PR evidence back to the original repro trigger and expected post-change behavior. Pass only if an executed command or code path with visible output/result distinguishes the intended behavior from the original failure. Grade `unclear` if commands, inputs, outputs, or their relation to the claimed behavior are absent.
3. Grade `applicable-repo-checks` third. For each requirement established in the repo facts, verify a visible result or an honest statement that it was not run. Pass an honestly disclosed shortfall unless the established requirement makes the omitted check mandatory for submission or the PR falsely claims it passed.
4. Grade `reviewable-diff` fourth. Inspect whether a reviewer can isolate the intended change without being diverted by unrelated material. Fail for material debris, secrets, unrelated behavior changes, or broad churn that obscures review; do not fail for a necessary focused test, fixture, or minimal configuration update.
5. Grade `standards-and-comms` fifth. Enforce only requirements established in the gathered repository/thread evidence. Pass a truthful, specific PR body that satisfies those requirements; fail ignored mandatory template/policy/direction or boilerplate that leaves an explicitly required item unanswered.
6. Grade `reviewer-orientation` last as preferred feedback. It can be graded from the already gathered plan, diff, test-evidence, and PR-description notes without rereading the package.
7. For each check, assign `pass` when the outcome is supported, `fail` when the evidence contradicts its pass condition, or `unclear` when necessary evidence is genuinely missing. Do not let a later check repair an earlier missing evidence source.

## Verdict assembly

1. Include every required and preferred check in the per-check output, with `pass`, `fail`, or `unclear` and a short evidence statement identifying the supporting or missing source.
2. Apply the rubric rule exactly: accept only if every required check passes. Reject if at least one required check fails or is unclear. Preferred feedback never changes the verdict.
3. When more than one required check fails or is unclear, treat the first one in rubric order as the deciding check and quote the smallest decisive evidence conflict or absence for it in the final summary.
4. If a PR is rejected, list all required checks holding it and state the narrowest revision needed for each: align the scope/deviation note, run or show decisive evidence, remove unrelated diff material, or satisfy an established repository requirement.
5. End with a fenced JSON object containing the item identifier, all per-check grades/evidence, and exactly one binary field: `"verdict": "accept"` or `"verdict": "reject"`.