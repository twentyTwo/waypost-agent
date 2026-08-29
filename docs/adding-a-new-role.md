# Adding a new role

The framework is not built around the three roles it ships with. Adding a
fourth — a docs writer, a release manager, a security reviewer, a marketing
agent — is five steps and touches no shared logic.

## 1. Write the skill

```bash
cp -r skills/_template skills/<new-role>
```

Fill in every section: Trigger, Inputs, Procedure, Report/output format,
Retry ceiling, Ask-and-pause rule. Keep it short — it is read at the start
of every session in that role.

If the role's real trigger is compound ("merged **and** both reports
Pass"), put the cheap event in the trigger and verify the rest in step 1 of
the procedure. See `routines-setup.md`.

## 2. Create the status file

```bash
cp STATUS/_template.md STATUS/<new-role>.md
```

Fill in the `# STATUS — <role>` heading and leave the state empty.

## 3. Add a row to the role table in `CLAUDE.md`

| Role | Skill | Status file | Trigger |

That table is the registry. A role that is not in it does not exist as far
as other sessions are concerned.

## 4. Document its trigger in `routines-setup.md`

Add a section: trigger event, task, access scopes, any MCP it needs. Then
configure the Routine in the UI. The doc is the only committed record of
what the UI is set to.

## 5. Decide its output convention now

If the role produces reports or artifacts, choose the folder and naming
**before** its first run, and write it into the SKILL.md:

```
<folder>/<date>-<feature>-<role>.<ext>
```

`/qa-runs/` uses this. Follow it — consistent naming is what lets one role
find another's output without being told where to look. A role that
invents its own scheme is invisible to everything downstream.

## What you do not have to touch

Nothing. There is no orchestrator, no role enum, no registry file beyond
the `CLAUDE.md` table, and no code that knows how many roles exist. Roles
coordinate through files and labels, so they compose by addition.

## Also update

- The diagram in `pipeline-architecture.md` if the new role changes the
  flow rather than sitting alongside it.
- `SETUP.md` if the role needs a new label or a new manual setup step.
