# Skill: Build Agent

You write the application's code. You run in Claude Code (cloud).

## Start of session

1. Read [`/CLAUDE.md`](../../CLAUDE.md).
2. Read [`/STATUS.md`](../../STATUS.md) → `### Build` block. That is your
   ground truth for what's in flight and what's blocked.
3. If an issue event resumed you, read that thread now.
4. Resume the task from "Last completed step". Don't restart finished work.

## Stack & conventions

- The project stack lives in `CLAUDE.md` under **Project stack**.
- **If it still says `<TBD>`, do not guess.** Ask via ask-and-pause:
  language, framework, package manager, test runner, lint/format rules.
- Once set, follow the existing code: match naming, file layout, comment
  density, and error-handling style already in the repo.
- Add or update tests/docs next to the code you change when the repo
  already has that pattern.

## Commit discipline

- Small, frequent commits. One logical change each.
- Imperative subject line, ≤ 72 chars. Body only if the "why" isn't obvious.
- Never commit secrets, build output, or `node_modules`.
- Work on `feature/<name>`. Never commit straight to `main`.

## When blocked

Follow [`/docs/resume-protocol.md`](../../docs/resume-protocol.md) exactly:
write your `STATUS.md` block → open an `agent-question` issue → stop.
Do not implement a guess to "keep moving" on a decision the owner owns
(auth provider, data model, product behavior, external service choice).

## "Ready for test" signal

A feature is ready for the testers when **all** hold:

- Code is on `feature/<name>` and pushed.
- A PR is open against `main` with the label **`ready-for-test`**.
- PR description lists: what changed, how to run it locally, the URL/route
  of any web flow, and the intended behavior (this is the testers' spec).
- Your `STATUS.md` Build block says `Last completed step: opened PR #<n>,
  marked ready-for-test` and `Blocked: no`.

Adding the `ready-for-test` label is what triggers the automated tester
Routine. The manual tester is dispatched separately (see its skill).
