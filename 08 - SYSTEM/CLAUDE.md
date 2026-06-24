---
type: system
file: CLAUDE.md
audience: [human, AI agent]
status: template
---
# Vault Intelligence System — CLAUDE.md

> This file is the vault manual. Humans read it to remember conventions; AI agents (Hermes, Claude, future subagents) read it to understand what the vault expects of them. Keep it current; every change to this file is a vault-wide convention change.

> **Template note:** This is the generic, identity-free version of CLAUDE.md. Fork this vault, then replace the contents of the `## Identity` section with your own. Everything below applies to any vault built on this structure.

## Identity (replace with your own)

- **Name:** [Your name]
- **Primary domain:** [e.g., "Software engineer", "Medical student", "Researcher"]
- **Stage:** [Where you are in your field]
- **Timezone:** [IANA timezone, e.g., "America/New_York"]
- **Languages:** [List primary languages]

## Vault Purpose

Long-term personal knowledge management for an AI-assisted personal OS. The vault holds permanent knowledge; an AI agent (Hermes, Claude, or similar) orchestrates tasks, automation, and synthesis on top of it. Optimised for **retrieval-first** — the goal is to find any note in under 30 seconds.

## Primary Intellectual Interests

*[Section to maintain over time. Five to ten high-level topics. Should be reviewed quarterly.]*

1. 
2. 
3. 
4. 
5. 

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
| `04 - AREAS/` | Ongoing responsibilities (work, school, business, health, finances — fill with what's true for you). |
| `05 - RESOURCES/` | Reference topics, people, tools. |
| `06 - HERMES-OUTPUTS/` | Agent-generated content — isolated from human notes. |
| `06 - HERMES-OUTPUTS/briefings/` | Morning briefs. |
| `06 - HERMES-OUTPUTS/analyses/` | Inbox summaries, connection-finder output. |
| `06 - HERMES-OUTPUTS/syntheses/` | Weekly/monthly syntheses. |
| `06 - HERMES-OUTPUTS/reviews/` | Project health reviews. |
| `07 - ARCHIVE/` | Completed/old material. |
| `08 - SYSTEM/` | This file, templates, skill specs, device-setup docs. |
| `08 - SYSTEM/templates/` | Templater templates. Eight note types shipped with the template. |
| `08 - SYSTEM/skills/` | Spec docs for AI vault-aware skills. |
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
7. **Honoring secrets:** never write secrets into notes. The vault's `.gitignore` excludes `attachments/secrets/` for a reason. If you discover a secret in a note during processing, flag it to the user and move it out of the tracked vault.

## Update Schedule

- **Active projects section:** weekly, after weekly review.
- **Active theses section:** monthly, only when a thesis has been tested against ≥3 new notes.
- **Primary interests section:** quarterly, only after deliberate re-evaluation.
- **Navigation / conventions:** as needed; bumps the file's git log.

## Sync Model

- **Source of truth:** wherever you host this vault (a VPS, your local machine, a NAS). The `08 - SYSTEM/device-setup.md` document captures the specifics for your topology.
- **Versioning:** private git repo (this one). 
- **Auto-commit:** depends on the Obsidian Git plugin configuration in `.obsidian/plugins/obsidian-git/data.json`.
- **Auto-push:** enabled on devices where credentials permit.
- **Manual override:** always available; nothing in this vault is gated on automation.

## Security

- Vault contents are personal and unencrypted at rest on the host. Acceptable for single-tenant, controlled-access setups.
- Secrets must NEVER live in notes. The vault's `.gitignore` excludes `attachments/secrets/`; use that for anything sensitive.
- Personally identifiable information about other people (medical, financial, etc.) must NEVER live in this vault unless you have explicit consent and a documented need. Use a separate secure storage for any PHI / PII.

## See also

- `00 - INBOX/2026-06-24 — Vault Setup Notes.md` — what was set up at template creation time, what's open for the fork owner to decide.
- `08 - SYSTEM/device-setup.md` — how to connect desktop/mobile clients (set up after first fork).
- `08 - SYSTEM/templates/` — the 8 note templates shipped with this vault.