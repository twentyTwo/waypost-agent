# Getting started

The exact steps to run a project on Waypost, from nothing to a merged,
twice-tested feature.

[`SETUP.md`](../SETUP.md) is the same setup as a bare checklist. This file
is the walkthrough, with the reasoning and the commands.

**Time:** about 20 minutes, once. Roughly 15 of that is clicking in a UI.

---

## 1. Create the project

```bash
gh repo create my-app --template twentyTwo/waypost-agent --private --clone
cd my-app
```

Name it after your application, not after the pipeline.

If your application already exists, create the repo anyway and copy your
code in. Waypost expects the app and the coordination files to live in one
repo, so an agent reads both in a single session.

## 2. Fill in the project stack

Open `CLAUDE.md` and replace every `<FILL IN>` in the **Project Stack**
block:

```
Application name:    My App
Language / runtime:  TypeScript / Node 22
Framework:           Next.js 15 (App Router)
Package manager:     pnpm
Test runner:         Playwright + Vitest
Deploy target:       Vercel
Branch model:        trunk = main; feature branches feature/<name>
```

Do this before anything runs. The build agent is instructed to stop and ask
rather than guess a stack — a wrong guess here propagates into every later
session. Leave it blank and your first run produces a question instead of
code, which costs you a day if you happen to be away when it lands.

## 3. Create the labels

Labels are how roles signal each other. Every trigger in the system reads
one; none of this is decoration.

```bash
gh label create ready-to-build --color 5319E7 --description "Approved for the build agent to start"
gh label create needs-human    --color B60205 --description "Agent blocked, needs owner decision"
gh label create ready-for-test --color 0E8A16 --description "PR ready for the test agents"
gh label create bug            --color D93F0B --description "Failing test / defect"
```

`needs-human` must exist before the first agent question — the issue
template applies it automatically and cannot create it.

## 4. Clear the example state, then commit

Each file in `/STATUS/` ships with a placeholder block and one example
changelog line. Delete both, in all three files.

```bash
# edit STATUS/build.md, STATUS/auto-test.md, STATUS/manual-test.md
git add -A && git commit -m "Set up Waypost" && git push
```

Skipping this is the most common cause of an agent redoing finished work. A
fresh session reads these files as ground truth and has no way to tell a
placeholder from live state.

Also turn off **Template repository** in the repo settings — your project is
a project now.

## 5. Wire the Routines

**In the Claude Code UI.** Routines are not repo config; nothing in this
template can create them for you. Full detail in
[`routines-setup.md`](routines-setup.md).

| Routine | Trigger | Task |
|---------|---------|------|
| Build — new work | issue labeled `ready-to-build` | follow `skills/build-agent/SKILL.md` |
| Build — resume | comment on an open `needs-human` issue where Role is `build` | same skill |
| Automated tester | PR labeled `ready-for-test` | follow `skills/automated-tester/SKILL.md` |

All three need repo write plus issue and PR read/write. The tester also
needs the **Playwright MCP**.

Wire **both** build Routines. They are different entry points — one starts a
feature, the other resumes a paused one — and configuring only the resume
Routine is the easy mistake. New work then never starts.

If you would rather not automate the first leg, skip the "new work" Routine
and start those sessions by hand. The resume Routine is the one that cannot
be replaced that way, because it fires while you are away.

## 6. Pair Cowork for the manual tester

**Also UI.** The manual tester is not a Routine. It runs in Claude Cowork
with computer use, on a VM. Invoke it with Dispatch or a Scheduled Task
pointing at `skills/manual-tester/SKILL.md`.

Confirm the VM will not sleep during a run. A machine that sleeps mid-run
produces a half-finished report with no error in it, which is worse than no
report at all.

## 7. Ship your first feature

```bash
gh issue create --title "Add login form" --body "..."
gh issue edit 1 --add-label ready-to-build
```

Write the issue for a competent developer who has never seen the product,
and state the acceptance criteria explicitly — the automated tester derives
its test plan from this issue, not from the code. Vague criteria produce a
vague test plan.

The `ready-to-build` label is the starting gun. An unlabelled issue is just
a note to yourself.

