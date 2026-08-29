# CLAUDE.md — Read this first

This repo is the **shared memory and coordination hub** for a multi-agent
development pipeline. Four agent roles use it. They are the same Claude
Code / Cowork engine loaded with a different Skill depending on what
triggered the session.

## The one rule: never block on a human

The human-in-the-loop (repo owner) is often unavailable. **No session
ever waits for a human.** If you are blocked on a decision only the human
can make:

1. Write a structured entry to [`STATUS.md`](STATUS.md) describing where
   you stopped and what you need.
2. Open a GitHub Issue using the `agent-question` template
   (`.github/ISSUE_TEMPLATE/agent-question.md`).
3. **End the session.** Do not poll, sleep, or spin.

When the human answers later, a Routine starts a **fresh** session that
reconstructs state from this repo. See
[`docs/resume-protocol.md`](docs/resume-protocol.md) for the exact
mechanic. No session is ever kept alive.

## Session start checklist (every role, every session)

1. Read this file.
2. Read [`STATUS.md`](STATUS.md) — find your role's section.
3. If you were resumed by an issue event, read that issue thread.
4. Read your role's skill (below), then continue the work.

## The four roles

| Role | Skill | Runs in |
|------|-------|---------|
| Build agent | [`skills/build-agent/SKILL.md`](skills/build-agent/SKILL.md) | Claude Code (cloud) |
| Automated tester | [`skills/automated-tester/SKILL.md`](skills/automated-tester/SKILL.md) | Claude Code (cloud) |
| Manual tester | [`skills/manual-tester/SKILL.md`](skills/manual-tester/SKILL.md) | Claude Cowork (VM, computer use) |
| Marketing agent | [`skills/marketing-agent/SKILL.md`](skills/marketing-agent/SKILL.md) | Cowork or Claude Code |

## Docs

- [`docs/pipeline-architecture.md`](docs/pipeline-architecture.md) — the whole system, plain language + diagram
- [`docs/resume-protocol.md`](docs/resume-protocol.md) — ask-and-pause / resume, step by step
- [`docs/routines-setup.md`](docs/routines-setup.md) — how to wire the triggers (done in the UI, not here)

## Project stack — FILL THIS IN

This scaffold is **stack-agnostic**. Before the build agent writes any
application code, the repo owner fills in the real project below. Until
then, the build agent must ask (via ask-and-pause) rather than assume.

```
Application name:   <TBD>
Language / runtime: <TBD>
Framework:          <TBD>
Package manager:    <TBD>
Test runner:        <TBD>  (Playwright is assumed for web E2E)
Deploy target:      <TBD>
Repo / branch model: trunk = main; feature branches feature/<name>
```
