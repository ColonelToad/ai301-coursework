# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-match | The repro report's environment record, read against the issue context and the target versions/platforms described in the issue. | Pass if the report identifies the relevant runtime, dependency or app version, operating system/platform, and setup details needed to interpret the result, and they match the issue's stated target or clearly identify a meaningful difference. | required |
| followable-trigger | The repro report's starting state, setup instructions, commands or interactions, and stated expected and actual result. | Pass if a stranger with the stated environment can begin from the described starting state, perform the actions in order, and know what observable result to look for without filling in a behavior-changing missing step. | required |
| issue-matched-proof | The report's artifact or observation—such as output excerpt, log, screenshot, test result, or precise observed behavior—read against the behavior described in the issue. | Pass if the evidence shows the same failure mode or behavior the issue describes, or if it supports a clearly stated cannot-reproduce result after attempting the issue's described trigger; evidence for a different symptom, feature, or error does not pass. | required |
| outcome-honesty | The claim comment and repro report, especially the result statement, alongside the steps, environment record, and artifacts offered as support. | Pass if the conclusion says no more than the available evidence establishes: a successful reproduction claims only the behavior shown, and a failed attempt is presented as cannot reproduce under the stated conditions rather than as proof the issue is invalid. | required |
| repo-aware-comms | The claim comment and repro comment, read against the issue thread and the repository facts, templates, contribution guidance, and disclosure requirements identified in the evidence guide. | Pass if the claim or report communicates a specific, evidence-backed outcome in the author's own words and does not contradict an applicable repository instruction. Treat a repository convention, template, or disclosure requirement as a failure only when the package identifies that requirement and the comment conflicts with it or omits information the requirement expressly requires. Do not fail solely because the package does not restate a convention, use a particular heading, include an unnecessary template field, or provide a disclosure when no applicable rule is shown. | required |
| useful-next-step | The repro report's conclusion and any next-step suggestion, read with the observed result and issue context. | Pass if any proposed next step follows from the evidence and is framed as a suggestion rather than an unsupported diagnosis; a package with no next-step suggestion is not penalized. | preferred |

## Verdict rule

Accept if every required check passes. Reject if any required check fails or is unclear. Preferred checks never change the verdict; they are reported as feedback only. For a claim-only draft, the reproduction-specific checks are reported as not yet applicable rather than failed, and the claim and communication checks determine whether the draft is ready to post.