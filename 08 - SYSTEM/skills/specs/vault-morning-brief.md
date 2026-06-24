---
type: skill-spec
name: vault-morning-brief
status: scaffold
audience: [human, AI agent]
---

# vault-morning-brief

## Purpose

Generate a structured morning brief using the vault as primary context, with optional web search for external developments relevant to current priorities. The brief should give Adam (or any future vault owner) a 60-second read on what's important today.

## Trigger

**Manual:** "Run morning brief" or "Generate today's brief" — invokes on demand.

**Scheduled:** Disabled by default. When enabled, fires daily at 06:00 Europe/Budapest (or the user's timezone per `08 - SYSTEM/CLAUDE.md` Identity section).

**Activation gate (CRITICAL):** this skill must NOT run on an empty or near-empty vault. Both conditions must be true:
- `ls "02 - NOTES/daily/"*.md | wc -l >= 10` (at least 10 daily notes exist)
- `08 - SYSTEM/CLAUDE.md` has a populated Identity section (Primary Interests non-empty)

If either fails, the skill aborts with a clear message: "vault-morning-brief requires ≥10 daily notes and a populated CLAUDE.md Identity section. Currently N daily notes; Identity populated: yes/no."

## Process

1. **Read `08 - SYSTEM/CLAUDE.md`** for identity, primary interests, and active projects/theses sections.
2. **Read today's daily note** if it exists at `02 - NOTES/daily/YYYY-MM-DD.md`. Note what's already written — the brief should *augment*, not duplicate.
3. **Read all `overview.md` files** in `03 - PROJECTS/*/overview.md`. Build a project-pulse list (active projects, last-modified dates).
4. **Check for overdue tasks** in each `03 - PROJECTS/*/tasks.md`. Tasks whose due date is before today count as overdue.
5. **Read the most recent weekly review** at `06 - HERMES-OUTPUTS/syntheses/[latest-date]-weekly-synthesis.md` if it exists, for context on what Adam was focused on last week.
6. **Optional web search**: for each item in CLAUDE.md's Primary Interests (max 5 searches), look for "significant developments from the last 24 hours." Filter using CLAUDE.md's stated preferences — exclude noise (opinion pieces, rehashed content) per Adam's communication style documented in CLAUDE.md rule #8.
7. **Synthesize** into the structured brief format below. Length: 60-second read = ~300-500 words.
8. **Quality Gate** before saving — see below. If FAIL, revise once. If still FAIL after one revision, save to `06 - HERMES-OUTPUTS/briefings/FAILED-YYYY-MM-DD-morning-brief.md` with a `## Failure Notes` section explaining which criteria failed.
9. **Save to** `06 - HERMES-OUTPUTS/briefings/YYYY-MM-DD-morning-brief.md`.

## Output format

```markdown
---
type: hermes-output
kind: morning-brief
date: YYYY-MM-DD
generated-by: vault-morning-brief v0.1
---

# Morning Brief — YYYY-MM-DD

## THE ONE THING
[Single most important thing for Adam today. Be specific. If from the vault: cite the note. If from web search: cite the source.]

## PROJECT PULSE
- [Project A] — [status: On Track / At Risk / Stalled] — [one concrete next action from tasks.md]
- [Project B] — ...
- (omit section if no active projects)

## OVERDUE / AT-RISK
- [Task from tasks.md with due date < today] — [project]
- (omit section if no overdue tasks)

## EXTERNAL INTELLIGENCE (optional, only if web search produced results)
- [Interest 1]: [1-2 sentence development] — [source]
- (omit section if no significant developments)

## DECISION REQUIRED
- [Any item where the brief surfaced a question Adam needs to answer. Max 2 items.]
- (omit section if nothing needs Adam's input)

## FROM PREVIOUS SESSION
- [Anything from the most recent weekly synthesis or last brief that Adam said he'd address but hasn't yet.]
- (omit section if nothing)
```

## Quality Gate

Before saving, verify all PASS criteria:

| Criterion | Check |
|---|---|
| **Identity cited** | Brief references at least one item from `CLAUDE.md`'s Primary Interests (proves it read the vault) |
| **Vault-grounded** | At least one section (PROJECT PULSE or OVERDUE/AT-RISK) is populated from real vault data, not boilerplate |
| **No duplicates with daily note** | If today's daily note exists, the brief doesn't repeat what's already in the "Schedule" or "Captures" sections |
| **Length in range** | 200-700 words total. Briefs under 200 are too thin to be useful; over 700 violates the 60-second-read promise |
| **Citations** | External facts cite a source URL. Vault facts cite the note filename (e.g., `02 - NOTES/daily/2026-06-24.md`) |
| **Date header correct** | File is named `YYYY-MM-DD-morning-brief.md` matching the date in frontmatter |

If any criterion FAILs, attempt one revision. If still failing, save with `FAILED-` prefix and `## Failure Notes`.

## Output

A single markdown file at `06 - HERMES-OUTPUTS/briefings/YYYY-MM-DD-morning-brief.md` (or `FAILED-` prefix if Quality Gate failed). No other files modified. Log entry in `hermes.log`.

## Configuration

**Activation:** per the gate conditions. To force-enable on an empty vault (testing), pass `--force` flag.

**Web search budget:** 5 searches max per run. If the vault has fewer than 3 Primary Interests populated, reduce to 3 searches.

**Timezone:** defaults to `Europe/Budapest`. Override via `08 - SYSTEM/CLAUDE.md` Identity section.

**Quiet mode:** if the user has `mode: focus` flag in CLAUDE.md (a flag we haven't built yet), suppress the EXTERNAL INTELLIGENCE section.

## See also

- `08 - SYSTEM/skills/specs/inbox-processor.md` — companion skill for processing captured content
- `08 - SYSTEM/skills/specs/weekly-synthesis.md` — companion skill for end-of-week consolidation
- `08 - SYSTEM/CLAUDE.md` — Identity section drives personalization
- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` Phase 5 — origin and trigger-condition rationale