# Resume Protocol — ask-and-pause, then resume

The pipeline assumes the human-in-the-loop is often away. Sessions never
wait. This is the exact mechanic every role follows.

## Part A — an agent gets blocked (ask-and-pause)

A "block" is a decision only the repo owner can make: product behavior,
stack choice, an external service, ambiguous spec, "bug or intended?".
A normal bug is **not** a block — report it and keep going.

When blocked:

1. **Write state to `STATUS.md`.** Update only your role's block:
   - `Current task` — what you're mid-way through
   - `Last completed step` — the last thing that is actually done
   - `State / branch` — branch, PR number, file paths
   - `Open questions` — will be `#<issue>` once step 2 is done
   - `Blocked: yes`
   - `Updated` — ISO 8601 UTC + your role
2. **Open a GitHub Issue** from `.github/ISSUE_TEMPLATE/agent-question.md`.
   Fill every field. The template auto-applies the `needs-human` label.
   Put the issue number back into your `STATUS.md` `Open questions` line.
3. **Commit** the `STATUS.md` change.
4. **End the session.** No polling, no sleeping, no waiting loop. Stop.

## Part B — the owner replies

The owner comments on the issue whenever they are next available. That's
the only thing they have to do. They may also add labels or edit
`STATUS.md`, but a comment is enough.

## Part C — a fresh session resumes (Routine-triggered)

A Routine (configured in the Claude Code UI — see
[`routines-setup.md`](routines-setup.md)) listens for the GitHub event
(a comment on an open `agent-question` / `needs-human` issue) and starts
a **new** session with the matching role's skill.

**No session is ever kept alive. Every resume is a cold start that
rebuilds context from this repo.**

The fresh session's first actions, in order:

1. Read [`/CLAUDE.md`](../CLAUDE.md).
2. Read [`/STATUS.md`](../STATUS.md) — locate the role block with
   `Blocked: yes` and the `#<issue>` reference.
3. Read that **issue thread** in full — the original question and the
   owner's answer.
4. Read the role's `SKILL.md`.
5. Check out the branch / PR named in `State / branch`.
6. Reconstruct context from `Last completed step` + the diff on that
   branch. Do not redo completed work.
7. Apply the owner's decision and continue.
8. Update `STATUS.md`: `Blocked: no`, clear/So-note `Open questions`,
   new `Last completed step`, new timestamp.
9. Close the issue (or leave a comment saying it's resolved and let the
   owner close it — follow whatever convention the repo settles on).

## Invariants

- State lives in the repo, never in a running process.
- `STATUS.md` is the index; issues hold the detail.
- One open `agent-question` issue per role at a time. If you'd open a
  second, you're probably not actually blocked — or the first needs a
  nudge, not a duplicate.
