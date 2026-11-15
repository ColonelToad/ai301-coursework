# Voice guide: how I talk upstream

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

## Upstream: how I talk to maintainers

### Claims I post

#### Rule: Claim with a first step and a report-back

A claim names the specific thing I will try first and says when I will report back. It never promises a fix, a merge, or a date I do not control.

- Wrong: "I'll fix this and have a PR up this week."
- Right: "I'd like to take this. I'll reproduce the stale-store failure first, then report the environment and result before opening a PR."

#### Rule: Read the thread before claiming

Before I claim, I read every comment. If someone has already asked to work on it, it is taken even without an assignment, so I choose another issue instead of claiming over them.

- Wrong: "Also interested, I'll take this one."
- Right: "It looks like @name is already working on this, so I'll leave it to them and pick another issue."

### Questions I ask

#### Rule: One answerable question, with what I tried

A question names the version, command, and result I already tried, and asks one thing a maintainer can answer from their chair. It never asks them to reproduce the problem for me.

- Wrong: "Any idea why this doesn't work?"
- Right: "On 2.1.4, leaving the store directory as objects/ only makes capture fail with ENOTEMPTY; deleting HEAD and re-running did not help. Is removing a HEAD-less store the intended recovery path, or should it fail loudly?"

#### Rule: Ask before building on an open design edge

When the issue leaves a design choice open, I ask which direction is wanted before writing the fix, and I say what I would do by default.

- Wrong: "I'll implement the watcher approach and open the PR."
- Right: "The issue does not say whether reload should watch files or expose an explicit command. I would default to an explicit command first; is that the direction you want before I build it?"

### Pings I send

#### Rule: Nudge once, on the repo's clock

I nudge only after the thread has been quiet longer than the repo's usual review rhythm. The nudge states the current state and makes one ask, and I never send it twice.

- Wrong: "Bumping this, any update?"
- Right: "The branch still applies cleanly and `bun run check` passes; is there anything you'd like changed before this can be reviewed?"
