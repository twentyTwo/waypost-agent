# CLAUDE.md — read this first, every session

This repository is the **shared memory and coordination surface** for a set
of AI agent roles working on one software project. Agents do not talk to
each other directly and do not share a process. Everything one agent needs
to know about another's work is a file in this repo. A session that ends —
for any reason — loses nothing, because nothing lived only in its context.

## The hard rule: agents never block waiting for a human

The repo owner is often unavailable for hours or days. **No session ever
waits.** If you are blocked on something only a human can decide:

1. Write your state to `/STATUS/<your-role>.md` — update `## Current`,
   append one line to `## Changelog`.
2. Open an issue from the `agent-question` template
   (`.github/ISSUE_TEMPLATE/agent-question.md`), batching every open
   question you accumulated this session into one checklist.
3. **End the session.** Do not poll, sleep, retry, or keep a turn alive
   waiting for a reply.

When the human answers, a Routine starts a **fresh** session that rebuilds
context from this repo. Exact mechanics: [`docs/resume-protocol.md`](docs/resume-protocol.md).

## Session start — mandatory, every role

1. Read this file.
2. Read `/STATUS/<your-role>.md`. That is your ground truth for what is in
   flight, not your memory of it.
3. If a GitHub event resumed you, read that issue or PR thread now.
4. Read your role's skill (table below).
5. Continue from "Last completed step". Never redo finished work.

## Roles

| Role | Skill | Status file | Trigger |
|------|-------|-------------|---------|
| Build agent | [`skills/build-agent/SKILL.md`](skills/build-agent/SKILL.md) | [`STATUS/build.md`](STATUS/build.md) | Human issue/PR; comment on its `agent-question` issue |
| Automated tester | [`skills/automated-tester/SKILL.md`](skills/automated-tester/SKILL.md) | [`STATUS/auto-test.md`](STATUS/auto-test.md) | PR labeled `ready-for-test` |
| Manual tester | [`skills/manual-tester/SKILL.md`](skills/manual-tester/SKILL.md) | [`STATUS/manual-test.md`](STATUS/manual-test.md) | Cowork Dispatch / Scheduled Task (not a Routine) |

This table is the registry. Adding a role means adding a row — nothing in
this framework is hardcoded to these three. See
[`docs/adding-a-new-role.md`](docs/adding-a-new-role.md).

## Project Stack

<!-- FILL IN: language, framework, package manager, test runner, deploy
     target, and any coding conventions specific to this project. Until
     this section is filled in, agents that need it must ask via
     ask-and-pause rather than guessing. Delete this comment when done. -->

```
Application name:    <FILL IN>
Language / runtime:  <FILL IN>
Framework:           <FILL IN>
Package manager:     <FILL IN>
Test runner:         <FILL IN>   (Playwright assumed for web E2E)
Deploy target:       <FILL IN>
Branch model:        trunk = main; feature branches feature/<name>
```

## Conventions that apply to every role

- **Batch questions.** One issue per session with a checklist, not one
  issue per ambiguity.
- **Retry ceiling.** Three attempts at the same failing thing, then stop
  and ask. Repeated identical retries are not progress.
- **Record the skill version.** When you write to your status file, include
  the commit hash of the SKILL.md you followed:
  `git log -1 --format=%h -- skills/<role>/SKILL.md`. A later session can
  then tell whether the rules changed under it.
- **Reports are named** `<date>-<feature>-<role>.<ext>`, consistently,
  whatever the role.

## Docs

- [`docs/pipeline-architecture.md`](docs/pipeline-architecture.md) — the whole system, with a diagram
- [`docs/resume-protocol.md`](docs/resume-protocol.md) — ask-and-pause and resume, step by step
- [`docs/routines-setup.md`](docs/routines-setup.md) — wiring the triggers (manual, in the UI)
- [`docs/adding-a-new-role.md`](docs/adding-a-new-role.md) — extending the framework
