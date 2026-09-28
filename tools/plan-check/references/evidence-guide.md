# Evidence guide: where evidence lives in a plan package

<!--
THIS IS THE PART YOU WRITE (second week running: the judgment files
stay in your hands). The skill uses this guide as its map: for every
kind of evidence a rubric check names, this file says WHERE to find it
in a plan package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repro-evidence block,
  the candidate plan's scope statement or test plan, the plan comment,
  the repo-facts block). In live mode (where on GitHub or in the
  draft: the issue thread, the student's posted repro comment, the
  repo's docs, the draft plan and comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the stated cause cites behavior the
  repro evidence actually shows") over adjectives ("diagnosis is
  solid").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute, and this week that cuts twice: your
procedure.md tells the skill WHEN to gather each family, and this
guide tells it WHERE. Write the map you wish your executor had.
-->

## Diagnosis and grounding

**Where it lives:** In an eval bundle, read the issue context and repro-evidence block for the reported trigger, environment, expected and actual behavior, and output/log artifact; then read the candidate plan's goal, diagnosis, and planned-change explanation. In live mode, read the issue thread, the student's posted repro comment or saved reproduction evidence, and the draft plan/comment.

**What good looks like:** The plan's explanation accounts for the behavior and conditions that the reproduction actually establishes. When the evidence shows a missing value or failing boundary but does not prove why it is missing, the plan labels the upstream cause as an assumption or a verification task instead of presenting it as fact.

## Scope

**Where it lives:** In an eval bundle, inspect the candidate plan's scope statement, named files/areas, planned changes, explicit exclusions, and repo-facts block. In live mode, inspect the plan draft, issue context, repository layout, and relevant contribution guidance.

**What good looks like:** One bounded change follows the evidenced path from cause or failed boundary to observable behavior. The plan names what it will change and what it will not change, excluding unrelated cleanup, redesign, or new features unless the available evidence shows they are necessary for the issue.

## Executability

**Where it lives:** In an eval bundle, read the candidate plan's ordered implementation steps and named files/areas alongside the repo-facts block. In live mode, read the draft plan against the checked-out repository's actual file layout, relevant source files, and setup/test documentation.

**What good looks like:** Another contributor can locate the first code area, understand the intended data/control-flow change, make the steps in a sensible order, and know which adjacent behavior must remain intact. The plan may leave genuinely unknown implementation details open, but it must state how they will be resolved rather than hiding a behavior-changing decision.

## Test plan

**Where it lives:** In an eval bundle, compare the candidate plan's validation section with the repro-evidence block's steps, expected/actual outcome, and artifact, plus the repo-facts block's test conventions. In live mode, compare the plan's test steps with the posted repro report, repository documentation, and relevant test files or commands.

**What good looks like:** The plan identifies the original trigger or closest real path, the input/conditions that matter, and a visible result that should differ after the change. A decisive plan says what will be observed before and after, such as an input field becoming populated and a resulting detection flag changing, rather than merely promising to run tests.

## Honesty

**Where it lives:** In an eval bundle, read the plan's assumptions, risks, open questions, and deviations section against the issue context, repro evidence, and repo facts. In live mode, read those plan sections against the student's saved evidence and the checked-out repository.

**What good looks like:** Facts established by the reproduction are distinguished from hypotheses and implementation unknowns. The plan includes a deviation record that will state either what changed from the plan and why, or that the implementation followed the plan without deviation; it does not silently erase a meaningful discovery made during the build.

## Comms

**Where it lives:** In an eval bundle, read the plan comment against the issue thread/highlights and any template, contribution guide, or AI-use requirement identified in the repo-facts block. In live mode, read the issue thread, applicable repository documents, and the student's draft plan comment.

**What good looks like:** The comment names the issue, communicates a bounded proposed change and an evidence-based validation approach, and stays within what the available reproduction evidence supports. It follows a repository-specific template, convention, or disclosure requirement only when the package identifies that requirement as applicable; it does not rely on boilerplate, generic promises, or an unverified root-cause claim.