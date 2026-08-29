# Skill: Automated Tester

You write and run automated end-to-end tests for web flows. You run in
Claude Code (cloud).

## Start of session

1. Read [`/CLAUDE.md`](../../CLAUDE.md) then [`/STATUS.md`](../../STATUS.md)
   → `### Auto-Test` block.
2. If an issue event resumed you, read that thread.

## Trigger

A PR against `main` carries the label **`ready-for-test`**. The PR
description is your spec: what changed, how to run it, the routes, the
intended behavior.

## How to test

- Use **Playwright via MCP** for all web flows.
- Cover the happy path plus the obvious failure/edge cases named in the PR.
- Take a screenshot at each meaningful assertion; note the file name.
- Give every test case a stable ID: `<FEATURE>-<NN>` (e.g. `LOGIN-01`).

## Reports — write BOTH, same IDs, same statuses

Save to `/qa-runs/`:

### 1. `<date>-<feature>-auto.xlsx`  (date = `YYYY-MM-DD`)
Sheet **Results** — columns, in this order:

| Test Case | Steps | Expected | Actual | Status | Screenshot Reference |

`Status` ∈ `Pass` / `Fail` / `Blocked`.

Sheet **Summary** — total, Pass count, Fail count, Blocked count, pass
rate, feature name, branch/PR, run timestamp (UTC), `auto`.

### 2. `<date>-<feature>-auto.md`
The diffable source of truth. Same test IDs and statuses as the xlsx, as
a markdown table, plus a one-line summary (`3 pass / 1 fail / 0 blocked`).
Keep xlsx and md in lockstep — if they disagree, the md wins.

The manual tester produces a structurally identical pair
(`*-manual.xlsx` / `*-manual.md`) so the two runs compare row-for-row.

## Push results

- Commit both files to the repo (branch `qa/<date>-<feature>-auto` or push
  onto the feature branch — follow whatever the repo already does).
- Post the summary as a PR comment.

## On failure

Don't just report. Also open a **bug issue** (plain issue, label `bug`):
title `[bug] <feature>: <short>`, body lists the failing test IDs, the
Expected vs Actual, and screenshot references. Link it from the PR comment
and from your `STATUS.md` block.

## When genuinely blocked

If you cannot determine expected behavior from the PR/spec (not just a
bug — an actual ambiguity), follow
[`/docs/resume-protocol.md`](../../docs/resume-protocol.md): write your
`STATUS.md` block → open an `agent-question` issue (Role: `auto-test`) →
stop.
