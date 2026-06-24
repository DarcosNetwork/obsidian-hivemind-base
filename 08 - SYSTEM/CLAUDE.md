---
type: system
file: CLAUDE.md
audience: [human, AI agent]
status: personal
---
# Vault Intelligence System — CLAUDE.md

> This file is the vault manual. Humans read it to remember conventions; AI agents (Hermes, Claude, future subagents) read it to understand what the vault expects of them. Keep it current; every change to this file is a vault-wide convention change.

## Identity

- **Name:** Adam Darcos
- **Primary domain:** Medical student — Semmelweis University, Budapest
- **Stage:** Final-year (Year 6)
- **Timezone:** Europe/Budapest (CET/CEST)
- **Languages:** English (primary), Hungarian (native), German + Spanish (beginner)

## Vault Purpose

Long-term personal knowledge management for a Hermes-powered personal AI-OS. The vault holds permanent knowledge; Hermes orchestrates tasks, automation, and synthesis on top of it. Optimised for **retrieval-first** — the goal is to find any note in under 30 seconds.

## Primary Intellectual Interests

1. Clinical reasoning and differential diagnosis
2. Cardiology
3. Neuroanatomy and cognition
4. Evidence-based medicine and statistics
5. Personal performance (sleep, focus, exercise)

## Active Projects

*[Section auto-maintained as projects land in `03 - PROJECTS/`. Empty by design at launch.]*

## Active Theses

*[Section auto-maintained after ≥4 weeks of real thinking captured. Empty by design at launch.]*

## Vault Navigation

| Folder | Purpose |
|---|---|
| `00 - INBOX/` | Raw captures. Land here first; process later. |
| `01 - LIBRARY/` | Reading list and literature catalog. |
| `02 - NOTES/permanent/` | Atomic notes — one idea per note. The core of the system. |
| `02 - NOTES/daily/` | Daily journal entries. |
| `02 - NOTES/meetings/` | Meeting notes. |
| `02 - NOTES/study/` | Study-session captures. |
| `03 - PROJECTS/` | Active multi-step projects with their own `overview.md`, `tasks.md`, `notes/`. |
| `04 - AREAS/` | Ongoing responsibilities (med-school, business, AI-OS, health, finances). |
| `05 - RESOURCES/` | Reference topics, people, tools. |
| `06 - HERMES-OUTPUTS/` | Agent-generated content — isolated from human notes. |
| `06 - HERMES-OUTPUTS/briefings/` | Morning briefs. |
| `06 - HERMES-OUTPUTS/analyses/` | Inbox summaries, connection-finder output. |
| `06 - HERMES-OUTPUTS/syntheses/` | Weekly/monthly syntheses. |
| `06 - HERMES-OUTPUTS/reviews/` | Project health reviews. |
| `07 - ARCHIVE/` | Completed/old material. |
| `08 - SYSTEM/` | CLAUDE.md, templates, skill specs, device-setup docs. |
| `08 - SYSTEM/templates/` | Templater templates. 8 note types. |
| `08 - SYSTEM/skills/` | Spec docs for Hermes vault-aware skills. |
| `09 - MOCS/` | Maps of Content — navigational hubs. Build after a topic has ≥15–20 permanent notes. |

## Note Type Conventions

| Type | Folder | Filename | Frontmatter anchors |
|---|---|---|---|
| Daily | `02 - NOTES/daily/` | `YYYY-MM-DD.md` | `type: daily`, `date`, `day` |
| Permanent | `02 - NOTES/permanent/` | `YYYY-MM-DD-perm-slug.md` | `type: permanent`, `status`, `tags`, `connections` |
| Meeting | `02 - NOTES/meetings/` | `YYYY-MM-DD-meet-slug.md` | `type: meeting`, `with`, `context` |
| Literature | `01 - LIBRARY/` | `YYYY-MM-DD-lit-slug.md` | `type: literature`, `citation`, `source-type` |
| Project | `03 - PROJECTS/[name]/` | `overview.md`, `tasks.md` | `type: project`, `status`, `area`, `completion` |
| Decision | `02 - NOTES/permanent/` (or `04 - AREAS/`) | `YYYY-MM-DD-dec-slug.md` | `type: decision`, `chosen`, `assumption`, `review_date` |
| Decision Review | `02 - NOTES/permanent/` | `YYYY-MM-DD-dec-review-slug.md` | `type: decision-review`, `original-decision` |
| Reference | `05 - RESOURCES/topics/` | `slug.md` | `type: reference`, `subject` |

