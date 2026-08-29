# SETUP.md — do this before any agent touches the repo

You just created a repository from this template. None of the steps below
can be pre-wired by the template — work through them once, in order.

- [ ] **Fill in the Project Stack section in [`CLAUDE.md`](CLAUDE.md).**
      Language, framework, package manager, test runner, deploy target,
      conventions. Until this is filled in, the build agent will stop and
      ask instead of writing code, which wastes a round trip.

- [ ] **Create the labels the pipeline signals with:**
      ```bash
      gh label create needs-human    --color B60205 --description "Agent blocked, needs owner decision"
      gh label create ready-for-test --color 0E8A16 --description "PR ready for the test agents"
      gh label create bug            --color D93F0B --description "Failing test / defect"
      ```
      `needs-human` is applied automatically by the issue template, so it
      must exist before the first agent question.

- [ ] **Configure the Routines** — one per role, in the Claude Code UI.
      See [`docs/routines-setup.md`](docs/routines-setup.md). Routines are
      not repo config; nothing in this template can create them for you.

- [ ] **If using the manual tester:** pair Cowork with the target VM via
      Dispatch, and confirm the VM stays awake for the length of a run.
      The manual tester is not a Routine — see
      [`docs/routines-setup.md`](docs/routines-setup.md).

- [ ] **Verify the issue template renders.** Open a throwaway issue from
      the `agent-question` template, confirm the fields appear and the
      `needs-human` label is applied automatically, then close it.

- [ ] **Clear the example entries in [`/STATUS/`](STATUS/).** Each role's
      file ships with a placeholder `## Current` block and one example
      changelog line. Replace them with empty state before real work
      starts, so a fresh session never mistakes an example for live state.

- [ ] **Turn off "Template repository"** in GitHub repo settings if this is
      now a real project rather than another template.

Once all boxes are ticked, open an issue describing the first feature. The
build agent takes it from there.
