# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue Link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18

**Verdict output**

Live run on three candidates from the Path Review repo, captured 2026-09-19 while the
repo was active and #18 had no open linked PR.

Issue Grading for First Issues

🟢 Issue #18 — BEST FIRST ISSUE
- Title: Repo analyzer never receives a file list, so has_tests and has_ci are always False
- Why it's best: Explicitly labeled "good first issue" + tier-1 + shortest scope (2–4 hrs)
- Clarity: Excellent — exact problem statement and root cause identified
- Scope: Well-bounded — populate missing file_structure key in two places
- Complexity: Low — requires understanding data flow, not complex algorithms
- Prerequisites: Basic Python, understanding of ingestion pipeline
- Supporting docs: Two clear files to modify
- Verdict: ✅ Best pick for a newcomer

---

🟡 Issue #13 — MODERATE FIRST ISSUE
- Title: Add an integration test that runs the full RAG pipeline against a mock LLM
- Why it's viable: Clear scope, good supporting files, no blocking dependencies
- Clarity: Good — explains what's needed (mocking strategy, pytest marks)
- Scope: Focused on one test file + fixtures
- Complexity: Moderate — requires understanding RAG pipeline architecture + mocking patterns
- Prerequisites: Pytest, knowledge of ReviewGenerator, RAG concepts
- Supporting docs: Three relevant files listed
- Verdict: ⚠️ Viable but requires more domain knowledge than #18

---

🔴 Issue #22 — NOT A FIRST ISSUE
- Title: Add a mock GitHub API server for integration tests
- Why it's not ideal: Tier-2, longest scope (5–7 hrs), requires DevOps/CI knowledge
- Clarity: Clear but assumes pytest-httpserver familiarity
- Scope: Largest — new test suite + fixture setup + CI modification
- Complexity: Higher — integrating with pytest-httpserver, modifying CI config
- Prerequisites: Advanced testing setup, GitHub Actions, pytest-httpserver library
- Verdict: ❌ Better for someone with testing infrastructure experience

---

Recommendation: Start with #18, then move to #13. Skip #22 until contributor has more CI/DevOps context.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "The repo-facts block shows pushes and default-branch commits inside the 90-day window."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "Recent default-branch commits are inside the 30-day window."},
      {"name": "bounded_scope", "grade": "pass", "evidence": "One localized defect: GitHubTool never sets file_structure, so two analyzer flags stay False."},
      {"name": "available", "grade": "pass", "evidence": "Issue open and unassigned, with no linked open PR at the time of the run."},
      {"name": "enough_context", "grade": "pass", "evidence": "Names the missing key, both detector functions, and the files to inspect."},
      {"name": "newcomer_signal", "grade": "pass", "evidence": "Labeled 'good first issue' (preferred; does not change the verdict)."},
      {"name": "contribution_policy", "grade": "pass", "evidence": "No statement restricting AI-assisted contributions in the repo-facts block."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/13",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "Repository activity is inside the 90-day window."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "Recent default-branch commits are inside the 30-day window."},
      {"name": "bounded_scope", "grade": "pass", "evidence": "One test file plus fixtures, though the RAG pipeline knowledge makes it slower."},
      {"name": "available", "grade": "pass", "evidence": "Open and unassigned at the time of the run."},
      {"name": "enough_context", "grade": "pass", "evidence": "Names the mocking strategy and the files involved."},
      {"name": "newcomer_signal", "grade": "pass", "evidence": "Labeled for newer contributors (preferred)."},
      {"name": "contribution_policy", "grade": "pass", "evidence": "No restriction on AI-assisted contributions."}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/22",
    "checks": [
      {"name": "maintainer_alive", "grade": "pass", "evidence": "Repository activity is inside the 90-day window."},
      {"name": "repo_in_use", "grade": "pass", "evidence": "Recent default-branch commits are inside the 30-day window."},
      {"name": "bounded_scope", "grade": "fail", "evidence": "A new mock server, fixtures, and CI changes are several linked deliverables rather than one localized behavior."},
      {"name": "available", "grade": "pass", "evidence": "Open and unassigned at the time of the run."},
      {"name": "enough_context", "grade": "pass", "evidence": "Names pytest-httpserver and the integration-test surface."},
      {"name": "newcomer_signal", "grade": "unclear", "evidence": "No beginner-oriented label stated."},
      {"name": "contribution_policy", "grade": "pass", "evidence": "No restriction on AI-assisted contributions."}
    ],
    "verdict": "reject"
  }
]
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

