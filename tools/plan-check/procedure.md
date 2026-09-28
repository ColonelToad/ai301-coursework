# Procedure: how this skill grades a plan package

<!--
THIS IS THE PART YOU WRITE, and it is a new kind of part. Weeks 1 and
2, SKILL.md carried a numbered workflow and you only wrote judgment
files. This week the workflow is gone from the frame: SKILL.md says
"execute procedure.md", and these are the operating steps you author.
The machinery is in your hands now.

Your operator swap is the design brief. When your executor stalled
because your rubric said WHAT to decide but not HOW to find the
evidence, that was a procedure gap. This file is where those gaps get
closed: a complete procedure lets someone who has never seen a plan
package before (a groupmate, or the skill itself) grade one exactly the
way you would.

Under each stage heading below, write the concrete steps for that
stage. The one-line note under each heading says what a complete
procedure must decide there. Write steps, not intentions: "read the
repro evidence before the plan, and note what behavior it pins down"
is a step; "understand the context" is a wish.
-->

## Read order

1. Read the issue context and thread highlights first. Record the reported symptom, the stated expected behavior, any maintainer constraints, and any repository-specific instructions that are actually provided.
2. Read the repro-evidence block next, before reading the candidate plan. Record the environment/conditions, trigger, observed result, artifact, and the strongest behavior the evidence proves. Mark any claimed cause that the evidence does not itself establish.
3. Read the repo-facts block and the repository guidance it cites. Record the relevant source locations, data/control-flow facts, test conventions, contribution requirements, and any applicable disclosure rule.
4. Read the candidate plan's goal, scope, implementation steps, validation plan, open questions, deviations section, and plan comment. Compare each statement to the facts recorded in steps 1–3 rather than treating the plan's wording as evidence.
5. This order prevents a plausible plan from redefining the reported problem, inventing a cause, or expanding the issue's scope before the evaluator knows what the reproduction and repository facts support.

## Evidence gathering

1. For diagnosis and grounding, gather the issue's reported behavior and the repro block's actual trigger, environment, output/artifact, and observed result. Gather the plan statement that explains the problem or names the intended cause/boundary.
2. For scope, gather the plan's named files/areas, planned changes, explicit exclusions, and any scope constraint from the issue or repo facts.
3. For executability, gather the plan's implementation sequence, starting file/area, intended behavior or data-flow change, and any repository facts needed to locate or perform the work.
4. For validation, gather the original reproduction steps and artifact, then gather the plan's proposed command/path, relevant input/fixture, expected post-change observation, and applicable test command or convention.
5. For honesty, gather every unresolved assumption, risk, question, and deviation note from the plan; compare them to uncertainties left by the reproduction evidence and repo facts.
6. For communications, gather the plan-comment text, issue/thread directions, and only the repository templates, policy, contribution, or disclosure rules that are explicitly available and applicable.
7. Keep short notes that quote or precisely identify the supporting location for each gathered fact. If a fact cannot be found in the package or identified live source, record it as absent rather than inferring it.

## Check execution

1. Grade `evidence-grounded-diagnosis` first. Compare the plan's mechanism to the reproduced trigger and artifact. Pass a carefully limited boundary-level explanation; fail a contradicted or overconfident cause claim.
2. Grade `bounded-change` second. Compare every planned change with the evidenced issue path. Pass only if the steps form one smallest coherent change; fail independent refactors, features, or redesigns not justified by the evidence.
3. Grade `executable-path` third. Starting from the named first file/area, attempt to describe the next action using only the plan and gathered repository facts. Pass only if no behavior-changing decision must be supplied by the executor.
4. Grade `decisive-validation` fourth. Map the test plan back to the reproduction's trigger and expected/actual observation. Pass only if the proposed after-state would visibly distinguish a working change from the original failure; do not treat a generic test-suite run as decisive by itself.
5. Grade `uncertainty-and-deviations` fifth. Pass only when unsupported details are explicitly labeled as unknown, assumptions, or checks to perform and the plan provides a place to record implementation deviations or a no-deviation result.
6. Grade `thread-aware-comms` sixth. Compare the comment with the gathered thread and repository requirements. Fail only for a missing or conflicting instruction when the available evidence establishes that the instruction applies; do not invent a requirement.
7. Grade `maintenance-context` last as preferred feedback. It may be graded without re-reading the full package after the repository facts, scope, and planned behavior are gathered.
8. When evidence for a required check is genuinely absent, grade that check `unclear`; do not replace missing evidence with a favorable assumption. A clearly contradicted condition is `fail`; a condition supported by the gathered evidence is `pass`.

## Verdict assembly

1. List every check in the output JSON with `pass`, `fail`, or `unclear`, including preferred checks.
2. Apply the rubric verdict rule exactly: accept only if all required checks pass. Reject if any required check fails or is unclear. Preferred checks never alter the verdict.
3. For each failed or unclear required check, quote or precisely identify the decisive missing, conflicting, or unsupported evidence in the short per-check summary.
4. If the verdict is reject, name the required check or checks that held the package and give the smallest revision that would supply the missing evidence, narrow the scope, make the implementation executable, or make validation decisive.
5. End with a fenced JSON block containing the item identifier, per-check grades and evidence, and exactly one binary `verdict` value: `accept` or `reject`.
