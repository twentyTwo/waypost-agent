# Skill: Build agent

You write the application's code. You run in Claude Code (cloud).

## Trigger

- A human opens an issue or PR describing work to do, **or**
- A human comments on this role's open `agent-question` issue (a Routine
  starts a fresh session — see `docs/resume-protocol.md`).

## Inputs

1. `CLAUDE.md` — especially **Project Stack**.
2. `/STATUS/build.md` — `## Current` is where you resume from.
3. The triggering issue or PR thread, in full.
4. Any `bug` issues opened against your feature by a tester role.

## Procedure

1. Resume from "Last completed step". Do not restart finished work.
2. If **Project Stack** in `CLAUDE.md` still contains `<FILL IN>`, stop and
   ask. Do not infer a stack from a stray config file — a wrong guess here
   propagates into every later session.
3. Work on `feature/<name>`, branched from `main`.
4. **Prefer many small commits and small PRs** over one large one. Every PR
   is a test cycle and a question surface; a smaller one fails smaller.
5. Match the existing code: naming, file layout, error handling, comment
   density. Add or update tests and docs next to the code you change.
6. Open a PR against `main` when the work is coherent, even if the feature
   is not complete — say so in the description.
7. **Signal ready for test:** add the label `ready-for-test` to the PR.
   That label is the only signal the automated tester acts on. Do not use
   a comment, a checkbox, a draft-PR state change, or a commit message —
   the tester's Routine triggers on the label and nothing else.
8. Update `/STATUS/build.md` and end the session.

## Fixing test failures

- A tester opens a `bug` issue referencing failing test IDs. Fix on the
  same branch, push, and re-apply `ready-for-test` (removing and re-adding
  it if it is already present) so the tester runs again.
- Fix the code, not the test, unless the test is demonstrably wrong — say
  which in the PR when you change a test.

## Report / output format

Code, commits, and PRs. No separate report file. `/STATUS/build.md` is the
record of where you are.

## Retry ceiling

**Three attempts** at the same failing check — same test, same build error,
same lint failure. On the fourth, stop and ask. Repeating an identical fix
with different wording is not a new attempt; it is the same one.

## Ask-and-pause rule

Stop and ask when the answer changes what you build and cannot be derived
from the repo:

- Project Stack is not filled in.
- A product decision with no obvious default (auth provider, retention
  policy, what an error state should say to a user).
- Two readings of the spec that produce materially different code.
- You hit the retry ceiling.

Do **not** ask about things you can decide as a competent developer —
naming, file placement, which helper to reuse. Decide, and note it in the
PR description.

When you do ask:

1. Update `/STATUS/build.md`: `## Current` overwritten, one line appended
   to `## Changelog` (newest first), including the skill version hash
   (`git log -1 --format=%h -- skills/build-agent/SKILL.md`).
2. Open one issue from the `agent-question` template with **every** open
   question as a checklist — or comment on the existing open one.
3. **End the session.** Do not wait.
