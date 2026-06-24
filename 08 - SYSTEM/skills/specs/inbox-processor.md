---
type: skill-spec
name: inbox-processor
status: scaffold
audience: [human, AI agent]
---

# inbox-processor

## Purpose

Process every file in `00 - INBOX/` into the right vault location with the right relationships, then archive the originals. This is the daily "triage" skill — it converts raw captures into atomic notes, project notes, references, or tasks, with the wikilinks that make them discoverable.

## Trigger

**Manual:** "Process my inbox" or "Triage today's captures" — invokes on demand.

**Scheduled:** Disabled by default. When enabled, fires daily at 20:00 (evening — catches the day's captures before they pile up overnight).

**Activation gate (CRITICAL):** this skill must NOT run on an empty inbox.
- `ls "00 - INBOX/"*.md 2>/dev/null | wc -l >= 1` (at least one capture file exists)

If zero files, skill aborts with: "inbox-processor: inbox is empty, nothing to process."

## Process

For each file in `00 - INBOX/`:

1. **Read the file.** Identify the type:
   - **Permanent idea**: a self-contained concept worth keeping long-term (1 idea, your words, no time-bound context)
   - **Project note**: progress, decisions, blockers on an active project
   - **Reference**: lookup-style content (how-to, formula, contact, tool spec)
   - **Daily capture**: time-stamped observation belonging in today's daily note
   - **Task**: actionable to-do (move to project's `tasks.md`)

2. **Classify.** Use the file's content + filename. If ambiguous (could be 2+ types), flag it as `## Needs human classification` in the summary.

3. **Find related notes** by searching `02 - NOTES/permanent/`, `02 - NOTES/daily/`, `03 - PROJECTS/`, `05 - RESOURCES/` for matches (substring match on titles + content keywords). Use the `obsidian` skill's search_files tool. Skip this step if the capture is a Task or Daily capture (those don't need wikilinks).

4. **File into the right folder** with the right template applied:
   - Permanent → `02 - NOTES/permanent/YYYY-MM-DD-perm-slug.md` (use `permanent.md` template)
   - Project → `03 - PROJECTS/[matching-project]/notes/YYYY-MM-DD-slug.md` OR append to existing project note
   - Reference → `05 - RESOURCES/topics/slug.md` (use `reference.md` template)
   - Daily → append to `02 - NOTES/daily/YYYY-MM-DD.md` (today's daily note, under `## Captures`)
   - Task → append to `03 - PROJECTS/[matching-project]/tasks.md` (preserve checkbox syntax)

5. **Add at least one wikilink** in the filed note pointing to a related note. For permanent notes, add at least 2 wikilinks (the spec calls for "atomic but practical" — one connection is too thin).

6. **Archive the original** by moving it to `07 - ARCHIVE/inbox-processed/YYYY-MM-DD/`. Preserve filename + content exactly. If multiple captures arrived on the same day, batch them into a subfolder.

7. **Continue until inbox is empty** (no captures left in `00 - INBOX/`).

8. **Write a summary** to `06 - HERMES-OUTPUTS/analyses/YYYY-MM-DD-inbox-summary.md` listing what was processed and where each capture landed.

9. **Quality Gate** before saving the summary — see below. If FAIL, revise once. If still FAIL, save with `FAILED-` prefix.

## Output format (summary)

```markdown
---
type: hermes-output
kind: inbox-summary
date: YYYY-MM-DD
generated-by: inbox-processor v0.1
input-count: N
output-count: M
flagged-count: K
---

# Inbox Summary — YYYY-MM-DD

## Processed (N files)
- `00 - INBOX/capture-name-1.md` → `02 - NOTES/permanent/2026-06-24-perm-name-1.md` (linked to [[2026-06-23-perm-related-note]])
- `00 - INBOX/capture-name-2.md` → `03 - PROJECTS/[project]/notes/2026-06-24-event.md`
- ...

## Flagged for human classification (K files)
- `00 - INBOX/ambiguous-capture.md` — could be permanent or project; **recommended classification: project, reason: contains dated event**

## Skipped
- (any captures skipped, with reason — e.g., empty file, duplicate of existing note)

## Vault state after run
- `00 - INBOX/` files: 0 (was N)
- `07 - ARCHIVE/inbox-processed/YYYY-MM-DD/` files: N (input count)
- New permanent notes: X
- New project notes: Y
- New references: Z
```

## Quality Gate

| Criterion | Check |
|---|---|
| **Inbox is empty after run** | `ls "00 - INBOX/"*.md 2>/dev/null | wc -l` returns 0 (or only `.gitkeep`) |
| **Every filed note has ≥1 wikilink** | For permanent notes specifically: at least 2 wikilinks. For other types: at least 1 wikilink |
| **Originals archived, not deleted** | `07 - ARCHIVE/inbox-processed/YYYY-MM-DD/` contains the originals |
| **Summary lists every input** | Count of items in "Processed" + "Flagged" + "Skipped" == input-count frontmatter field |
| **No secret leakage** | `grep -iE "<secrets-pattern>"` against the new permanent notes finds nothing (matches the `attachments/secrets/` gitignore convention) |
| **Path correctness** | Permanent notes go to `02 - NOTES/permanent/`, references to `05 - RESOURCES/topics/`, etc. — no misroutes |

If FAIL, fix the issue (move to correct folder, add missing wikilink, restore from archive if needed) and re-run the gate. If still FAIL after 2 attempts, save with `FAILED-` prefix.

## Output

A single summary file at `06 - HERMES-OUTPUTS/analyses/YYYY-MM-DD-inbox-summary.md`. Side effects: N files moved from `00 - INBOX/` to `07 - ARCHIVE/inbox-processed/YYYY-MM-DD/`, plus new notes in their target folders. No content is deleted; only moved.

## See also

- `08 - SYSTEM/skills/specs/vault-morning-brief.md` — runs first in the morning; surfaces items Adam needs to address
- `08 - SYSTEM/skills/specs/connection-finder.md` — companion skill that finds links between recently-processed notes and older permanent notes
- `08 - SYSTEM/CLAUDE.md` rule #5 — atomic notes belong to humans; agent output belongs in `06 - HERMES-OUTPUTS/`
- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` Phase 5 — origin and trigger-condition rationale