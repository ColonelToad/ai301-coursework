# Toolkit Portfolio

Path: `beat-3-portfolio/toolkit-portfolio.md`

**Student:** Christopher Onyiuke

## Featured Tools

I picked these four because they are the ones that showed change; each started from a course starter and ended somewhere different, with evidence. issue-scout changed three times before it stopped recommending issues I was not allowed to take. repro-check rejected my own claim before it accepted my report. The ship-gate stopped my branch twice and passed it twice, and each time it was right. The pr-monitor changed the least, so its entry is about the playbook I wrote rather than a repair I made to the tool.

**issue-scout**

**What it is, and the judgment hole I wrote:** A rubric-driven selector for a
first issue. The tool fetches and grades; the judgment sits in three files: `scope.md` is the fit profile (languages, repo size, what I will and will not take), `search-strategy.md` is nine queries plus the ranking order and `rubric.md` is seven checks and the verdict rule that turns them into accept/reject.

**How it evolved, dated:** There were four changes, each driven by a scan that came back
wrong.

- 2026-10-31, before spending on a scan: a dry run of the five broad queries
  returned 494 unique issues and **no** repository on my watchlist. I added four
  repo-scoped queries and a beginner-label ordering.
- 2026-10-31, after the first scan: the whole top five were LLVM `good first
  issue` issues, and LLVM's AI policy bans AI-assisted work on exactly that
  label. I changed the LLVM query to `help wanted` and dropped
  `llvm/llvm-project` `good first issue` results in ranking.
- 2026-10-31, after the second scan: it failed llvm#174064 on
  `contribution_policy` even though the issue is `help wanted`, and the scout
  admitted it had not read the policy file. I read it: the ban is label-specific.
  The check became label-aware — a repo-wide ban still fails, a label-specific
  ban fails only when the issue carries that label.
- 2026-11-02: I replaced `bounded_scope` with `scope-feasible` plus a difficulty
  note, because the old row rejected real issues for naming several files or
  carrying an open design edge.

**Evidence:** the first scan's summary, from `scan-output.md`:

> All five top-ranked candidates were llvm/llvm-project issues … All five
> reject, on the same check: contribution_policy.

The second scan, after the strategy patch, returned 621 unique issues (151
dropped, 470 left) and its top five were `wasmtime#5879`, `polars#29641`,
`llvm#174064`, `polars#19266`, `polars#21898` — one accept, four rejects on
`available`. The 2026-11-02 re-grade of the issue I actually took shows the new
rows working on a real case:

> opencode #59751 … fails `available`. … The contribution policy only warns
> that long AI-generated PR or issue text "may be ignored", which is a condition
> rather than a ban. … It would have been moderate difficulty.

**repro-check**

**What it is, and the judgment hole I wrote:** it grades a reproduction package —
a claim comment and a report — against the issue it points at, and returns
accept or reject with a row per check. My judgment lives in `rubric.md` (five
required checks, including `followable-trigger` and `issue-matched-proof`) and
`procedure.md`. In unit 6 I swapped its classroom `scope.md` for the wild one,
so it could grade a real repo instead of the sandbox.

**How it evolved, dated:** the tool did not change much; what changed was what
it forced me to write.

- 2026-11-02: the first run rejected my already-posted claim on
  `outcome-honesty`. It said "I can reproduce the wedge" while quoting no
  environment, command, or output of my own. The grade was right.
- 2026-11-02: the revised claim passed `outcome-honesty` but rejected on
  `repo-aware-comms`, because the live claim already exists in the thread.
- 2026-11-02: the paired run (claim plus report) rejected on
  `followable-trigger` and `issue-matched-proof`: the branch was only on my fork,
  `upstream/dev` was never added, and the cache half had no output of its own.
- 2026-11-02: after adding the fork and remote steps and a second before/after
  block, the pair graded `accept` with all five required checks passing.

