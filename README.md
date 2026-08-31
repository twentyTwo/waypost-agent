# Waypost Agent

**Every session leaves a marker. The next one picks up the trail.**

A template repository for running a software project with multiple AI agent
roles — build, automated test, manual test — that coordinate entirely
through this repo and **never block waiting for a human**.

A waypost is a marker left on a trail for whoever comes next. You will
never meet them; the marker is the entire conversation. That is how these
agents work. When one gets stuck it writes what it knows to
`/STATUS/<role>.md`, asks its question in a GitHub Issue, and ends its
session. Hours or days later you reply, and a **fresh** session reads the
markers, reconstructs where the trail went cold, and carries on.

No session is ever held open waiting for you.

## Why it works this way

An agent that waits for an answer holds a session open for hours, burning
budget doing nothing — and dies anyway, on a timeout or a disconnect,
losing whatever lived only in its context.

So nothing lives only in context. Every fact a successor needs is a
committed file: `/STATUS/<role>.md` for state, `/qa-runs/` for results,
labels for signals. **Sessions are disposable. The repo is not.**

This costs a cold start on every resume, and buys immunity to timeouts,
disconnects, and a repo owner who is asleep.

## This repo contains no application code

It is the scaffold. Click **Use this template**, then work through
[`docs/getting-started.md`](docs/getting-started.md).

| | |
|---|---|
| [`docs/getting-started.md`](docs/getting-started.md) | **start here** — the exact steps, with commands |
| [`SETUP.md`](SETUP.md) | the same setup as a bare checklist |
| [`CLAUDE.md`](CLAUDE.md) | what every agent session reads before anything else |
| [`docs/pipeline-architecture.md`](docs/pipeline-architecture.md) | how the whole thing fits together, with a diagram |
| [`docs/resume-protocol.md`](docs/resume-protocol.md) | ask-and-pause and resume, step by step |
| [`docs/routines-setup.md`](docs/routines-setup.md) | wiring the triggers |
| [`docs/adding-a-new-role.md`](docs/adding-a-new-role.md) | adding a fourth role, or a tenth |

## The three roles it ships with

| Role | Runs in | Triggered by |
|------|---------|--------------|
| Build agent | Claude Code (cloud) | a human issue, or a reply to its question |
| Automated tester | Claude Code + Playwright MCP | PR labeled `ready-for-test` |
| Manual tester | Cowork, computer use | dispatched by you |

Both testers must pass. The build agent shaped the code *and* the tests, so
a green suite can share the author's blind spot — the manual tester looks
at the actual screen. Neither signs off alone.

Nothing here is hardcoded to these three. The role table in
[`CLAUDE.md`](CLAUDE.md) is the whole registry; adding a role is five steps
and touches no shared logic. See
[`docs/adding-a-new-role.md`](docs/adding-a-new-role.md).
