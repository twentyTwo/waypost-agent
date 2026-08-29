# Resume protocol — ask-and-pause, then resume

The mechanic that lets agents run against an absent human. Read this once;
the rule it produces is in `CLAUDE.md`.

## The problem it solves

An agent that waits for an answer holds a session open for hours or days.
That session burns budget doing nothing, and dies anyway — on a timeout, a
disconnect, a restart — losing whatever was only in its context.

So: **no session ever waits.** A blocked agent writes down everything a
successor would need, asks, and stops.

## Step 1 — the agent blocks

Not every uncertainty is a block. A block is a decision the human must make
that changes what gets built, and that cannot be derived from the repo.
Each role's SKILL.md says what counts for that role.

## Step 2 — the agent records state

Update `/STATUS/<role>.md`:

- `## Current` — **overwritten**. Task, last completed step (specific and
  verifiable), the skill version hash, the open question link, timestamp.
- `## Changelog` — **one line appended, newest first**. Never edited, never
  deleted.

The skill version hash matters: `git log -1 --format=%h -- skills/<role>/SKILL.md`.
If the SKILL.md changes between the pause and the resume, the successor can
see that the rules moved under the entry it is reading.

## Step 3 — the agent asks

Open one issue from the `agent-question` template, with **every** question
accumulated this session as a checklist. If an open question issue for this
role already exists, comment on it instead of opening a second.

One issue per session, not one per question. The human answers a batch in
one sitting; a stream of single-question issues turns one interruption into
five.

## Step 4 — the session ends

Cleanly. No polling, no sleeping, no keeping a turn alive, no "I'll check
back". The session is over.

## Step 5 — the human replies

Whenever. Minutes or days; the protocol does not care.

## Step 6 — a Routine starts a fresh session

The reply is a GitHub event. A per-role Routine triggers on it
(`routines-setup.md`) and starts a **new** session — not a continuation.
Nothing carries over except this repository.

## Step 7 — the fresh session rebuilds context

In this order, before doing anything else:

1. `CLAUDE.md`
2. `/STATUS/<role>.md` — where the predecessor stopped
3. The issue thread — the question and the answer
4. Its own `skills/<role>/SKILL.md`

Then it continues from "Last completed step". It does not redo finished
work, and it does not re-ask an answered question.

## Why the repo is the only memory

Every fact a successor needs is a committed file. That is the whole design:
sessions are disposable, the repo is not. It is also why status files are
structured fields rather than prose — a cold session has to parse them in
one read, without the context that made them obvious to write.
