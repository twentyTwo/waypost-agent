# Routines setup

One Routine per role, each triggered by a GitHub event. Configured in the
**Claude Code UI**, not in this repo.

> **Limitation, stated plainly:** Routine configuration is not repo config.
> It cannot be committed, reviewed, versioned, or copied by "Use this
> template". Every adopter wires these by hand, and a change to a trigger
> leaves no trace in git. Record any non-obvious choice here in this file
> so the repo at least documents what the UI is set to.

## Build agent

- **Trigger:** comment added to an open issue labeled `needs-human` where
  the Role field is `build`.
- **Task:** follow `skills/build-agent/SKILL.md`, starting from the session
  checklist in `CLAUDE.md`.
- **Access:** repo write, issues read/write, pull requests read/write.

## Automated tester

- **Trigger:** pull request labeled `ready-for-test`.
- **Task:** follow `skills/automated-tester/SKILL.md`.
- **Access:** repo write (to commit reports to `/qa-runs/`), issues
  read/write (to open `bug` issues), PR read/write (to comment).
- **Also needs:** the Playwright MCP.

## Manual tester — not a Routine

Runs in Cowork with computer use. Invoke it via **Dispatch** or a
**Scheduled Task** pointing at `skills/manual-tester/SKILL.md`.

The VM must stay awake for the length of a run — a sleeping VM produces a
half-finished report with no error, which is worse than no report.

## Compound conditions are not expressible

Some triggers you actually want are not single GitHub events. The clearest
case: manual test should run when the PR is labeled `ready-for-test` **and**
the automated report exists **and** it is all `Pass`.

GitHub gives you one event. So:

**Put the rest of the condition in the first step of the task, not in the
trigger.** Each affected SKILL.md opens with a verification step that
checks the remaining conditions and ends the session if they do not hold.
Never assume the trigger firing means the full condition was met.

This applies to any role you add later. When a new role's real trigger is
compound, split it: the cheapest event goes in the trigger, everything else
goes in step 1 of the procedure.

## Adding a Routine for a new role

See `adding-a-new-role.md`. Add a section here at the same time — this file
is the only record of what the UI is configured to do.
