# STATUS.md — Pipeline state

Structured, not prose. One block per role. A new session must parse this
in one read. Update your own block before you stop; leave others alone.

Fields per role:
- **Current task** — one line
- **Last completed step** — one line, concrete
- **State / branch** — branch, PR, or path the work lives on
- **Open questions** — `#<issue>` links, or `none`
- **Blocked** — `yes` / `no` (if yes, an open question must exist)
- **Updated** — ISO 8601 UTC, by which role/session

---

## Example (delete once real work starts)

### Build
- Current task: Add login form to web app
- Last completed step: Scaffolded `/login` route, form renders, no submit handler yet
- State / branch: `feature/login`
- Open questions: `#12` (which auth provider?)
- Blocked: yes
- Updated: 2026-08-29T14:05:00Z by build-agent

### Auto-Test
- Current task: —
- Last completed step: Waiting for `feature/login` to be marked ready for test
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T14:06:00Z by automated-tester

### Manual-Test
- Current task: —
- Last completed step: Nothing shipped yet
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T14:06:00Z by manual-tester

### Marketing
- Current task: —
- Last completed step: Nothing merged + fully passed yet
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T14:06:00Z by marketing-agent

---

## Live

### Build
- Current task: —
- Last completed step: Repo scaffolded; awaiting project stack in CLAUDE.md
- State / branch: `main`
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T00:00:00Z by scaffold

### Auto-Test
- Current task: —
- Last completed step: —
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T00:00:00Z by scaffold

### Manual-Test
- Current task: —
- Last completed step: —
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T00:00:00Z by scaffold

### Marketing
- Current task: —
- Last completed step: —
- State / branch: —
- Open questions: none
- Blocked: no
- Updated: 2026-08-29T00:00:00Z by scaffold