---

## The loop, from then on

1. You label an issue `ready-to-build`.
2. **Build agent** works on `feature/<name>` in small commits, opens a PR,
   labels it `ready-for-test`, updates `/STATUS/build.md`, ends its session.
3. **Automated tester** fires on that label. Drives Playwright, writes
   `qa-runs/<date>-<feature>-auto.xlsx` and a matching diffable `.md`,
   comments the pass/fail counts on the PR, opens a `bug` issue for any
   failure.
4. You dispatch the **manual tester** once the automated report is all Pass.
   Same test IDs, same report format, `-manual` instead of `-auto`.
5. Failures become `bug` issues. The build agent fixes them on the same
   branch and re-applies `ready-for-test` — removing and re-adding the label
   if it is still there, because the trigger fires on the labelling event,
   not on the label existing.
6. Both reports all Pass → **you** merge. Waypost does not merge for you;
   "both passed" is a state it reports, not an action it takes.

The automated tester never signs off alone. The build agent shaped both the
code and the tests, so a green suite can share the author's blind spot — its
PR comment says "auto-test: 12 pass, 0 fail", never "ready to merge".

## When an agent needs you

This is the part everything else is built around, and none of it is
time-sensitive.

A blocked agent writes its state to `/STATUS/<role>.md`, opens one
`needs-human` issue with **every** question from that session as a
checklist, and ends its session. It does not poll, sleep, or hold a turn
open. You get one notification with a batch of questions rather than five
with one each.

You reply whenever — minutes later or next Tuesday. Your comment fires the
resume Routine, which starts a **brand new** session. It reads `CLAUDE.md`,
then the status file, then the issue thread, then its own skill, and
continues from the last completed step.

Nothing carries over except the repository. Full detail in
[`resume-protocol.md`](resume-protocol.md).

## What your job actually is

1. Write issues, and label the ones you want built.
2. Answer `needs-human` questions when convenient.
3. Dispatch the manual tester when the automated report is green.
4. Merge when both reports pass.

---

## Troubleshooting

Ordered by how often each one actually happens.

**I labelled an issue and nothing happened.** Usually the label was
`ready-for-test` rather than `ready-to-build`, or the **Build — new work**
Routine was never wired. Opening an issue is deliberately not a trigger —
otherwise every bug report and discussion thread would spawn a build
session.

**The build agent immediately asked me about the stack.** `CLAUDE.md` still
contains `<FILL IN>`. It is behaving correctly. Fill it in, then comment on
the issue to resume.

**The automated tester never ran on my PR.** The Routine fires on the
labelling *event*, not on the label being present. If `ready-for-test` was
already applied, remove it and add it again.

**The manual test report stops halfway with no error.** The VM slept
mid-run. Check the power settings on the Cowork machine — this failure is
silent, which is why it is worth checking first.

**An agent redid work that was already finished.** The example content in
`/STATUS/` was never cleared, so a session read a placeholder as live state.
That is step 4, and it is the one people skip.

**Two agents overwrote each other's status.** They should not be able to.
Each role writes only its own file in `/STATUS/`; that separation exists
precisely to make concurrent status writes impossible. If it happened, a
role is writing outside its lane — check that skill.

---

## Where everything lives

| Path | What it is | Who reads it |
|------|------------|--------------|
| `CLAUDE.md` | Orientation, the no-blocking rule, role table, project stack | Every session, first |
| `SETUP.md` | The one-time checklist | You, once |
| `STATUS/<role>.md` | Live state and an append-only changelog, one file per role | That role; you, catching up |
| `skills/<role>/SKILL.md` | How that role works | That role |
| `qa-runs/` | Test reports, `.xlsx` plus diffable `.md` | Both testers, and you |
| `docs/` | Architecture, resume protocol, Routine wiring, adding roles | You |

Adding a fourth role — a docs writer, a release manager — is five steps and
touches no shared logic, because nothing is coded against a fixed set of
roles. The table in `CLAUDE.md` is the entire registry. See
[`adding-a-new-role.md`](adding-a-new-role.md).
