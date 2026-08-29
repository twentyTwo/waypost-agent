# Agentic Pipeline

A reusable **coordination scaffold** for running a software project with
four AI agent roles — build, automated test, manual test, marketing —
that share this repo as their memory and **never block waiting on a
human**. When an agent needs your input it records its state, asks in a
GitHub Issue, and stops. A later session picks up exactly where it left
off using only what's in the repo.

This repo contains **no application code**. It is the hub you point the
agents at. See [`docs/pipeline-architecture.md`](docs/pipeline-architecture.md)
for the full picture.

---

## Using this in a new project

You have two ways to adopt it.

### Option A — pipeline lives alongside your app (recommended)

Copy the scaffold into your application repo so agents have code and
coordination in one place.

```bash
# from your app repo root
git clone --depth 1 https://github.com/<you>/agentic-pipeline /tmp/ap
rm -rf /tmp/ap/.git
cp -r /tmp/ap/{CLAUDE.md,STATUS.md,docs,skills} .
cp -r /tmp/ap/.github/ISSUE_TEMPLATE .github/
mkdir -p qa-runs marketing && touch qa-runs/.gitkeep marketing/.gitkeep
```

If your app repo already has a `CLAUDE.md`, merge the two: keep your
project notes, and paste in the **"never block on a human"** rule, the
**session-start checklist**, and the **role table** from this scaffold's
`CLAUDE.md`.

### Option B — pipeline as its own repo

Keep this repo separate and have the build agent work in a submodule or a
sibling checkout of your app. Simpler to start, but agents then juggle two
repos. Only do this if you can't touch the app repo.

---

## First-time setup checklist

1. **Fill in the stack.** Edit the `Project stack` block in `CLAUDE.md`
   with your real language, framework, package manager, test runner, and
   deploy target. Until this is filled in, the build agent will stop and
   ask.

2. **Create the GitHub repo** (if new) and push:
   ```bash
   gh repo create <name> --private --source . --push
   ```

3. **Create labels:**
   ```bash
   gh label create needs-human   --color B60205 --description "Agent blocked, needs owner decision"
   gh label create ready-for-test --color 0E8A16 --description "PR ready for the test agents"
   gh label create bug            --color D93F0B --description "Failing test / defect"
   ```
   (Add `ready-for-marketing` if you choose that gating option — see
   step 5.)

4. **Wire up Routines** in the Claude Code UI, following
   [`docs/routines-setup.md`](docs/routines-setup.md):
   - Build-agent resume Routine — on comments to `needs-human` issues
   - Automated-tester Routine — on PRs labeled `ready-for-test`
   - Marketing Routine — on PR merged to `main`
   Each Routine needs repo write + issue/PR access; the tester also needs
   the Playwright MCP.

5. **Decide the marketing gate.** The marketing trigger ("merged AND both
   QA reports all-Pass") can't be expressed as a GitHub trigger. Pick one:
   a check step at the start of the Routine task, or a manual
   `ready-for-marketing` label. Noted as a follow-up in
   `docs/routines-setup.md`.

6. **Set up the manual tester** as a Claude Cowork **Dispatch / Scheduled
   Task** (not a Routine) pointing at
   [`skills/manual-tester/SKILL.md`](skills/manual-tester/SKILL.md).

---

## Day-to-day workflow

1. **You** open an Issue or PR describing a feature you want built.
2. **Build agent** implements it on `feature/<name>`, opens a PR against
   `main`, and adds the `ready-for-test` label when done.
3. **Automated tester** runs Playwright flows, writes
   `qa-runs/<date>-<feature>-auto.xlsx` + `.md`, comments on the PR, and
   opens a `bug` issue on any failure.
4. **Manual tester** (dispatched by you in Cowork) clicks through the app,
   writes `qa-runs/<date>-<feature>-manual.xlsx` + `.md` in the same
   format.
5. On failures → build agent fixes → back to step 3.
6. Once merged and **both** reports are all-Pass, the **marketing agent**
   drafts content into `marketing/<feature>/` and opens a PR for you.
7. Any time an agent is blocked on a decision, it updates its `STATUS.md`
   block, opens an `agent-question` issue, and stops. You reply whenever
   you're free; a fresh session resumes automatically. See
   [`docs/resume-protocol.md`](docs/resume-protocol.md).

---

## How an agent starts a session

Every session, regardless of role:

1. Read [`CLAUDE.md`](CLAUDE.md)
2. Read [`STATUS.md`](STATUS.md) — find its role's block
3. If resumed by an issue event, read that issue thread
4. Read its role skill in [`skills/`](skills/)
5. Continue the work — never redo completed steps

---

## Repo layout

```
CLAUDE.md                       orientation + the no-blocking rule (read first)
STATUS.md                       structured live state, one block per role
README.md                       this file
.github/ISSUE_TEMPLATE/
  agent-question.md             template agents use when blocked (auto-labels needs-human)
skills/
  build-agent/SKILL.md
  automated-tester/SKILL.md
  manual-tester/SKILL.md
  marketing-agent/SKILL.md
qa-runs/                        test reports (.xlsx + .md), auto and manual
marketing/                      marketing drafts, per feature
docs/
  pipeline-architecture.md      the whole system, plain language + mermaid diagram
  resume-protocol.md            ask-and-pause / resume, step by step
  routines-setup.md             how to wire the triggers (done in the UI)
```

---

## Customizing

- **Report columns / test-ID scheme** — edit both tester skills together
  so auto and manual stay structurally identical.
- **Branch names, label names** — edit `skills/build-agent/SKILL.md` and
  `docs/routines-setup.md` together.
- **Extra roles** (e.g. docs writer, release manager) — add
  `skills/<role>/SKILL.md`, a `STATUS.md` block, and a Routine.
- Keep every file **short and scannable** — agents read these at session
  start, not at leisure.
