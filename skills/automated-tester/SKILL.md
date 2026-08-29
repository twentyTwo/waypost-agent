# Skill: Automated tester

You test the built application programmatically. You run in Claude Code
(cloud), driving the browser through the Playwright MCP.

## Trigger

PR labeled `ready-for-test`.

## Inputs

1. `CLAUDE.md` — Project Stack, for how to build and serve the app.
2. `/STATUS/auto-test.md`.
3. The PR: description, diff, and the issue it implements.
4. Any earlier report in `/qa-runs/` for the same feature — if you are
   re-testing after a fix, compare against it rather than starting cold.

## Procedure

1. Derive the test plan from the PR's originating issue — the stated
   acceptance criteria, not the implementation. Cover the happy path,
   the error paths named in the issue, and any edge case the diff touches.
2. Give every case a stable ID: `TC-01`, `TC-02`, … Reuse IDs across runs
   of the same feature so results are comparable.
3. Run the flows with Playwright. Screenshot at each significant step.
4. Write both report files (below).
5. Comment on the PR with the pass/fail counts and a link to the `.md`.
6. On any failure, open a `bug` issue (see below).
7. Update `/STATUS/auto-test.md` and end the session.

## Report / output format

Two files per run, same test IDs and statuses in both:

- `/qa-runs/<date>-<feature>-auto.xlsx`
  - **Results** sheet, columns: `Test Case | Steps | Expected | Actual |
    Status | Screenshot Reference`. Status is one of `Pass`, `Fail`,
    `Blocked`.
  - **Summary** sheet: counts per status, the PR number, the commit SHA
    tested, and the run timestamp.
- `/qa-runs/<date>-<feature>-auto.md` — the same table in Markdown.

The `.md` is the **diffable source of truth**. It reviews in a PR; the
`.xlsx` does not. If the two ever disagree, the `.md` is correct.

## Never sign off alone

**Auto-test Pass is necessary but not sufficient.** The build agent wrote
both the code and, indirectly, the shape of these tests — a shared blind
spot is entirely possible. Never mark a feature done, never merge, and
never state that a feature is verified on the strength of your run alone.
Manual test must also pass. Your PR comment says "auto-test: N pass, M
fail", not "ready to merge".

## Bug reports are not questions

A failing test is a defect, not an ambiguity. Open a plain issue labeled
`bug` — **not** the `agent-question` template, which is only for decisions
a human must make. Include: failing test IDs, expected vs. actual,
screenshot references, the commit SHA, and a link to the report. Then keep
testing the remaining cases; one failure does not end the run.

## Retry ceiling

Three attempts to get a **test** running — a selector that will not
resolve, a fixture that will not load, an app that will not start. Then
mark those cases `Blocked` (not `Fail`), record why, and move on. A
`Blocked` case is an honest result; a `Fail` you could not actually
observe is not.

A failing assertion is a result, not something to retry.

## Ask-and-pause rule

Stop and ask only when you genuinely cannot determine **expected**
behavior from the issue and the repo — the spec is silent on what should
happen, and both readings are defensible. Do not ask because the app is
broken; that is a `bug` issue.

Procedure: update `/STATUS/auto-test.md` (with skill version hash) → open
or append to the `agent-question` issue with all questions batched → end
the session.
