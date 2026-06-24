---
type: skill-spec
name: connection-finder
status: scaffold
audience: [human, AI agent]
---

# connection-finder

## Purpose

Discover non-obvious connections between recently-modified notes and older permanent notes. The "atomic notes belong to humans" rule (CLAUDE.md #5) means humans are responsible for adding wikilinks, but humans forget. This skill surfaces *proposed* links for Adam to accept or reject — never silently edits the notes themselves.

## Trigger

**Manual:** "Find connections" or "Surface links between recent notes" — invokes on demand.

**Scheduled:** Disabled by default. When enabled, fires Sundays at 17:00 Europe/Budapest (end of week, before weekly-synthesis at 19:00 so the synthesis can incorporate).

**Activation gate (CRITICAL):** this skill must NOT run on an empty or sparse vault.
- `find "02 - NOTES/permanent/" -name "*.md" 2>/dev/null | wc -l >= 15` (at least 15 permanent notes exist — too few and the analysis is meaningless)
- AND `find "02 - NOTES/" -name "*.md" -mtime -7 2>/dev/null | wc -l >= 3` (at least 3 notes modified in the last 7 days — gives the skill something to anchor against)

If either fails, skill aborts with: "connection-finder requires ≥15 permanent notes and ≥3 modified-this-week notes. Currently: N permanent / M recent."

## Process

1. **List recent notes**: find all `.md` files in `02 - NOTES/` modified in the last 7 days (use `find -mtime -7`).
2. **Read each recent note** to extract key concepts (title, headings, first paragraph, distinctive terms).
3. **List candidate target notes**: all `.md` files in `02 - NOTES/permanent/` modified more than 7 days ago (the older population).
4. **For each recent note**, score against candidate targets:
   - **High-confidence connection** (recommend linking): shared distinctive concept, parallel structure (e.g., both notes describe competing mental models), or one note explicitly references the other's domain
   - **Medium-confidence connection** (mention but don't recommend): overlapping vocabulary without structural connection
   - **No connection**: surface similarity but no semantic link
5. **For each high-confidence connection**, propose:
   - Source note (the recent one)
   - Target note (the older one)
   - Reason for the link (1 sentence)
   - Suggested link text (where in the source note should the wikilink go, and what should the anchor text say)
6. **Output the report.** **Do NOT modify any note files.** This is read-only on the vault; Adam reviews and accepts by adding the wikilinks himself.
7. **Quality Gate** before saving — see below.

## Output format

```markdown
---
type: hermes-output
kind: connections
date: YYYY-MM-DD
generated-by: connection-finder v0.1
recent-note-count: N
target-note-count: M
proposed-links: K
---

# Connections — Week of YYYY-MM-DD

## How to use this report

For each proposal below, add the wikilink to the source note at the suggested anchor text if you agree the link is real. Reject proposals where the connection is forced — your judgment > this skill's heuristics.

---

## Proposed connections (K)

### Source: [[2026-06-20-perm-attention-economy]]
**Targets:**
- `[[2026-05-12-perm-deep-work-defenses]]` — reason: both discuss productivity under distraction; deep-work defenses is the explicit counter to attention-economy framing
  - Suggested anchor: under `## Tension`, link with text "see also [[2026-05-12-perm-deep-work-defenses]] for the practical counter-argument"
- `[[2026-04-03-perm-flow-state-prerequisites]]` — reason: flow requires sustained attention, but the attention economy fragments it
  - Suggested anchor: under `## Why This Matters`

### Source: [[2026-06-22-daily-clinical-reasoning-day]]
...

## Not recommended (medium-confidence, surfaced for awareness only)

- `[[2026-06-15-perm-x]]` ↔ `[[2026-06-10-perm-y]]` — share vocabulary "decision" but structurally different; not linked
- ...

## No matches

The following recent notes had no high-confidence connection to any older permanent note. That's expected — not every note needs to link backward.

- [[2026-06-21-perm-random-thought]]
- ...
```

## Quality Gate

| Criterion | Check |
|---|---|
| **Source notes exist** | Every proposed source note is in `02 - NOTES/` and modified in the last 7 days |
| **Target notes exist** | Every proposed target note is in `02 - NOTES/permanent/` and was modified more than 7 days ago (no proposing links to fresh notes) |
| **Every proposal has a reason** | No "connection found, link them" entries — each must say *why* in 1 sentence |
| **Suggested anchor is specific** | Each suggestion names a section heading AND proposed anchor text (not just "add a wikilink somewhere") |
| **No silent edits** | `git status` shows no modifications to any note files in `02 - NOTES/` after the skill runs (vault is read-only) |
| **Report is reviewable in 5 min** | Total length < 1000 words; if N proposals is large, prioritize top 10 by confidence |

## Output

A single markdown file at `06 - HERMES-OUTPUTS/analyses/YYYY-MM-DD-connections.md`. Read-only on the vault — Adam reviews and accepts/rejects proposals by manually editing his notes. No other files modified.

## See also

- `08 - SYSTEM/skills/specs/inbox-processor.md` — runs earlier in the day, creates the notes that this skill analyzes
- `08 - SYSTEM/skills/specs/weekly-synthesis.md` — runs after this skill, incorporates surfaced connections into weekly themes
- `08 - SYSTEM/CLAUDE.md` rule #5 — atomic notes belong to humans; this skill proposes, never silently edits
- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` Phase 5 — origin and trigger-condition rationale