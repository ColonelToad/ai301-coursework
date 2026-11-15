# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_alive | Read the repo-facts block: `last push to any branch`, `latest release`, `last 5 default-branch commits`, and the maintainer first-response sample. | Pass if the repo-facts block shows either a push to any branch, a default-branch commit, or a release within the last 90 days. Do not require a maintainer comment on this particular issue when recent repository activity exists. | required |
| repo_in_use | Check the repo-facts block for the dates of the last 5 commits on the default branch. | At least 1 of the last 5 default-branch commits is dated within the last 30 days. | required |
| bounded_scope | Read the issue title, body, labels, and comment thread. Evaluate the required outcome, not the amount of explanatory text or the number of examples. | Pass when the issue has one coherent outcome or one localized behavior to implement or repair, including a group of similar cases within the same feature. Do not fail merely because it names several related files, rules, examples, suspected causes, or optional implementation suggestions. Fail only when it explicitly requires unrelated deliverables, a broad redesign, a migration, a new subsystem, or a new feature with no defined completion condition. | required |
| available | Read the issue state, assignees, linked-PR state in the repo-facts block, labels, and all comments in the issue thread. | Pass if the issue is open and unassigned, has no linked open PR that addresses it, and has no explicit current claim or maintainer-confirmed statement that another contributor is actively working on it. A closed linked PR, historical discussion, an unconfirmed expression of interest, or a claim that was later revoked, abandoned, or inactive for more than 90 days does not fail this check. | required |
| enough_context | Read the issue title, body, labels, comment thread, and any named component, file, rule, or behavior. | Pass if the issue identifies a specific affected behavior, component, file, rule, or desired change and gives a contributor an identifiable starting point. A brief issue can pass when it names the missing/broken behavior and the relevant feature area; numbered reproduction steps, logs, screenshots, and a complete implementation plan are not required. Fail only when the issue gives no specific behavior, component, location, or completion target. | required |
| newcomer_signal | Read the issue labels, body, and comment thread. | The issue has a beginner-oriented label such as `good first issue`, `good first bug`, `help wanted`, or an explicit maintainer statement that it is suitable for a new contributor. | preferred |
| contribution_policy | Read the `contribution policy` line in the repo-facts block. | Pass if the policy allows AI-assisted contributions, allows them with conditions such as review, testing, understanding, or disclosure, or has no statement about AI/tooling. Fail if it explicitly says AI-generated code or documentation is not accepted, AI assistance is prohibited, or an equivalent restriction conflicts with the course workflow. | required |

## Verdict rule

Return `accept` only when every required check passes. Return `reject` when
any required check fails or is unclear. Preferred checks never change the
accept/reject verdict; use them only to rank accepted issues, with issues that
pass more preferred checks ranked higher. For every failed or unclear required
check, state the check name and cite the issue or repo evidence that caused
that result.