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

**The verdict must record `accept` for this issue.** Choose an issue your own skill
accepts. If your skill rejects every candidate you try, that is a signal about your
rubric rather than about the issues: revise it and re-run — retries are unlimited and a
partial re-run costs about $0.20 — or run the skill on different candidates. Output
recording `reject` for the issue you chose earns no credit for this field.

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

My full-run agreement scores, in order, were:

> agreement: 13/20 scored items  (bar: 18/20: below the bar)

> agreement: 14/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)

> agreement: 17/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in policy)

> agreement: 19/20 scored items  (bar: 18/20: PASS)

Between runs, I changed the rubric based on the failed-check notes. I broadened the maintainer-activity evidence, clarified that a coherent issue can span related files or examples, separated stale claims from active work, and added an explicit contribution-policy check.

**Issue analysis**

I analyzed `issue-15`. My rubric returned `accept`, while the gold label was `reject`:

> issue-15  reject  accept   NO     graded accept

The issue was open and unassigned in the repo-facts block, and its label included `good first issue`, so my rubric treated it as available. However, the comment history contained repeated claim attempts and prior work signals, including:

> I have worked on this issue and I have made a PR. Review is pending

and a Zulip bot message:

> This issue cannot be claimed, as someone else is already working on it.

My availability wording still did not reliably make Claude reject this case. I kept the final rule rather than treating every historical claim as blocking, because a stricter rule had previously rejected `issue-09`, where old interest did not mean the issue was actively taken.

**Check rationale**

| contribution_policy | Read the `contribution policy` line in the repo-facts block. | Pass if the policy allows AI-assisted contributions, allows them with conditions such as review, testing, understanding, or disclosure, or has no statement about AI/tooling. Fail if it explicitly says AI-generated code or documentation is not accepted, AI assistance is prohibited, or an equivalent restriction conflicts with the course workflow. | required |

I added this check because repository activity and an apparently well-scoped issue are not enough when the repository explicitly rejects the contribution method relevant to this course. The check distinguishes an explicit prohibition from reasonable contributor obligations. For example, reviewing, testing, and understanding a change are requirements I can meet, while a repository that says it does not accept AI-generated code or documentation cannot be a valid candidate under this workflow.

**Trade-offs**

The contribution-policy check accepts a repository with no stated AI policy, because absence of a restriction is not evidence of a prohibition. That choice avoids rejecting otherwise viable issues such as the accepted case whose repo-facts said:

> contribution policy (CONTRIBUTING.md): no statement on AI or contribution tooling

The trade-off is that a repository with an unstated or unclear policy could later impose expectations that are not visible in the eval bundle. I chose not to reject on silence because treating silence as a failure would create false rejections.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

[Answer all three:

1. The issue's fit to your interests and to the time available.
2. What the verdict identified correctly, and what you weighed that the rubric could
   not.
3. The anticipated difficulty in claiming it.]

---

I chose issue #18 because it fits my current interest in Python debugging and learning how information moves through an application. Its task is limited to ensuring the repository analyzer receives a file list so that `has_tests` and `has_ci` can be calculated correctly. The live evaluation described it as a short, focused change involving two locations, which fits the time I have for the next unit.

The verdict correctly identified that the issue has a concrete root cause, a clear expected result, named files to investigate, and a bounded implementation. Beyond the rubric, I also weighed my own preference for a debugging/data-flow task over a RAG integration-test task or an issue involving test infrastructure and CI configuration.

I expect the main difficulty will be understanding where the repository ingestion pipeline creates and passes the file structure, then setting up enough of the project locally to reproduce and test the incorrect `has_tests` and `has_ci` values. I will not claim the issue until Unit 2, when I can follow the course’s claim-comment process.

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.