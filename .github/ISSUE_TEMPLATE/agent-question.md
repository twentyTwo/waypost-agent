---
name: Agent question (needs human)
about: An agent is blocked and needs a decision from the repo owner
title: "[needs-human] <role>: <short question>"
labels: ["needs-human"]
assignees: []
---

<!--
Fill every field. Keep it short. After opening this issue, update your
role block in STATUS.md and END THE SESSION. Do not wait for a reply.
A fresh session resumes when the owner comments. See docs/resume-protocol.md
-->

### Role
<!-- exactly one: build / auto-test / manual-test / marketing -->
build

### What I was doing
<!-- one or two sentences -->

### What I need clarified
<!-- the specific decision only you can make -->

### Options I'm considering
<!-- list, or "none — open question" -->
-

### Current state reference
<!-- link/anchor into STATUS.md, plus branch / PR / file path -->
- STATUS.md → `### <Role>` block
- Branch / PR / path:

### What happens on your reply
Commenting here triggers a Routine that starts a fresh session. It reads
CLAUDE.md → STATUS.md → this thread, then continues. Nothing is lost.
