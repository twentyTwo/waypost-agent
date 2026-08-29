# Routines Setup

Routines are configured in the **Claude Code UI / settings**, not in this
repo. This file is the spec for what to create. One Routine per automated
role. Each Routine's task should begin by following the session-start
checklist in [`/CLAUDE.md`](../CLAUDE.md).

> The `agent-question` template is a classic Markdown template, so "Role"
> is a text field, not a true dropdown. If you want an enforced dropdown,
> convert it to a GitHub **Issue Form** (`.github/ISSUE_TEMPLATE/
> agent-question.yml` with a `dropdown` element). Behavior is otherwise
> identical; the `needs-human` label still auto-applies.

---

## 1. Build agent Routine

- **Trigger:** a comment is added to an open issue that has the
  `needs-human` label (i.e. any reply on an `agent-question` issue).
  - Simplest workable form: "new comment on any open issue labeled
    `needs-human`". The task step then reads the issue's Role field and
    proceeds only if Role = `build` (or handles whichever role matches —
    see note below).
- **Task:** load `skills/build-agent/SKILL.md`, run the resume protocol
  (`docs/resume-protocol.md` Part C).

## 2. Automated-tester Routine

- **Trigger:** a PR is labeled `ready-for-test` (label-added event on
  pull requests targeting `main`).
- **Task:** load `skills/automated-tester/SKILL.md`, run the tests,
  write both reports to `/qa-runs/`, comment on the PR, open a `bug`
  issue on any failure.
- **Also** reuse this Routine (or a sibling) for the "comment on
  `needs-human` issue where Role = `auto-test`" resume case.

## 3. Marketing Routine

- **Trigger (best available):** a PR is merged to `main`.
- **Compound condition that can't live in the trigger:** marketing should
  only run when the PR is merged **AND** both
  `<date>-<feature>-{auto,manual}.md` exist in `/qa-runs/` **AND** every
  test case in both is `Pass`.
  - **Follow-up decision for the owner — do not guess a workaround:**
    implement this as a lightweight check at the very start of the
    Routine's task (read `/qa-runs/`, parse the two `.md` reports, exit
    quietly if the condition isn't met), *or* introduce an explicit
    `ready-for-marketing` label/gate that something else sets. Flagging
    this here rather than picking one.
- **Task:** load `skills/marketing-agent/SKILL.md`.

## 4. Manual tester — NOT a Routine

The manual tester runs in **Claude Cowork** with computer use on a VM.
It is invoked via Cowork **Dispatch / Scheduled Task**, not a GitHub
Routine. Trigger it manually (or on a schedule) when a feature is ready
for human-style testing, pointing it at the feature PR. Skill:
[`/skills/manual-tester/SKILL.md`](../skills/manual-tester/SKILL.md).

---

## Routing note

If per-role triggers on the same "comment on `needs-human` issue" event
are awkward in the UI, use **one resume Routine** whose task reads the
issue's Role field first and loads the matching `skills/<role>/SKILL.md`.
Fewer Routines, one dispatch point.

## Cross-cutting requirements

- Every Routine needs repo read/write (commit to branches, open issues,
  comment on PRs) and, for the tester, Playwright MCP.
- Routines must be able to open issues from the template and apply labels
  (`needs-human`, `bug`, `ready-for-test`).
