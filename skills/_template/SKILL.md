<!--
Copy this directory to /skills/<new-role>/ and fill in every section.
See docs/adding-a-new-role.md for the other four steps.

Keep the finished file SHORT. An agent reads it at the start of every
session, alongside CLAUDE.md and its status file. Anything that is not
actionable is costing you context on every run.
-->

# Skill: <Role name>

<!-- One sentence: what this role is responsible for, and where it runs
     (Claude Code cloud, Cowork VM, local). -->

## Trigger

<!-- The exact repo event or dispatch that starts a session in this role.
     Be concrete — "PR labeled ready-for-test", not "when code is ready".
     If the real condition is compound (two things must both be true),
     state that the trigger only approximates it and the first step of the
     procedure must verify the rest. See docs/routines-setup.md. -->

## Inputs

<!-- What this role reads before acting, in order. Always starts with
     CLAUDE.md and /STATUS/<role>.md. Then: the triggering thread, the
     diff, prior reports, whatever this role specifically needs. -->

## Procedure

<!-- Numbered steps. The happy path only — exceptions belong in the
     ask-and-pause and retry sections below. -->

## Report / output format

<!-- Where output goes and what shape it takes. Use the shared naming
     convention: <date>-<feature>-<role>.<ext>. If this role produces
     structured reports, keep a diffable .md alongside any binary format
     so changes are reviewable in a PR. If it produces nothing, say so. -->

## Retry ceiling

<!-- How many attempts at the same failing thing before stopping. Default
     is 3 (see CLAUDE.md). State what "the same failing thing" means for
     this role specifically. -->

## Ask-and-pause rule

<!-- When this role stops and asks rather than deciding for itself. Be
     specific about what counts as a genuine blocker for this role versus
     a judgment call it should just make. Then the standard procedure:
     write /STATUS/<role>.md → open or append to the agent-question issue,
     batching all questions → END THE SESSION. -->
