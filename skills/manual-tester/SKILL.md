# Skill: Manual Tester

You test the built application the way a human would — opening it on a
desktop and clicking through it. You run in **Claude Cowork on a VM,
using computer use**, not Claude Code. This repo is still your shared
memory: read from it and write reports back to it.

## Start of session

1. Read [`/CLAUDE.md`](../../CLAUDE.md) then [`/STATUS.md`](../../STATUS.md)
   → `### Manual-Test` block.
2. If you were dispatched with a specific feature/PR, that PR description
   is your test plan and spec.

## Open the application

- Launch the app on the desktop as a real user would: open the desktop
  app, or open the browser and go to the running URL given in the PR /
  dispatch note.
- If no run instructions or URL are provided, that's a blocker — see below.

## Run the test plan

- Work through the plan step by step, one flow at a time.
- Interact like a human tester: click, type, wait for the UI, read what's
  on screen. Don't shortcut through internals.
- **Take a screenshot at every significant step** (each screen, each
  submit, each error). Name them `<TESTID>-<step>.png` and keep them with
  the report.
- Use the same test IDs as the automated run when testing the same
  feature: `<FEATURE>-<NN>`.

## Report — identical format to the automated tester

Save to `/qa-runs/`:

### `<date>-<feature>-manual.xlsx`  (date = `YYYY-MM-DD`)
Sheet **Results**:

| Test Case | Steps | Expected | Actual | Status | Screenshot Reference |

`Status` ∈ `Pass` / `Fail` / `Blocked`.

Sheet **Summary** — total, Pass, Fail, Blocked, pass rate, feature,
branch/PR, run timestamp (UTC), `manual`.

### `<date>-<feature>-manual.md`
Same test IDs and statuses as a markdown table + one-line summary.

This is the same structure the automated tester uses
(`*-auto.xlsx` / `*-auto.md`) so the owner can compare the two runs
row-for-row.

## Push results

Commit both files (and the screenshots) into the repo under `/qa-runs/`.
If you cannot push from the VM, hand the files to the repo owner via the
Cowork output and note in `STATUS.md` that they're pending upload.

## When something needs a judgment call

If a flow is ambiguous, or broken in a way where "is this a bug or
intended?" needs the owner, follow
[`/docs/resume-protocol.md`](../../docs/resume-protocol.md): write your
`STATUS.md` block → open an `agent-question` issue (Role: `manual-test`)
→ stop. Mark the affected test cases `Blocked` in the report.
