# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/79

**Branch**

[fix/18-has-tests]

## Eval iterations

**Run history**

20/20 agreement.

I ran one complete evaluation with the installed `pr-precheck` tool at `C:\Users\legot\.claude\skills\pr-precheck`. The run matched every gold verdict: clear-accept 7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2, and unreviewable 3/3. The final score of 20/20 matches the agreement line in the harness-written `eval-run.txt` committed at `beat-1-sandbox/unit-4/eval-run.txt`.

**Package analysis**

`pkg-03` was a `silent-drift` package. The gold verdict was `reject`, and my rubric verdict was also `reject`. My tool rejected it through the `plan-to-diff-fidelity` check because that check compares each behavior-changing change in the candidate PR diff with the implementation boundary and deviations recorded in the plan, then separately compares the PR description’s claims with what the diff actually delivers. A PR that silently adds work outside the plan, omits work while claiming it was completed, or describes behavior the diff does not support is not ready to submit even if its test evidence or writing otherwise looks acceptable.

**Check rationale**

```md
| plan-to-diff-fidelity | The plan-context block's in-scope and out-of-scope statements plus deviation notes, read against the candidate PR's changed files, unified diff, and description claims about what the change does. | Pass if every behavior-changing diff hunk is inside the plan's stated boundary or is explained by an explicit, evidence-based deviation, and the PR description makes no claim that the diff contradicts or fails to deliver. A scoped plan may intentionally leave related problems unfixed when that boundary is stated honestly. | required |
```

I made `plan-to-diff-fidelity` required because the core Unit 4 handoff is whether the implementation matches the plan that justified building it. A clean-looking diff and passing tests do not make a pull request ready if it silently expands the change, omits promised behavior while claiming completion, or describes work that the branch does not contain. I rejected a weaker check that only asks whether the files named in the plan changed, because file names alone cannot distinguish a bounded implementation from unrelated behavior-changing edits within the same file.

**Trade-offs**

This check can reject a PR whose extra change may be useful if that change is outside the posted plan and has no deviation note explaining why it was necessary. I accepted that trade-off because a reviewer needs to know whether a behavior-changing addition was intentional and justified, rather than discovering it from the diff. The check still allows necessary deviations when they are explicitly recorded and evidence-based, so it does not require an implementation to follow an earlier plan blindly.
stated reason nothing changed elsewhere. "Nothing changed, and here is how I know" earns
the point in full when the reason follows.]

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
