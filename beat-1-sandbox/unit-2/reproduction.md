# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

ColonelToad

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18#issuecomment-5822769063

I’m claiming [Issue #18](https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18), “Repo analyzer never receives a file list, so has_tests and has_ci are always False.”

I will set up the repository following its documented instructions, trace the repository-data path from `agent/tools/github_tool.py` to `ingestion/parsers/repo_analyzer.py`, and attempt to reproduce whether the analyzer receives a `file_structure` value and reports `has_tests` and `has_ci` as `False` for a repository whose files should indicate tests or CI. I will follow up with my environment, the exact steps and commands I used, the observed result, and supporting evidence.

**Reproduction comment**

No reproduction comment was posted on the shared thread. The claim above promised a
follow-up, and I kept the thread to the claim and the plan rather than adding a second
comment. The report I wrote and ran is kept below and in
`beat-1-sandbox/unit-2/repro-report.md`.

Environment: WSL2 Debian on Windows 11, Python 3.12, repository
`ColonelToad/pathreview-ai301-fa26-s1` at commit `f89c06f`, fresh virtual environment
with `httpx` and `structlog`. I checked out `main`, ran a small check that calls the real
`GitHubTool.execute()` and the real `RepoAnalyzer.parse()` against fixed GitHub
responses (a tree containing `tests/test_app.py` and `.github/workflows/ci.yml`), then
repeated the same command on `fix/18-has-tests`.

On `main`:

```
metadata keys: description, fork_count, has_readme, homepage, last_commit_date, name, open_issues_count, primary_language, star_count, topics
file_structure present: False
analyzer has_tests: False
analyzer has_ci: False
```

On `fix/18-has-tests`:

```
metadata keys: description, file_structure, fork_count, has_readme, homepage, last_commit_date, name, open_issues_count, primary_language, star_count, topics
file_structure present: True
file_structure entries: 3
contains tests/test_app.py: True
contains .github/workflows/ci.yml: True
analyzer has_tests: True
analyzer has_ci: True
```

Conclusion: under these conditions the reported behavior reproduces on `main`, and the
same check passes on the branch. The full script, including the stubbed responses, is in
`repro-report.md` so a stranger can run it again.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

The committed September run ended below the bar because the disclosure category had no
match:

> agreement: 19/20 scored items  (bar: 18/20: below the bar; category floor unmet: no match in disclosure)

I rewrote `repo-aware-comms` so the positive condition comes first, then re-graded only
the miss and one canary from each category the change could touch
(`pkg-20,pkg-01,pkg-04,pkg-19,pkg-02`):

> agreement: 5/5 scored items

The confirming full run then reported:

> agreement: 19/20 scored items  (bar: 18/20: PASS)

That last score is the agreement line in the committed `eval-run.txt`
(`categories: clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`).
The full run's miss moved from `pkg-20` to `pkg-12`; the disclosure category now has its
match, so the bar and the category floor are both met.

**Package analysis**

I analyzed `pkg-20` (ghostty-org/ghostty#13604). The gold label is `reject`, in the
`disclosure` category. My September rubric returned `accept`; the committed run recorded:

> pkg-20  reject  accept   NO     graded accept

The package is a strong reproduction on every proof check, which is why every other check
passed. The failure was communication: the repo-facts block says the project's AI policy
requires all AI usage in issues and comments to be disclosed, stating the tool and the
extent of the assistance, and the candidate claim comment does not disclose AI use. My
old pass condition ended with escape clauses — it told the grader not to fail for a
missing disclosure "when no applicable rule is shown" — and the model read the policy as
background rather than as an applicable rule, so the omission never reached the verdict.
The rewrite states the positive condition first: when the repository's stated policy
requires disclosure in issues or comments, the comment must disclose. `pkg-20` now fails
on the omission, which is the outcome the gold label expects.

**Check rationale**

```md
| repo-aware-comms | The claim comment and repro comment, read against the issue thread and the repository facts, templates, contribution guidance, and disclosure requirements identified in the evidence guide. | Pass if the comment complies with every repository requirement that the package's evidence shows applies to it: when the repository's stated policy requires AI-assistance disclosure in issues or comments, the comment discloses that AI was used and the extent of the assistance; when the repository's template or guidance requires specific information in a claim or report, the comment supplies it; and in all cases the comment states a specific, evidence-backed outcome in the author's own words. Fail when an applicable stated requirement exists and the comment omits or contradicts it, even if the rest of the comment is specific and well written. If the evidence shows no applicable requirement, the specificity and authorship condition alone decides the check. | required |
```

I revised this check after the September run because its old wording was defensive
first: it described specificity and then told the grader when *not* to fail, which made
the disclosure case easy to explain away. The new wording names the positive obligations
in order, so an applicable disclosure rule is a condition the comment must meet rather
than an exception the grader has to notice. I rejected a version that failed any package
whose repo had a policy file at all, because a policy that only asks contributors to
understand and test their work is compatible with this course; the check should fail on
an omitted requirement, not on the existence of a policy.

**Trade-offs**

The rewritten check can over-read a repository's template as a requirement on the
comment. That is what happened to `pkg-12`: prettier's bug template asks for a runnable
reproduction, the candidate report says the script contains the issue's two calls but
does not paste the script, and the rubric rejected a package the gold label accepts. I
kept the wording because the alternative — ignoring stated template asks — is what let
`pkg-20` through, and the disclosure wall is the category the course treats as the one a
rubric must not miss. I re-ran `pkg-01`, `pkg-04`, `pkg-19`, and `pkg-02` as canaries with
`--only`; all four held, so the cost of the stricter reading is visible in `pkg-12`
rather than spread across the other categories.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
