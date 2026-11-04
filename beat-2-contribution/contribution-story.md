# Contribution: snapshot: Git.repo.create deadlocks (ENOTEMPTY) when the store dir is a stale partial repo

Path: `beat-2-contribution/contribution-story.md`

**Contribution Number:** 1
**Student:** Christopher Onyiuke
**GitHub Username:** ColonelToad
**Issue:** https://github.com/anomalyco/opencode/issues/59751
**Pull Request:** https://github.com/anomalyco/opencode/pull/60061

---

## Toolkit

The tool blocks below follow the story in order. One dated entry per week of
tool work comes first; each story section then carries its own block.

**Unit 5: built issue-scout and a nine-query C++/Rust/TypeScript strategy**

- Installed the scout over my unit-1 selector and wrote nine queries across C++,
  Rust, and TypeScript.
- Dry-ran the queries before spending. The five broad searches returned
  week-old, zero-comment repos, so I added repo-scoped queries and a
  beginner-label ordering.
- First scan: the top five were all LLVM `good first issue` issues, and all five
  rejected on `contribution_policy`. The rubric was right; my ranking had spent
  the whole budget on ineligible issues.
- Replaced the LLVM query with `help wanted` and dropped LLVM `good first issue`
  results from the ranking.
- Second scan: one accept (wasmtime #5879) and four rejects, all blocked by open
  PRs that `gh search issues` cannot see.
- The re-grade of my merged Verilator issue rejected it on `available`, which is
  correct: the issue is closed by a merged PR.
- The scan also exposed a rubric bug. It failed an LLVM `help wanted` issue on
  `contribution_policy` even though LLVM's AI ban names only `good first issue`.
  I made that check label-aware: a repo-wide ban still fails, but a
  label-specific ban fails only when the issue carries that label, so a
  `help wanted` or unlabeled issue in the same repository passes.

**Unit 6: installed the voice guard and a repo-memory file**

- Installed `check_voice.py` and one `PreToolUse` hook, and promoted a single
  canonical `voice-guide.md` at `~/.claude/voice-guard/`.
- Wrote the Upstream section — rules for claims, questions, and pings — each
  with a wrong/right pair from my own hand.
- Verified the guard twice: a bad draft was stopped with the exact rule quoted,
  and a good draft passed. `guard-log.md` records both.
- Two install problems, both fixed. Symlinks created on `/mnt/c` are unreadable
  from Windows, so the canonical guide is hardlinked into each skill instead of
  symlinked. Windows Python could not launch `claude.cmd`, so the hook runs
  through `wsl.exe` with the script path inside a quoted `bash -c`.
- Wrote `CLAUDE.local.md` in the opencode clone with the build/test commands and
  the gotchas that cost me time: tests run from package directories, and
  `bun test` at the repo root exits on purpose.

**Unit 7: installed the ship-gate and wrote a manifest for anomalyco/opencode**

- Installed the gate, then repointed its hook through `wsl.exe` for the same
  reason as the voice guard: the generated `python3 /mnt/c/...` command is not
  something Windows can run directly.
- Wrote three manifest rows from CONTRIBUTING.md: `bun install`, `bun run
  check`, and the two test files the fix touches.
- Tested the hook and got the two expected stops, one from the voice guard and
  one from the ship-gate, both recorded in their logs.
- The first real branch run stopped: my local `dev` ref was 32 commits behind,
  so the gate diffed 295 files instead of 4, and my PR draft claimed
  interruption behavior the tests do not cover.
- Fast-forwarded `dev` to `upstream/dev`, added a Limitations section, and
  removed the untested claim. The next run passed all required checks.

**Unit 8: installed the pr-monitor and wrote a playbook**

- Installed the monitor as a folder copy. It has no hook on purpose; it is
  invoked, not latched.
- `--state` found #60061 on the first try: 6 days old, 5 checks passing,
  mergeable but merge state BLOCKED, one thread item — the bot review.
- Wrote the four-section playbook with a 7-day human clock, a 3-day floor for a
  direct question, one nudge per PR, and a 30-day walk-away after the nudge.
- The full run drafted **"No reply and no nudge. Wait."** because the only
  review is the bot's, it asks nothing, and bot activity does not reset the
  human clock. The draft names 2026-11-05 as the first day a nudge is allowed.
- I did not revise or post it. Waiting is the correct move here, and the
  monitor's own draft says a thank-you reply would add noise, not information.

---

## Why I Chose This Issue

My own scan did not surface this issue. Its top five were LLVM `good first
issue` issues, and after the strategy patch the only accept was wasmtime #5879.
I brought opencode #59751 in myself because it fits the same profile I had been
grading against: a TypeScript developer tool, a named and testable behavior, a
mature repository with a real test harness, and no beginner label that would put
me on a fast track I had not earned. The reporter had supplied far more than a
symptom — a store path, a reproduction, and page-level damage from a corrupted
database — and the bot had already reproduced it on both 2.1.4 and `dev`. That
made it a bug I could check myself instead of one I would have to take on faith.

I claimed it on 2026-10-28, before I installed the Unit 6 tools, and opened PR
#60061 the next day. The tool phases below were run afterwards, against the
real branch, not reconstructed afterwards from memory. When I re-graded the
issue with the scout on 2026-11-02, it rejected on `available` because my own
claim and PR are now open. That is the correct answer to "should a newcomer take
this now?", not a verdict that the work was out of scope. The scout also put it
at **moderate** difficulty under the new `scope-feasible` rule: the failure
location was named, but the cache side was an open edge I had to raise with the
reporter before building.

Claim: https://github.com/anomalyco/opencode/issues/59751#issuecomment-6476988954

**Toolkit**

- Tool: issue-scout (my own search strategy and rubric), run in direct mode on
  the issue URL after the claim.
- What it produced: a reject on `available` (my claim and PR #60061 are open), a
  pass on `contribution_policy` after reading opencode's CONTRIBUTING.md, and a
  **moderate** difficulty note. Earlier, the same rubric graded my merged
  Verilator issue as a reject on `available`, and my scan accepted wasmtime
  #5879 with a scope caveat.
- Difficulties: two. The broad-search ranking surfaced ineligible LLVM
  `good first issue` issues, and the policy check read an agent-facing
  "never open PRs or comment" rule as a contributor ban. The `available` check
  also cannot see linked PRs from search results, so it only catches them at
  grading time.
- What changed: the LLVM query moved from `good first issue` to `help wanted`,
  LLVM `good first issue` results are dropped in ranking, `bounded_scope`
  became `scope-feasible` with a difficulty note, and `contribution_policy`
  now reads the linked policy itself and separates agent rules from contributor
  rules.

---

## Understanding the Issue

**Problem Description**

`Git.repo.create` builds a temporary Git repository and renames it onto the
store path. It handles a successful rename and a target that already has `HEAD`,
but not a target that exists without `HEAD`. The reporter's case was a stray
empty `git init` in a directory opencode snapshotted, which leaves exactly that
state. The next rename then fails with `ENOTEMPTY`, and the code has no branch
for a target that exists but is not a valid store, so the stale directory is
never removed or repaired. Every later capture for that workspace fails the same
way. The failure is then memoized: the location keeps the failed repository
fiber, so captures replay the error even after the stale directory is deleted by
hand. Only a location rebuild — a server restart or `opencode reload` — clears
it.

**Expected Behavior**

A create against a stale partial store should recover: discard a target without
`HEAD`, retry the rename once, and keep a valid store that a concurrent creator
finished in the meantime. A failed repository initialization should not be
cached for the lifetime of the location; the next capture should try again.
Successful initialization should stay cached, and cancelling a caller should
still leave the shared attempt running.

**Current Behavior**

With `upstream/dev`'s source, the rename in `Git.repo.create` returns
`ENOTEMPTY`, no `HEAD` appears, and the stale directory stays. In
`packages/core/src/snapshot.ts`, the location-scoped repository fiber keeps the
failed result, so a second `capture()` returns nothing instead of retrying.

**Affected Components**

- `packages/core/src/git.ts` — `Git.repo.create`: the temp-directory build, the
  `rename`, and the `OperationError` path.
- `packages/core/src/snapshot.ts` — the location-scoped repository fiber and
  the capture path that joins it.
- `packages/core/test/git.test.ts` and `packages/core/test/snapshot.test.ts` —
  where the regression tests live.
- The snapshot store itself, at
  `~/.local/share/opencode/snapshot/global/<hash of worktree>/`.

**Toolkit**

- Tool: none of the course skills did this phase. I read the issue body and
  thread with `gh issue view`, then read `packages/core/src/git.ts` and
  `packages/core/src/snapshot.ts` directly.
- What it produced: the two-part failure description above — the unhandled
  stale target at the rename, and the cached failed fiber at the capture.
- Difficulties: the issue's reproduction is a TUI prompt with
  `snapshots: true`, which is slow and noisy to drive. Reading the code found a
  smaller, deterministic entry point through `Git.repo.create` itself.
- What changed: nothing yet at this stage; the two candidate fixes and the open
  cache-scope question moved into the plan.

---

## Reproduction Process

**Environment Setup**

- OS: Debian GNU/Linux 13 (trixie) on WSL2, kernel
  `6.18.33.2-microsoft-standard-WSL2`
- Runtime: Bun `1.4.4`, the version pinned by `packageManager` in the repo
- Repo: `anomalyco/opencode`, `upstream/dev` at `69269e4ebe`; the branch's merge
  base is `c5d9c308`
- Branch under test: `snapshot-recovery` at `33f256f4ba` (PR #60061)
- Setup: `bun install` at the repo root. The issue reports the released binary
  `2.1.4`; this runs the source at the `dev` commit above, where the bug still
  applies.

The one real challenge was where to run the tests: `bun test` at the repo root
exits on purpose (`do not run tests from root`), so every run below starts from
`packages/core`.

**Steps to Reproduce**

The branch lives on the fork, so fetch both remotes:

```
git clone https://github.com/ColonelToad/opencode.git
cd opencode
git remote add upstream https://github.com/anomalyco/opencode.git
git fetch upstream dev
git switch snapshot-recovery
bun install
```

**A. Stale store without HEAD**

1. `cd packages/core`
2. Run `bun test test/git.test.ts -t "replaces a stale store directory that has no HEAD"`.
   Expected: pass. Actual: pass, 2 expects.
3. Replace only the production source with `dev`, keeping the test:
   `git checkout upstream/dev -- packages/core/src/git.ts packages/core/src/snapshot.ts`
   (from the repo root).
4. Re-run the same test. Expected: fail on the stale store. Actual: fails with
   `ENOTEMPTY`, and no `HEAD` is created.
5. Restore the fix: `git checkout HEAD -- packages/core/src/git.ts packages/core/src/snapshot.ts`.

The test creates a Git repo, makes a store directory containing only `objects/`
(no `HEAD`), and calls `Git.repo.create` with that store as `gitDirectory`.

**B. Cached repository resolution**

1. `cd packages/core`
2. Run `bun test test/snapshot.test.ts -t "retries repository initialization"`.
   Expected: pass. Actual: pass, 6 expects.
3. Replace only `packages/core/src/snapshot.ts` with the `dev` version.
4. Re-run. Expected: the second capture reuses the failed resolution and returns
   nothing. Actual: fails at the second `capture()` with `Received: undefined`.
5. Restore `snapshot.ts` from the branch.

**Reproduction Evidence**

- **Commit showing reproduction:**
  https://github.com/ColonelToad/opencode/commit/08f7a7dbcea47ad53e8625ed6e75005905fd1053
  (the stale-store test) and
  https://github.com/ColonelToad/opencode/commit/33f256f4bafa5f4e72fdf4433fdee72b30d9c963
  (the cached-fiber test).
- **Screenshots/logs:** the failing runs below, captured from the terminal.

A, before the fix (step 4):

```
Git.OperationError: Failed to move Git storage into place
...
ENOTEMPTY: directory not empty, rename
'/tmp/opencode-core-test-hSHhVZ/storage.init-4634-ezksktepglj' ->
'/tmp/opencode-core-test-hSHhVZ/storage'
 syscall: "rename"
   errno: -39
    code: "ENOTEMPTY"

(fail) Git > replaces a stale store directory that has no HEAD

 0 pass
 21 filtered out
 1 fail
```

A, after the fix (step 2):

```
bun test v1.4.4 (38afee6c8)

 1 pass
 21 filtered out
 0 fail
 2 expect() calls
Ran 1 test across 1 file.
```

B, before the fix (step 4):

```
error: expect(received).toBeDefined()

Received: undefined

      at .../packages/core/test/snapshot.test.ts:132:47

(fail) Snapshot > retries repository initialization after a failed create

 1 fail
```

B, after the fix (step 2):

```
 1 pass
 11 filtered out
 0 fail
 6 expect() calls
Ran 1 test across 1 file.
```

- **My findings:** with `dev`'s `git.ts`, an `objects/`-only store makes the
  rename return `ENOTEMPTY`; the directory stays and no `HEAD` appears. With
  `dev`'s `snapshot.ts`, a second `capture()` replays the first failure and
  returns nothing. I have labelled the cause as a suggested cause, not a
  confirmed root cause: the two runs show the behavior, and the code reading
  explains it, but they do not prove no other path contributes.

**Toolkit**

- Tool: repro-check in live mode, run on my claim comment and this report
  together, pointed at the issue URL.
- What it produced: a **reject** on the first pairing and an **accept** after
  revision. The reject was specific: the report could not be followed by a
  stranger (the branch was only on my fork, and `upstream/dev` was never added),
  the cache half had no output of its own, and the opening line asserted a
  cached failure before showing it. The second run passed all five required
  checks and returned `accept`.
- Difficulties: the report had to describe a fix-verification method rather than
  a repeat of the bot's 2.1.4 TUI reproduction, and it had to say so plainly so
  it would not read as a duplicate. The grader also caught two inconsistent
  "after the fix" step labels.
- What changed: the report gained the fork clone and `upstream` remote steps,
  expected-versus-actual for both tests, a separate before/after block for the
  cached-fiber case, a line saying it does not repeat the 2.1.4 reproduction,
  and a fix to the step labels.

---

## Solution Approach

**Analysis**

There are two failure points, and the reproduction ran both.

1. In `Git.repo.create` (`packages/core/src/git.ts`), the function builds a
   repository in a temporary sibling directory and renames it onto
   `input.gitDirectory`. It handles a successful rename, and it returns early
   when the target already has `HEAD`. It has no branch for a target that
   exists, is non-empty, and has no `HEAD`: `rename` returns `ENOTEMPTY`, the
   stale directory stays, and the create fails every time it is retried. The
   reporter's case was a stray empty `git init` in a directory opencode
   snapshotted, so the store path itself was a non-empty directory without
   `HEAD`.
2. In `packages/core/src/snapshot.ts`, the location-scoped repository is cached
   with `Effect.cached`, which caches a failed initialization as readily as a
   successful one. Every later `capture()` joins the same failed fiber, so
   deleting the stale directory by hand does not recover the location; only a
   reload does.

The first point is directly checkable in the code and in the failing test. The
second is visible in the cached-fiber test, which shows the second `capture()`
returning `undefined`. I treat both as the mechanisms the evidence supports, not
as proof that no other path contributes.

**Proposed Solution**

In `Git.repo.create`, after a failed rename whose target has no `HEAD`, remove
the target and retry the rename once. If the retry fails and the target still
has no `HEAD`, raise the existing `OperationError` with the retry's cause. A
concurrent creator that finished in the meantime has `HEAD` and is returned by
the existing check, so a valid store is adopted rather than deleted.

In `snapshot.ts`, replace `Effect.cached` with
`Effect.cachedInvalidateWithTTL(..., Duration.infinity)` and invalidate only
when the joined fiber fails. Keep the join `Effect.uninterruptible`, so a
caller's cancellation does not kill the shared attempt, and keep a successful
result cached for the location's lifetime.

Deliberately not changing: the TUI prompt path, the capture engine below
`Git.repo.create`, the packing schedule, the public API, and databases already
damaged before the fix.

**Implementation Plan**

1. In `packages/core/src/git.ts`, add the remove-and-retry-once branch after the
   first rename and the `HEAD` check, and reuse the existing error path with the
   retry's cause.
2. In `packages/core/src/snapshot.ts`, switch the cache to
   `cachedInvalidateWithTTL` and invalidate on failure inside the joined fiber.
3. Add `replaces a stale store directory that has no HEAD` to
   `packages/core/test/git.test.ts` and
   `retries repository initialization after a failed create` to
   `packages/core/test/snapshot.test.ts`.
4. Run both test files, then `bun run check`.

Deviation record: the cache fix was not in my first claim. The reporter asked
for it in the same PR if it stayed small, and approved the design before I
built it, so it is part of the plan rather than drift. Late in the tool phase, a
gate run showed that the origin sentence in my first commit's code comment ("a
create that died between `git init` and the rename") is not supported: the
create initializes in a temp directory, and the reporter's case was a stray
`git init`. I corrected the PR description to that case and left the reviewed
branch's comment for a follow-up rather than rewriting a pushed, reviewed
branch. That is a known imprecision in the diff, recorded here rather than
hidden.

**Toolkit**

- Tool: plan-check in live mode, on `plan.md` and `plan-comment.md`, against
  the issue URL.
- What it produced: `accept` on both runs, with all six required checks
  passing. It checked the plan against the actual `snapshot-recovery` diff, not
  just its wording, and put the plan comment's scope at two internal functions
  plus two tests.
- Difficulties: the first run flagged that the comment stated the failure
  mechanism as plain fact, which breaks my own "separate result from
  interpretation" rule. It also noted the comment never names the issue number.
  The skill reported a procedure gap for grading a plan written after the
  build.
- What changed: I rewrote the comment to start from the code reading ("From
  reading the code, I think the failure has two parts…") and to say the failing
  runs are consistent with the mechanism rather than proof of it. I left the
  issue number out because the comment stays on that issue's thread and is never
  posted; the PR description carries `Closes #59751`.

---

## Testing Strategy

The check is the reproduction path, not the test count alone. Both new tests
build the exact failing state and assert the post-fix behavior:

- `replaces a stale store directory that has no HEAD` creates a Git repo, makes
  a store directory containing only `objects/` (no `HEAD`), and calls
  `Git.repo.create` with that store as `gitDirectory`. On `dev`'s source it
  fails with `ENOTEMPTY` and no `HEAD`; on the branch it passes with 2 expects.
- `retries repository initialization after a failed create` makes the first
  create fail and asserts that a second `capture()` retries. On `dev`'s
  `snapshot.ts` it fails at the second capture with `Received: undefined`; on
  the branch it passes with 6 expects.

The before/after method holds everything constant except the production source:
check out the branch, copy `upstream/dev`'s `git.ts` and `snapshot.ts` over it,
run, then restore. On the branch, `cd packages/core && bun test
test/git.test.ts test/snapshot.test.ts` reports 34 pass, 0 fail, and
`bun run check` (lint + typecheck) exits 0 with 35/35 tasks successful.

Honest limits: the tests drive `Git.repo.create` and `Snapshot.capture`
directly rather than the TUI prompt the issue used, and the interruption
behavior of the cache change is not separately tested. The PR says both things
in its Limitations section.

**Toolkit**

- Tool: the ship-gate, run by hand on the branch with `--base dev`, and
  pr-precheck in live mode underneath it.
- What it produced: **STOP** on the first run, then **PASS** on the final run,
  with all three manifest checks green (`bun install`, `bun run check`, the two
  touched test files) and every required pr-precheck row passing. The gate log
  at `~/.claude/ship-gate/gate-log.md` records each run.
- Difficulties: the first stop was a real one. My local `dev` ref was 32 commits
  behind the branch's fork point, so the gate diffed 295 files and 30 commits
  instead of 4 files and 2 commits, and it could not read the changed hunks. It
  also rejected because the description claimed interruption behavior that my
  test evidence said was untested.
- What changed: I fast-forwarded the local `dev` ref to `upstream/dev` (the
  three-dot diff then showed 4 files, 2 commits), added a "What this does not
  change" section and a "Limitations" section to the PR draft, and stopped
  claiming the untested interruption path. A later run also caught an
  unsupported origin story in the description, which I replaced with the
  reporter's actual case. The final run passed.

---

## Implementation Notes

**2026-10-28**

Read `git.ts` and `snapshot.ts` and found the two entry points: the rename in
`Git.repo.create` and the cached repository fiber in `snapshot.ts`. Added the
stale-store test and the remove-and-retry-once branch in `git.ts` (commit
`08f7a7dbce`). The before/after failure output quoted above was captured later,
on 2026-11-02, by swapping the production sources only.

**2026-10-28**

Raised the cache question in the thread instead of silently expanding the PR.
The reporter asked for the cache fix in the same PR if it stayed small, then
approved the design. Added `cachedInvalidateWithTTL` with failure-only
invalidation and a test that fails the first create and retries on the second
capture (commit `33f256f4ba`).

**2026-10-29**

Opened PR #60061. The branch is two commits, four files, +114/−4. The bot
reproduced the issue against both the base and the branch and posted no blocking
concerns.

**2026-11-02 to 2026-11-03**

Ran the Unit 6–8 tool phases against the real branch. Rebuilt the before/after
by swapping only the production sources, which produced the `ENOTEMPTY` and
`undefined` failures quoted in the Reproduction section. A late ship-gate run
caught an unsupported origin sentence in the description and a stale local
`dev` ref; both are recorded in the Testing Strategy block.

**Toolkit**

- Tool: `git` for the commit history and the source-swap runs, `bun test` and
  `bun run check` for the outcomes, and `gh` for the thread and PR text. No
  course skill graded this phase; repro-check and plan-check were used on the
  reproduction and the plan, not here.
- What it produced: the two commits above and the before/after outputs quoted
  in this story.
- Difficulties: `bun test` at the repo root exits on purpose, so every run has
  to start in `packages/core`; and swapping `dev`'s sources into the branch
  changes the test's expected behavior, so the restore step has to be exact.
- What changed: the second commit was added at the reporter's request; the
  original PR description's origin story was corrected after a gate run showed
  it was not supported by the code.

---

## Pull Request

**PR Link:** https://github.com/anomalyco/opencode/pull/60061

**PR Description:** The PR closes #59751 and says: `Git.repo.create` builds a
temporary repository and renames it onto the store path, handling a successful
rename and a target with `HEAD` but not a target without it; the reporter's case
was a stray empty `git init`; against such a target every later rename fails
with `ENOTEMPTY`, and a cached failed fiber replays the error until reload. It
describes the remove-and-retry-once change and the failure-only cache
invalidation, lists what does not change (the TUI path, the capture engine, the
packing schedule, the public API, damaged databases), gives the before/after
test evidence and the 34-test plus `bun run check` results, states the
limitations, and discloses AI assistance. The updated body is the one the
ship-gate passed.

**Maintainer Feedback:**

- 2026-10-27: opencodejr (bot) reproduced the issue on 2.1.4 and on `dev`
  (`fd8268e`), with a script that creates an `objects/`-only store and shows
  `ENOTEMPTY` followed by the same cached failure after the directory is
  deleted.
- 2026-10-29: Lomare (the issue reporter) asked for the cache fix in the same PR
  if it was small, and to file a separate issue if it grew. After I described
  the design, they wrote: "That design works — clearing the cache on init
  failure (and keeping it on success) is exactly what breaks the replay loop,
  and the fails-first-create/succeeds-next-capture test is the right regression
  test for the wedge. Go ahead with the single PR."
- 2026-10-29: opencodejr (bot) reviewed the PR: it ran the #59751 reproduction
  against the base and the branch from source, reported `ENOTEMPTY` on the base
  and a clean capture on the branch, confirmed both new tests fail when the old
  sources are put back, and found "No blocking concerns."
- No human maintainer review yet.

**Status:** Awaiting review.

**Toolkit**

- Tool: the ship-gate with pr-precheck in live mode, plus the voice guard on the
  PR body before it went out.
- What it produced: `PASS` with `plan-to-diff-fidelity`,
  `observable-validation`, `applicable-repo-checks`, `reviewable-diff`,
  `standards-and-comms`, and `reviewer-orientation` all passing, then the voice
  guard's `pass` on the same text.
- Difficulties: the gate is strict about the diff range and about claims the
  evidence does not support. It twice stopped on wording that overreached:
  interruption behavior that was not tested, and an origin story the code does
  not support.
- What changed: I added a limitations section, added what the PR deliberately
  does not change, replaced the unsupported origin story with the reporter's
  actual case, and added the Bun/OS conditions to the verification line. The
  final pass is on that body.

---

## Learnings and Reflections

**Technical Skills Gained**

I learned how Effect's `cached` versus `cachedInvalidateWithTTL` changes failure
semantics: the default caches a failed fiber for the scope's lifetime, which is
right for a value and wrong for an initialization that can be retried. I also
learned to read a rename as a state machine (success, target with `HEAD`, target
without `HEAD`) instead of a single call, and to test a filesystem ordering
failure by constructing the state rather than racing a real interruption.

**Challenges Overcome**

The hardest part was scope. The issue's reproduction is a TUI prompt, which is
slow and noisy; finding the two smaller entry points made the work testable. The
second hard part was honesty under tool pressure: repro-check rejected my first
claim because it said "I can reproduce" before I had my own evidence, and the
ship-gate rejected a description that claimed interruption behavior the tests do
not cover. Both were right, and both changed what I wrote rather than just how I
phrased it.

**What I'd Do Differently Next Time**

I would run the tool phases before opening the PR rather than after, so the plan
and the PR description are written against live evidence instead of
reconstructed. I would also check the local base ref before running a diff-based
gate; the 295-file diff was my stale `dev`, not the branch. And I would write the
code comment from the reporter's actual case the first time, instead of
inferring an origin the code does not support.

---

## Resources Used

- The issue thread, especially opencodejr's reproduction script and Lomare's
  severity note about the corrupted database:
  https://github.com/anomalyco/opencode/issues/59751
- opencode's `CONTRIBUTING.md` and `AGENTS.md`, for the branch, test, and PR
  conventions:
  https://github.com/anomalyco/opencode/blob/dev/CONTRIBUTING.md
- The Effect source for `cachedInvalidateWithTTL` in the repo's installed
  `effect` package, to understand when invalidation runs.
- The course's Unit 6 and Unit 7 install guides, for the voice guard, the wild
  scopes, and the ship-gate manifest.

