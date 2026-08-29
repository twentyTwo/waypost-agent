# Agentic Build/Test Pipeline — template repository

A reusable coordination scaffold for running a software project with
multiple AI agent roles — build, automated test, manual test — that share
this repo as their memory and **never block waiting for a human**.

When an agent gets stuck it writes its state to `/STATUS/<role>.md`, asks
in a GitHub Issue, and ends its session. Hours or days later you reply; a
fresh session rebuilds context from the repo and continues. No session is
ever held open waiting.

This repo contains **no application code**. Click **Use this template**,
then work through [`SETUP.md`](SETUP.md).

- [`SETUP.md`](SETUP.md) — what you must do manually, first
- [`CLAUDE.md`](CLAUDE.md) — what every agent session reads first
- [`docs/pipeline-architecture.md`](docs/pipeline-architecture.md) — how the whole thing fits together
- [`docs/adding-a-new-role.md`](docs/adding-a-new-role.md) — adding a fourth role, or a tenth
