# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->
## Environment

**Where it lives:** In an eval bundle, read the issue context for the versions, operating system, deployment mode, configuration, or prerequisites the reporter names; then read the repro report's environment record and repo-facts block. In live mode, compare the student's draft report with the issue body/comments and the repository's setup or installation documentation.

**What good looks like:** The report names the environment details that could change the result, such as OS, runtime or language version, package/app version or commit, install method, configuration, and relevant service or hardware conditions. Those details match the issue's target, or the report calls out the difference and does not imply it tested the exact target when it did not.

## Steps

**Where it lives:** In an eval bundle, find the repro report's starting state, setup, ordered actions or commands, and expected-versus-actual result. In live mode, find those details in the draft comment/report and confirm any setup assumptions against the repository's documented instructions.

**What good looks like:** A reader can identify where to start, prepare the same relevant conditions, take the stated actions in sequence, and recognize the result to compare. Commands, inputs, configuration changes, and triggering actions are included when leaving them out could produce a different outcome.

## Behavior shown

**Where it lives:** In an eval bundle, read the output excerpt, error text, log, screenshot, test result, recording, or other artifact in the repro report alongside the issue description and repo-facts block. In live mode, compare the draft's linked or pasted evidence with the issue's stated expected and actual behavior.

**What good looks like:** The artifact is tied to the reported attempt and visibly supports the stated outcome. Its error, result, or behavior matches the issue's trigger and symptom rather than merely showing that something else failed nearby; when a reproduction did not occur, the report shows what was attempted and what happened instead.

## Honesty

**Where it lives:** In an eval bundle, compare the claim comment and the report's conclusion with the environment record, steps, and behavior artifact. In live mode, compare the student's draft claims with the evidence included or linked in the same draft and the issue thread.

**What good looks like:** The claim comment is prospective: it names the issue, says the author will investigate or attempt reproduction, and promises a report. The repro report distinguishes observed facts from interpretation, says "could not reproduce under these conditions" when appropriate, and never turns missing, ambiguous, or different evidence into a stronger conclusion than it supports.

## Comms

**Where it lives:** In an eval bundle, inspect the claim comment and final repro comment against the issue context, repo-facts block, and any contribution guide, issue template, code of conduct, or AI-use policy actually provided or cited by the package. In live mode, inspect the issue thread, the repository documents that apply to the issue, and the student's draft comment.

**What good looks like:** The claim identifies the issue and planned investigation without claiming a completed result. The final comment states a specific, evidence-backed outcome in the author's own words. A package must follow a template, disclosure requirement, or other convention only when the applicable instruction is identified; missing optional headings, unshown conventions, or generic boilerplate alone do not make an otherwise honest report fail.