**Filename rule:** always include date prefix. Slugs lowercase, hyphenated. `TYPE` code is optional (helps search); `slug` is required.

**Decision placement:** decision notes can live in `02 - NOTES/permanent/` (cross-cutting) or `04 - AREAS/<area>/` (area-specific). Pick by reach: if the decision affects one area, place it there; if it affects multiple, place it in permanent.

## Tagging Conventions

- **Structured prefixes only.** `p:` for high-level domains (`p:medicine`, `p:finance`), `system:` for body systems, `level:` for study level, `status:` for workflow states, `mode:` for study mode (`mode:anki`, `mode:lecture`).
- **No one-off free-form tags.** Only create a tag if it'll appear on ≥5 notes.
- **Dataview queries:** build queries against `type`, `status`, `tags`, `area` first; query against `connections` only when needed.

## Intelligence Instructions (for AI agents)

When acting on this vault:

1. **Finding connections:** only propose links that *change how notes are read together*. Surface-level co-occurrence is noise; semantic relevance is signal.
2. **Monitoring theses:** flag notes that contradict active theses. Don't over-flag noisy additions — a single instance isn't a contradiction.
3. **Generating syntheses:** add new structure or insight beyond what's in the existing notes. Don't paraphrase the source notes back.
4. **Detecting patterns:** require ≥3–4 independent notes before naming a pattern. Two notes is coincidence.
5. **Writing in the vault:** never write directly into `02 - NOTES/permanent/` from a synthesis without leaving a backlink in the source note. Atomic notes belong to humans; agent output belongs in `06 - HERMES-OUTPUTS/`.
6. **Modifying CLAUDE.md:** a change to this file is a vault-wide convention change. Log it in the commit message with full reasoning.
7. **Honoring secrets:** never write secrets into notes. The vault's `.gitignore` excludes `attachments/secrets/` for a reason. If you discover a secret in a note during processing, flag it to me and move it out of the tracked vault.
8. **Clinical claims:** when Hermes helps with medical content, default to conservative — cite the source note, never invent clinical recommendations, flag uncertainty explicitly. This vault is a study aid, not a clinical decision tool.

## Update Schedule

- **Active projects section:** weekly, after weekly review.
- **Active theses section:** monthly, only when a thesis has been tested against ≥3 new notes.
- **Primary interests section:** quarterly, only after deliberate re-evaluation.
- **Navigation / conventions:** as needed; bumps the file's git log.

## Sync Model

- **Source of truth:** VPS (`/root/Documents/Obsidian Vault/`, Tailscale IP `100.96.209.60`).
- **Public template:** `DarcosNetwork/obsidian-hivemind-base` (clean, identity-free, MIT-licensed).
- **This private working copy:** `DarcosNetwork/obsidian-hivemind`.
- **Update flow:** when public template gets updates, pull/cherry-pick into this repo. When this repo gets personal changes, do *not* push back to public.
- **Auto-commit:** Obsidian Git plugin, every 30 minutes.
- **Auto-push:** enabled on devices where credentials permit.
- **Manual override:** always available; nothing in this vault is gated on automation.

## Security

- Vault contents are personal and unencrypted at rest on the VPS. Acceptable per the "VPS is single-tenant, Tailscale-only ingress" model.
- Secrets must NEVER live in notes. Drop in `attachments/secrets/` (gitignored) and reference the path only.
- Patient-identifiable information must NEVER live in this vault. Use a separate secure storage for any PHI.

## See also

- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` — the plan that built this vault (Hermes-internal).
- `08 - SYSTEM/device-setup.md` — Tailscale SSH coordinates and per-device setup.
- `08 - SYSTEM/templates/` — the 8 note templates.