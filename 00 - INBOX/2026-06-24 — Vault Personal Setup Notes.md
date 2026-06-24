# 2026-06-24 — Vault Personal Setup Notes

> First note in the personal vault. Captures *what this is* and *what's open* so future-me can reconstruct the setup without asking.

## What this is

This is my personal Obsidian vault, structured for a Hermes-powered personal AI-OS. It started from the public template `obsidian-hivemind-base` (clean, identity-free, MIT-licensed) and has been personalized with my identity and conventions in `08 - SYSTEM/CLAUDE.md`.

## Two-repo setup

| Repo | Visibility | Purpose |
|---|---|---|
| `DarcosNetwork/obsidian-hivemind-base` | Public | Generic template. Safe to share. |
| `DarcosNetwork/obsidian-hivemind` (this one) | Private | My working vault. Personal notes, identity, conventions. |

**Sync flow:** when the public template gets updates I want, I pull/cherry-pick into this repo. I never push personal changes back to the public repo — that would leak my identity.

## What's set up so far

- ✅ Folder structure (9 top-level folders per the masterclass spec)
- ✅ 8 note templates in `08 - SYSTEM/templates/`
- ✅ CLAUDE.md populated with my identity and primary interests
- ✅ `.gitignore` covering secrets, per-device state, heavy media
- ✅ Tailscale SSH access from desktop/mobile (Phase 4 of the plan)

## What's intentionally NOT set up yet

- **Phase 3 plugins** — Templater, Dataview, Obsidian Git config files. Awaiting review of the existing structure.
- **Phase 5 skills** — Hermes vault-aware skills (inbox-processor, morning-brief, weekly-synthesis, etc.). Deferred until I have ≥2 weeks of real vault content.
- **First MOC in `09 - MOCS/`** — will emerge when a topic has ≥15–20 permanent notes.

## Plan and history

- `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` — full plan that built this vault
- The `obsidian-hivemind-base` Git history shows the original scaffolding and template-cleanup commits; my personal additions are after the rename point.