**Evidence:** the rejection was specific enough to act on. The reason it gave,
as I recorded it at the time, was that step 1 told a reader to `git switch
snapshot-recovery`, but that branch was only on my fork (PR #60061), so a
stranger could not find it, and step 3 used `upstream/dev` without saying how to
add that remote.

The substance the accept rested on is the before/after pair from the real
branch. On `dev`'s `git.ts` the stale-store test fails with `ENOTEMPTY`, and on
`dev`'s `snapshot.ts` the retry test fails at the second capture with
`Received: undefined`. With the branch's sources, `cd packages/core && bun test
test/git.test.ts test/snapshot.test.ts` reports `34 pass / 0 fail`.

**ship-gate**

**What it is, and the judgment hole I wrote:** a `PreToolUse` hook on `gh pr
create`. It runs the checks in my `ship-manifest.md` against the repo, then runs
pr-precheck in live mode on the branch. The judgment is the manifest: which
checks must pass before a PR leaves my hands. Mine has three rows, each with the
repo file it came from — `bun install`, `bun run check`, and
`cd packages/core && bun test test/git.test.ts test/snapshot.test.ts`.

**How it evolved, dated:** one install fix and four verdicts on one branch.

- 2026-11-02: installed, then repointed the hook through `wsl.exe`. The generated
  command carried a `/mnt/c/...` path, which Windows cannot execute; the same
  problem the voice guard had.
- 2026-11-03 20:30: **BLOCK.** My local `dev` ref was 32 commits behind the
  branch's fork point, so the gate diffed 295 files and 30 commits instead of 4
  files and 2 commits, and could not read the changed hunks. The description
  also claimed interruption behavior my tests did not cover.
- 2026-11-03 20:32: **PASS** after I fast-forwarded `dev` to `upstream/dev` and
  added a Limitations section and a "what this does not change" list.
- 2026-11-03 20:34: **BLOCK** again. The pass had let through a sentence saying
  the store was left by "a create that stopped after `git init`", but `git.ts`
  initializes in a temporary directory, so that origin was not supported.
- 2026-11-03 20:36: **PASS** after I replaced it with the reporter's actual case
  (a stray `git init`) and added the OS and Bun conditions to the verification
  line.

**Evidence:** the first stop, from `~/.claude/ship-gate/gate-log.md`:

> The `plan-to-diff-fidelity` row came back unclear: "Diff truncated before
> git.ts/snapshot.ts hunks (stat only), and the 295-file/30-commit range against
> stale local dev hides the PR's real range."

The final pass, same log, `head 33f256f4ba` and `body 33d690f9c8af`, passed
`reviewable-diff` with "4 files, 114 insertions, 4 deletions, two focused
commits, no debris or unrelated hunks" and `standards-and-comms` with
"Conventional title, `Closes #59751`, template sections answered, own-words AI
disclosure, honest Limitations section."

**pr-monitor**

**What it is, and the judgment hole I wrote:** an on-demand runner that reads a
live PR's state and drafts the next move — answer the review, nudge, or wait. It
has no hook, because it is invoked rather than latched. My judgment is
`playbook.md`, four sections: response rules, staleness thresholds, nudge
etiquette, walk-away criteria. The monitor folds that file into every draft, so
a nudge with no threshold behind it would be the machine's judgment, not mine.

**How it evolved, dated:** the tool itself I did not modify. The dated work is
the playbook and the two runs.

- 2026-11-03 20:36: installed as a folder copy and ran `--state`. It found
  #60061 on the first try: 6 days old, 5 checks passing, `MERGEABLE` with merge
  state `BLOCKED`, one thread item — the bot review.
- 2026-11-03: wrote the thresholds: a 7-day human clock for this repo, a 3-day
  floor when a human asks me a direct question, one nudge per PR ever, and a
  30-day walk-away after an unanswered nudge. A bot comment does not reset the
  human clock.
- 2026-11-03 20:37: the full run drafted **"No reply and no nudge. Wait."**

**Evidence:** from `drafted-move.md`, the reason the draft gives for doing
nothing:

> The only review is from the bot (`opencodejr`). It reports a successful
> reproduction, says "No blocking concerns", and asks nothing, so there is no
> feedback to answer. A thank-you reply would add noise, not information.

And the playbook line the monitor cited as the rule driving it:

> **Playbook rule driving this:** "Activity by a bot does not reset the human
> clock. A bot comment that asks nothing does not make the thread fresh."

## Inventory

- **issue-select** (unit 1): the first selector, superseded by issue-scout; its
  rubric and evidence guide were carried into the new skill, and its
  `eval-run.txt` is still the record of the unit-1 run.
- **plan-check** (unit 3, re-scoped in unit 6): grades a plan and its comment
  against the issue; accepted both runs of my opencode plan with all six
  required checks, after catching a comment that stated the mechanism as fact.
- **pr-precheck** (unit 4, re-scoped by the unit-7 install): the six-row PR
  rubric that now runs inside the ship-gate; its rows produced the four
  STOP/PASS verdicts above.
- **voice-guard** (unit 6): a `PreToolUse` hook over `check_voice.py` and one
  canonical `voice-guide.md`; `guard-log.md` records every stop and pass,
  including the 2026-11-03 stop on a real PR draft line.
- **CLAUDE.local.md** (unit 6): repo memory for the opencode clone — conventions,
  subsystem map, build and test commands, AI policy; kept out of the diff through
  `.git/info/exclude`.
- **The unit 1–4 eval harness**: not a tool I extend, but the thing that produced
  the saved `eval-run.txt` files; it ran each unit's skill against the sandbox
  sets and recorded the output I graded myself on.

## The Arc

**Beat 1**
Date: 2026-10-31
Friction: Before spending on a scan I dry-ran the five broad queries; they returned 494 unique issues and not one repository from my watchlist, with week-old zero-comment repos at the top.
Change: Added four repo-scoped queries (llvm, wasmtime, polars, verilator) and a beginner-label ordering before the first paid scan.
Result: The new top five were all real, active issues in watchlist repos: llvm#236445, llvm#184672, wasmtime#5879, llvm#236470, polars#29641.

**Beat 2**
Date: 2026-10-31
Friction: The first full scan's entire top five were LLVM `good first issue` issues, and every one rejected on `contribution_policy`, because LLVM's AI policy bans AI-assisted work on exactly that label; a maintainer had said so on #235270's own thread.
Change: Changed the LLVM query from `good first issue` to `help wanted` and added a ranking drop for `llvm/llvm-project` `good first issue` results, keeping the repo for labels the policy does not name.
Result: The second scan graded a mixed top five and produced one accept, wasmtime#5879, and four rejects on `available` from open linked PRs.

**Beat 3**
Date: 2026-10-31
Friction: The second scan failed llvm#174064 on `contribution_policy` even though it carries `help wanted`, and the scout admitted it had not read the policy file.
Change: Read the policy: the ban is specifically on `good first issue`. Made the check label-aware, so a repo-wide ban fails but a label-specific one fails only when the issue carries that label.
Result: #174064 still rejects, but on `available` and scope rather than on a misread policy, so the failing check finally names the real problem.

**Beat 4**
Date: 2026-11-02
Friction: The scout rejected real wild issues for naming several files or carrying an open design edge, and its policy read still could not tell a repo-wide ban from a label-specific one.
Change: Replaced `bounded_scope` with `scope-feasible`, which fails only when there is no single piece of work to start, and rewrote `contribution_policy` to read the linked policy itself and separate agent rules from contributor bans.
Result: The re-grade of opencode #59751 reports moderate difficulty and the correct `available` reject, and wasmtime#5879 now fails on the repo-wide AI ban written in its `AGENTS.md` plus scope, instead of on the beginner label.

**Beat 5**
Date: 2026-11-02
Friction: The voice guard would not run on this machine: symlinks created on `/mnt/c` are unreadable from Windows, and Windows Python could not launch `claude.cmd`.
Change: Hardlinked one canonical `voice-guide.md` into each skill instead of symlinking it, and routed the hook through `wsl.exe` with the script path inside a quoted `bash -c`.
Result: The guard stopped a bad draft with the exact rule quoted and passed a good one; on 2026-11-03 at 20:33 it stopped a real PR draft line for stating a suggestion as a fact.

**Beat 6**
Date: 2026-11-02
Friction: repro-check rejected my claim (I said "I can reproduce" while quoting nothing of my own), then rejected the report (a stranger could not reach the branch, and the cache half had no output).
Change: Made the claim prospective, added fork clone and `upstream` remote steps, and gave each half its own before/after block with expected versus actual.
Result: The paired package graded `accept` with all five required checks passing, on 2026-11-02.

**Beat 7**
Date: 2026-11-03
Friction: The ship-gate's first branch run diffed 295 files because my local `dev` ref was stale, and rejected a description that claimed interruption behavior the tests did not cover.
Change: Fast-forwarded `dev` to `upstream/dev`, added a Limitations section and a list of what the PR does not change.
Result: The 20:32 run passed all six rows; a later stop exposed a wrong origin story in the body, and after that correction the 20:36 run passed again with `head 33f256f4ba`.

**Beat 8**
Date: 2026-11-03
Friction: None met. The first `--state` read and the first full run both worked, and the only thread item was the bot's review, which asks nothing.
Change: Wrote the four-section playbook with explicit thresholds: 7 days before a nudge, 3 days for a direct question, one nudge ever, 30 days to walk away.
Result: The full run drafted "No reply and no nudge. Wait." and named 2026-11-05 as the first day a nudge would be allowed.

## Limitations and What's Next

The toolkit grades artifacts, not behavior. Every skill here reads what it is
shown — a comment, a report, a diff, a PR body — and none of them can watch me
work. That is why the strong verdicts cluster around writing: repro-check and
the ship-gate both caught sentences I could not defend, and neither would have
caught a claim I never wrote down. The loop also still depends on me reading the
output. The monitor drafts a move and stops; the gate stops a push and waits. On
a week when I did not read the logs, all four tools would have been silent.

What I would build next, in order: a check in issue-scout that reads linked PRs
before ranking, because `gh search issues` cannot see them and that gap spent
four of my five grades on taken issues; a way for the ship-gate to compute its
diff against `upstream/dev` rather than whatever local ref happens to exist,
since the 295-file stop was self-inflicted and cost a whole run; and a
pre-claim version of repro-check, so the claim is graded before it is posted
rather than after. Past that, the honest next step is not another tool: it is a
second contribution where I run the phases in order, instead of
retrofitting them onto a PR that was already open.

**Tool limits.**

- **issue-scout:** `available` cannot see linked PRs from `gh search issues`, so a good ranking still spends grades on issues that are already taken, and the AI-policy read is only as good as the policy file it finds.
- **repro-check:** it grades the package's words, not the machine that ran them, so quoted output is taken as evidence; live mode also reads a re-graded posted claim as a competing claim unless the draft matches the thread.
- **ship-gate:** its diff comes from the local base ref, so a stale `dev` produces a misleading range; it only runs what the manifest names, and its voice-guide check expects a symlink that cannot be read from Windows on this setup.
- **pr-monitor:** it finds the PR by the branch you are standing on and the repo `gh` resolves, so a fork-only clone reports `NO PR YET`, it cannot see uncommitted work, and its draft still needs your rewrite before it can ship.
