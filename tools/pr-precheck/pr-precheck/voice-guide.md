# Voice guide: how I talk upstream

<!--
THIS IS A CARRY-OVER SLOT, not a new hole. You wrote this guide in
week 2 and carried it through week 3; paste your filled voice-guide.md
here, whole. It is not re-authored and it is not graded as new work
this week.

Then reread it with the PR in mind. Your comments promised, reported,
and committed to an approach; a PR title and description ask a
maintainer to spend review time on your work. If your rules do not
cover that register (for example: how a title earns its thirty
seconds, how a description promises exactly what the diff contains,
how a disclosed shortfall is worded so it reads as honesty rather
than apology), extend the guide with what it needs. Extending is
allowed and encouraged; starting over is not required.

Live mode reads this file and holds your draft PR title and
description against it, reporting any rule your draft breaks. Eval
mode ignores it entirely, because your voice is yours and carries no
gold labels.
-->

## Who I am in threads

I am a student contributor learning this repository by investigating one issue at a time. I will report what I personally tried, the environment I used, and the evidence I observed.

Readers can expect a clear claim before I begin and an evidence-backed follow-up afterward. I do not speak for maintainers, other contributors, or the project as a whole.

## Rules I write by

### Rule: Report observations, not certainty

I describe the behavior I observed and connect it to the evidence. I do not call something a root cause, regression, or confirmed fix unless my work actually establishes that.

- Wrong: "This is definitely caused by the cache implementation."
- Right: "Under the environment below, I observed the reported error after clearing the cache and running the listed command."

### Rule: Keep the claim prospective

My claim comment says what I intend to investigate, not that I have already reproduced or verified the issue.

- Wrong: "I reproduced this and will send the fix soon."
- Right: "I’m claiming this issue to attempt a reproduction, and I’ll follow up with the environment, steps, and result."

### Rule: Name the conditions

I include the specific version, platform, command, input, or configuration that makes my result meaningful instead of relying on vague statements.

- Wrong: "It fails on my machine."
- Right: "On Windows 11 with Node 22.14.0, running `npm test -- parser`, I received the error excerpt below."

### Rule: Separate result from interpretation

I state what happened before suggesting what it might mean. A suggestion is labeled as a suggestion, not written as a fact.

- Wrong: "The parser is broken because it ignores empty files."
- Right: "The parser returned an empty result for the attached input; this may be related to its handling of empty files, but I have not confirmed the cause."

### Rule: Make non-reproduction useful

If I cannot reproduce the issue, I say so plainly and document the attempt rather than treating that result as evidence that the report is invalid.

- Wrong: "Cannot reproduce, so this bug is not real."
- Right: "I could not reproduce the behavior under the environment and steps below; the result may depend on a version, configuration, or condition I did not match."

## Things I never post

- A claim that I reproduced, fixed, diagnosed, or verified something before I have evidence for it.
- "Works for me" without the environment, trigger, and observed result.
- "Same as above," "can confirm," or another contributor’s proof presented as my own work.
- A claim that an issue is invalid just because I could not reproduce it once.
- A promise to deliver a fix, timeline, review, or support outcome that I do not control.
- A confident root-cause statement based only on a symptom or one failed test.
