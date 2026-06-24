---
type: system-doc
file: README.md
audience: [human, AI agent]
status: active
---
# Vault-Aware Skills — Architecture

This folder contains the **specifications** for the 5 vault-aware skills that orchestrate daily/weekly vault workflows. Each spec lives in this folder as readable markdown. The actual implementations live in `~/.hermes/skills/` (Hermes-internal, gitignored, not part of the vault).

## Why split: specs in vault, code in ~/.hermes/

Two homes, two purposes:

| Home | Purpose | Version-controlled |
|---|---|---|
| `08 - SYSTEM/skills/specs/` (this folder) | Readable specification of what each skill does, when it runs, and what it produces. The "why" and "what." | ✅ Tracked in vault git history |
| `~/.hermes/skills/` | Executable skill code. The "how" — the actual procedure Hermes runs. | ❌ Hermes-internal, lives outside the vault |

**Rationale:** the spec is a document the vault owner can read, edit, and review without touching code. Changes to the spec (e.g., "add a Quality Gate for X") are tracked in the vault's git log alongside the notes themselves. The implementation can be regenerated from the spec without losing intent.

This resolves the historical open question about skill placement: "should skills live in the vault or in `~/.hermes/skills/`?" — both. Specs in vault, code in Hermes.

## The 5 skills

| Spec file | Purpose | Trigger gate |
|---|---|---|
| [`vault-morning-brief.md`](vault-morning-brief.md) | 60-second morning brief grounded in the vault | ≥10 daily notes + populated CLAUDE.md Identity |
| [`inbox-processor.md`](inbox-processor.md) | Triage raw captures into atomic notes / project notes / references / tasks / daily entries | ≥1 file in `00 - INBOX/` |
| [`project-health.md`](project-health.md) | Weekly audit of every active project | ≥3 active projects |
| [`connection-finder.md`](connection-finder.md) | Surface non-obvious links between recent and older notes | ≥15 permanent notes + ≥3 modified-this-week notes |
| [`weekly-synthesis.md`](weekly-synthesis.md) | Cross-cutting synthesis of the week's activity | ≥5 daily notes from this week |

**All 5 skills start with `status: scaffold` and `disabled by default` for scheduled runs.** Manual invocation always works. Scheduled runs only fire if the activation gate is met AND scheduling is explicitly enabled (a future setting; not yet wired).

## The 4-section skill format

Every spec uses this exact shape (validated against real workflow patterns):

1. **Purpose** — what it does, in one sentence
2. **Trigger** — manual phrase, schedule, and the activation gate that prevents empty-vault noise
3. **Process** — step-by-step procedure
4. **Output** — what it produces, where it saves, format, Quality Gate

## Quality Gates (the convention that ties them together)

Every spec includes a **Quality Gate** step before saving output. The gate is a table of explicit pass criteria with concrete checks. If any criterion fails, the skill revises once. If still fails, the output is saved with `FAILED-` prefix and a `## Failure Notes` section explaining which criteria failed.

This pattern forces skill authors to specify success criteria upfront rather than discovering quality issues after the fact. Empty-vault runs that produce empty output fail the Quality Gate and get flagged for review, rather than silently landing in `06 - HERMES-OUTPUTS/` as useless boilerplate.

## What this folder does NOT contain

- **Skill implementations.** Those live in `~/.hermes/skills/<skill-name>/`. Code, scripts, any executable.
- **Cron jobs.** Skill scheduling is configured at the Hermes agent level (separate from specs).
- **Run logs.** Skill execution logs go to `06 - HERMES-OUTPUTS/` per the output schemas in each spec.

## How to add a new skill

1. Copy any existing spec in this folder as a starting template.
2. Rename and rewrite the 4 sections for the new skill's purpose.
3. Add a Quality Gate with at least 3 concrete pass criteria.
4. Add an entry to the table above.
5. Commit to the vault with a `feat: add <skill-name> spec` commit message.
6. (Separately) write the implementation in `~/.hermes/skills/<skill-name>/` — this is not in scope for the vault git history.

## How to evolve an existing spec

Just edit the markdown. Skill specs are documentation; they don't need versioning discipline beyond the vault's existing commit-and-push workflow. If a spec change is non-trivial (e.g., changing the Output format that downstream skills depend on), call it out in the commit message.

## See also

- The plan that built this vault (Hermes-internal) — see `/root/.hermes/plans/` for the most recent `2026-06-24_obsidian-second-brain.md`
- `08 - SYSTEM/CLAUDE.md` — vault manual, especially Intelligence Instructions (rules 5, 9, 10 cover agent output paths and skill conventions)
- `08 - SYSTEM/device-setup.md` — how to enable the skills on each device once implementations exist