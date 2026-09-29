# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

ColonelToad

**Plan comment**

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

---

## Your branch

**Branch**

fix/18-has-tests

**Evidence**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/18#issuecomment-5881520300

I’m planning a bounded fix for Issue #18.

I will update GitHubTool._fetch_repo_metadata() to retrieve repository file paths through GitHub’s recursive tree API and return them as file_structure. That is the field RepoAnalyzer already reads when determining has_tests and has_ci. I will keep the implementation limited to the file-path handoff and focused test coverage; I will not expand the existing detection heuristics or refactor unrelated metadata and ingestion behavior.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

14/20 agreement → 20/20 agreement.

My first complete scored run had 14/20 agreement. The six disagreements were `pkg-02`, `pkg-03`, `pkg-05`, `pkg-08`, `pkg-09`, and `pkg-14`: each was a gold `accept` that my rubric rejected at `uncertainty-and-deviations`. I revised the required check, the Honesty section of `references/evidence-guide.md`, and the corresponding procedure steps so that the check evaluates material unsupported certainty rather than requiring every pre-build plan to list hypothetical risks or a deviation. I then ran the confirming complete evaluation, which reached 20/20 agreement. The final score matches the agreement line in my committed `eval-run.txt`.

**Package analysis**

`pkg-02` was a `clear-accept` package. The gold label was `accept`, but my initial rubric verdict was `reject` because `uncertainty-and-deviations` treated the absence of an explicit risks or deviations record as a required failure. That was an incorrect, structure-based interpretation: the package did not state an unestablished cause, behavior, convention, or implementation detail as certain, and the available evidence did not show a material unresolved fact that would change the implementation or validation. After I narrowed the check to require treatment only of material uncertainties visible in the package evidence, the rubric verdict for `pkg-02` became `accept`, matching the gold label.

**Check rationale**

```md
| uncertainty-and-deviations | The plan's statements about assumptions, risks, open questions, and deviations, read against uncertainties or constraints visible in the issue context, repro-evidence block, and repo-facts block. | Pass if the plan does not present an unestablished cause, behavior, repository convention, or implementation detail as certain. When the available evidence leaves a material question that would change the proposed implementation or validation, the plan identifies it as an assumption, question, or verification step. Treat a stated deviation record as sufficient when present, but do not fail a pre-build plan solely because it has no deviation to report or does not list speculative risks. | required |
```

I made this check required because a plan that states an unproven cause, repository behavior, or implementation detail as certain can direct the build toward the wrong change. My first version was too strict because it required every pre-build plan to include explicit risks and a deviations record, even when no material uncertainty was visible and implementation had not started. I revised the check to reject unsupported certainty and hidden material unknowns, while allowing a grounded pre-build plan to omit speculative risks or deviations that do not yet exist. I rejected the earlier version because it graded the presence of a planning-document section rather than whether the plan was honest about evidence that could change the build or validation.

**Trade-offs**

The revised `uncertainty-and-deviations` check can miss an implementation risk that exists in the real repository but is not visible in the issue context, repro evidence, or repo-facts available to the evaluator. I accepted that limit because treating unshown information as a mandatory requirement caused the initial false rejections of clear pre-build plans such as `pkg-02`. The procedure still requires an assumption, question, or verification task when a material unknown is visible and could change the implementation or validation. I also reran the complete evaluation after the change; it maintained correct results across the wrong-cause, scope-creep, unbuildable, and thread-convention categories and reached 20/20 agreement.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
