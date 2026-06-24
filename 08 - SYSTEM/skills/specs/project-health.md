---
type: skill-spec
name: project-health
status: scaffold
audience: [human, AI agent]
---

# project-health

## Purpose

Audit every active project folder in `03 - PROJECTS/` and classify each as On Track / At Risk / Stalled / Blocked, with one concrete next-action suggestion per project. Outputs a weekly report Adam can scan in 90 seconds.

## Trigger

**Manual:** "Run project health check" or "Audit my projects" — invokes on demand.

**Scheduled:** Disabled by default. When enabled, fires Mondays at 07:00 Europe/Budapest (start of work-week — Adam reviews before diving in).

**Activation gate (CRITICAL):** this skill must NOT run on an empty projects folder.
- `ls "03 - PROJECTS/"*.md 2>/dev/null | wc -l + ls -d "03 - PROJECTS/"*/ 2>/dev/null | wc -l >= 3` (at least 3 active project folders OR overview.md files)

If fewer than 3 projects, skill aborts with: "project-health requires ≥3 active projects. Currently N."

## Process

For each project in `03 - PROJECTS/*/`:

1. **Read `overview.md`** (if exists). Extract: goal, status, deadline, completion %.
2. **Read `tasks.md`** (if exists). Extract: open task count, tasks with overdue due dates, last-modified date.
3. **Read all files in `notes/`** (subfolder, if exists). Look at last-modified dates of the most recent notes.
4. **Classify the project:**
   - **On Track**: completion advancing, no overdue tasks, recent notes (within last 14 days), deadline still realistic
   - **At Risk**: completion stalled OR 1-2 overdue tasks OR no notes in 14-30 days OR deadline tight
   - **Stalled**: no notes in 30+ days OR completion hasn't moved AND there are open tasks
   - **Blocked**: any note or tasks.md entry explicitly mentions a blocker ("waiting on X", "blocked by Y")

5. **Suggest one concrete next action** based on the project's state:
   - On Track → continue current trajectory; suggest the next task in `tasks.md`
   - At Risk → suggest the highest-impact unblocking move (e.g., "schedule 30min review session for Friday")
   - Stalled → suggest the smallest possible reactivation (e.g., "open `overview.md` and update `last-updated` date; pick one task and set a deadline")
   - Blocked → name the blocker explicitly; suggest the conversation/email that would unblock

6. **Repeat for each project.**

7. **Sort the report** with most-at-risk projects first (Blocked → Stalled → At Risk → On Track).

8. **Quality Gate** before saving — see below. If FAIL, fix once. If still FAIL, save with `FAILED-` prefix.

9. **Save to** `06 - HERMES-OUTPUTS/reviews/YYYY-MM-DD-project-health.md`.

## Output format

```markdown
---
type: hermes-output
kind: project-health
date: YYYY-MM-DD
generated-by: project-health v0.1
project-count: N
classification-summary: { on-track: X, at-risk: Y, stalled: Z, blocked: W }
---

# Project Health — YYYY-MM-DD

## TL;DR
- **N projects audited**
- **Blocked:** W (drop everything else, these are the real problem)
- **Stalled:** Z
- **At Risk:** Y
- **On Track:** X

## Blocked (W projects)
### [Project Name]
- **Blocker:** [specific blocker from notes or tasks.md]
- **Suggested unblock:** [concrete conversation/email to send, with whom, by when]
- **Last activity:** YYYY-MM-DD ([N days ago])

## Stalled (Z projects)
### [Project Name]
- **Last activity:** YYYY-MM-DD ([N days ago])
- **Suggested reactivation:** [smallest possible step to restart]

## At Risk (Y projects)
### [Project Name]
- **Risk:** [overdue tasks / stale notes / deadline tight]
- **Suggested action:** [highest-impact unblock]

## On Track (X projects)
### [Project Name]
- **Progress:** [completion %]
- **Suggested next:** [next task from tasks.md]
```

## Quality Gate

| Criterion | Check |
|---|---|
| **All projects classified** | Every folder in `03 - PROJECTS/` appears in one of the 4 buckets |
| **Concrete suggestions** | Every suggestion is an action, not an observation. (No "consider reviewing" — must be "schedule X for Y") |
| **Blockers explicit** | If a project is classified Blocked, the blocker must be quoted from a note or tasks.md entry (with citation) |
| **Sorted by risk** | Blocked projects appear before Stalled before At Risk before On Track within the report |
| **Date header correct** | File is named `YYYY-MM-DD-project-health.md` matching the date |
| **Self-contained** | Reading just this file gives a complete picture; no need to open any project folder to understand the report |

## Output

A single markdown file at `06 - HERMES-OUTPUTS/reviews/YYYY-MM-DD-project-health.md`. No other files modified.

## See also

- `08 - SYSTEM/skills/specs/vault-morning-brief.md` — references the most recent project-health report
- `08 - SYSTEM/skills/specs/weekly-synthesis.md` — references project-health output for weekly themes
- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` Phase 5 — origin and trigger-condition rationale