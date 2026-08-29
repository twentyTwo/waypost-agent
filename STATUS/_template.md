<!--
Copy this file to /STATUS/<new-role>.md when adding a role.
See docs/adding-a-new-role.md.

Rules for every status file:
  - `## Current` is OVERWRITTEN each session. It holds present state only.
  - `## Changelog` is APPEND-ONLY, NEWEST FIRST. One line per session.
    Never edit or delete an existing line — a human catching up after days
    away reads from the top down and needs the record intact.
  - Be specific. "Working on auth" is useless to a fresh session;
    "commit abc123 — login form renders, no submit handler" is not.
  - Skill version = git log -1 --format=%h -- skills/<role>/SKILL.md
    It tells a later session whether the rules changed under this entry.
-->

# STATUS — <role>

## Current
- Task: <what is being worked on, one line — or "none">
- Last completed step: <specific and verifiable: commit hash, file, PR>
- Skill version used: <short commit hash of this role's SKILL.md>
- Open question: <#issue link, or "none">
- Updated: <ISO 8601 UTC, e.g. 2026-08-29T14:02:00Z>

## Changelog
<!-- newest first; append above this line's successor, never below -->
- <YYYY-MM-DD HH:MM> — <what happened this session, one line>
