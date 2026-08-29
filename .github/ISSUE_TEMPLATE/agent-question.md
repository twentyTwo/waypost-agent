---
name: Agent question (needs human)
about: An agent is blocked and needs a decision only the repo owner can make
title: "[needs-human] <role>: <short summary>"
labels: ["needs-human"]
assignees: []
---

<!--
Agents: fill every field, then update /STATUS/<your-role>.md and END THE
SESSION. Do not wait for a reply. Batch every question you accumulated
this session into the checklist below — do not open one issue per
ambiguity. If an open question issue for your role already exists, append
a comment to it instead of opening a second one.
-->

### Role
<!-- The role name exactly as it appears in the CLAUDE.md role table,
     e.g. build / auto-test / manual-test. New roles use their own name. -->


### What I was doing


### Questions
<!-- One checkbox per question. The human ticks them off as they answer. -->
- [ ]
- [ ]

### Options I'm considering
<!-- Per question where you have candidates, or "none — open question". -->


### State reference
- Status file: `/STATUS/<role>.md` → `## Current`
- Skill version followed: `<short hash>`
- Branch / PR / path:

### What happens when you reply
Commenting here triggers a Routine that starts a **fresh** session. It reads
`CLAUDE.md` → its status file → this thread, then continues from the last
completed step. Nothing is lost by the delay. See `docs/resume-protocol.md`.