I recovered the filled rubric from my installed skill, filled the installed `scope.md`, and
then ran the harness against that installed copy. The one-bundle smoke run reported:

> agreement: 1/1 scored items

My two full runs then reported, in order:

> agreement: 18/20 scored items  (bar: 18/20: PASS)

> agreement: 18/20 scored items  (bar: 18/20: PASS)

Both full runs used the same rubric; I did not change a check between them, and the
second is the run written to `eval-run.txt`. The two disagreements are the two `scope`
items, `issue-15` and `issue-20`, so the category floor is still met
(`categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 2/4`).
I kept the wording rather than tightening it after the first run because the tighter
availability rule I tried in September also rejected `issue-09`, a gold `accept` whose
old claim was stale. A first run at the bar is a complete answer, so I stopped spending
credit on re-runs and spent the time on the evidence below instead.

**Issue analysis**

I analyzed `issue-15` (zulip/zulip#19589). My rubric returned `accept`, while the gold
label was `reject`, and the harness recorded:

> issue-15  reject  accept   NO     graded accept

The issue is open, unassigned, and labeled `good first issue`, and the issue text asks
for one coherent change: separate the bot mention into a `command` field and put the
message body in `text`. Every individual check therefore passed. My `available` rule
also passed it because the claim attempts were auto-revoked more than 90 days ago and
the linked PRs are closed; the rule deliberately does not treat old interest as current
work. The gold label reads the history differently. The thread shows years of design
debate and two abandoned pull requests, including a contributor saying:

> I have worked on this issue and I have made a PR. Review is pending

and the repository bot saying, when someone else tried to claim it:

> This issue cannot be claimed, as someone else is already working on it.

The rubric has no check for "previous attempts at this exact change failed", and
`bounded_scope` alone cannot see it: the issue looks bounded because the requested diff
is small, while the hard part is the unresolved design decision behind it. I did not add
that check because a rule that treats any past claim or closed PR as disqualifying also
rejected `issue-09`, where the 2022 claim was stale and a maintainer had invited new
takers. The miss is real, and it is the trade-off recorded below rather than a wording
bug I could fix without losing a clear accept.

**Check rationale**

```md
| contribution_policy | Read the `contribution policy` line in the repo-facts block. | Pass if the policy allows AI-assisted contributions, allows them with conditions such as review, testing, understanding, or disclosure, or has no statement about AI/tooling. Fail if it explicitly says AI-generated code or documentation is not accepted, AI assistance is prohibited, or an equivalent restriction conflicts with the course workflow. | required |
```

I added this check because repository activity and an apparently well-scoped issue are
not enough when the repository explicitly rejects the contribution method relevant to
this course. The check distinguishes an explicit prohibition from reasonable contributor
obligations. For example, reviewing, testing, and understanding a change are requirements
I can meet, while a repository that says it does not accept AI-generated code or
documentation cannot be a valid candidate under this workflow. That is why the pass
condition names conditions such as disclosure instead of treating every AI policy as a
rejection.

**Trade-offs**

The contribution-policy check accepts a repository with no stated AI policy, because
absence of a restriction is not evidence of a prohibition. That choice avoids rejecting
otherwise viable issues such as the accepted case whose repo-facts said:

> contribution policy (CONTRIBUTING.md): no statement on AI or contribution tooling

The trade-off is that a repository with an unstated or unclear policy could later impose
expectations that are not visible in the eval bundle. I chose not to reject on silence
because treating silence as a failure would create false rejections. The same logic
explains the `available` miss on `issue-15`: excluding historical claims keeps `issue-09`
accepted, but it also lets an issue with a long, abandoned design debate through.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

I chose issue #18 because it fits my current interest in Python debugging and learning
how information moves through an application. Its task is limited to ensuring the
repository analyzer receives a file list so that `has_tests` and `has_ci` can be
calculated correctly. The live evaluation described it as a short, focused change
involving two locations, which fits the time I have for the next unit.

The verdict correctly identified that the issue has a concrete root cause, a clear
expected result, named files to investigate, and a bounded implementation. Beyond the
rubric, I also weighed my own preference for a debugging/data-flow task over a RAG
integration-test task or an issue involving test infrastructure and CI configuration.

I expect the main difficulty will be understanding where the repository ingestion
pipeline creates and passes the file structure, then setting up enough of the project
locally to reproduce and test the incorrect `has_tests` and `has_ci` values. I will not
claim the issue until Unit 2, when I can follow the course’s claim-comment process.

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
