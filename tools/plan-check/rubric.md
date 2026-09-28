# Rubric: is this plan ready to post and build from?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here (via your procedure.md). It ships empty on
purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the plan's scope statement, the test plan read against
     the repro evidence's steps, the plan comment read against the
     thread highlights, the repo-facts block) or a location from your
     references/evidence-guide.md. "The plan" is not a source; "the
     plan's stated cause read against what the repro evidence shows"
     is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (is
     this one bounded change? could a stranger start executing it?),
     never the write-up's shape (how many sections it has, how long it
     is, whether it uses headings). Structure-shaped checks are what
     make graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad plans posted. The lecture named the
failure families: the diagnosis ignores or contradicts the reproduced
evidence, the change is unbounded (scope creep), the plan targets the
symptom while the evidence points at the cause, a stranger could not
start executing it, the test plan proves nothing observable, the
unknowns are dressed up as certainty, and the comment ignores what the
thread or the repo's stated conventions ask. A rubric that ignores a
family will fail eval packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| evidence-grounded-diagnosis | The plan's stated problem mechanism or cause, read against the issue context and the repro-evidence block's environment, trigger, observed behavior, and artifact. | Pass if the plan explains a mechanism that accounts for the behavior the reproduction actually shows, or explicitly limits itself to an observed data-flow failure without claiming an unproven root cause. Fail if it contradicts the reproduced behavior, ignores a material condition in the evidence, or treats an inference as a confirmed cause. | required |
| bounded-change | The plan's scope statement, named code areas, planned changes, and not-in-scope boundary, read against the issue context and repo-facts block. | Pass if the proposed work is one coherent change that addresses the evidenced problem through the smallest relevant code path, with unrelated cleanup, redesign, feature work, and speculative refactors excluded. Fail if the plan adds independent goals or expands into an unbounded rewrite without evidence that the issue requires it. | required |
| executable-path | The plan's implementation steps, named files or code areas, repository facts, and stated order of work. | Pass if a stranger can identify where to begin, what behavior or data flow to change, the order in which to make the change, and what must remain unchanged without asking the author to supply a behavior-changing missing decision. Exact line numbers and final code are not required. | required |
| decisive-validation | The test plan, read against the repro-evidence block's trigger, expected/actual behavior, and artifact, plus applicable repository test conventions in the repo-facts block. | Pass if validation reruns the original reproduction path or the closest real code path and names an observable before/after result that would distinguish the proposed change working from it not working. Any additional automated test is relevant only if it exercises that behavior; passing unrelated tests alone does not pass. | required |
| uncertainty-and-deviations | The plan's statements about assumptions, risks, open questions, and deviations, read against uncertainties or constraints visible in the issue context, repro-evidence block, and repo-facts block. | Pass if the plan does not present an unestablished cause, behavior, repository convention, or implementation detail as certain. When the available evidence leaves a material question that would change the proposed implementation or validation, the plan identifies it as an assumption, question, or verification step. Treat a stated deviation record as sufficient when present, but do not fail a pre-build plan solely because it has no deviation to report or does not list speculative risks. | required |
| thread-aware-comms | The plan comment, read against the issue thread/highlights and the repository's stated templates, contribution instructions, and disclosure requirements identified in the repo-facts block or evidence guide. | Pass if the comment identifies the issue, states the bounded intended change and validation approach in the author's own words, and follows an applicable instruction that the available evidence establishes. Do not fail solely for omitting optional headings, repeating no unshown convention, or lacking a disclosure when no applicable requirement is identified. | required |
| maintenance-context | The plan's stated compatibility, error-handling, or test considerations, read against repository conventions and relevant code-path facts. | Pass if it identifies a concrete maintenance consideration that applies to the planned change, or if the evidence supports that no additional consideration is relevant. This check provides feedback only. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict and are reported as feedback only. When a package is a plan-only draft, grade only evidence that is available; do not require completed implementation evidence, but reject a plan that lacks the evidence needed to justify its diagnosis, scope, or validation approach.