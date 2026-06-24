# Obsidian Hivemind — Vault Template

> A generic, identity-free Obsidian vault template for a Hermes-integrated personal AI-OS. Fork this repo, replace the placeholder identity in `08 - SYSTEM/CLAUDE.md`, and start capturing.

## What this is

A retrieval-first Obsidian vault structure optimised for:

- **Long-term personal knowledge management** (PKM) — atomic notes, daily journals, literature, projects.
- **AI integration** — built-in conventions so AI agents (Hermes, Claude, etc.) can read, write, and synthesize without explicit per-vault instruction.
- **Multi-device sync over Tailscale** — folder structure works whether the source of truth is local, on a VPS, or on a NAS.

This is **not** a turnkey system. It's a structure and a set of conventions. You populate it with your own notes; the value compounds as you maintain the conventions.

## Quick start

1. **Fork this repo** to a private repo under your account.
2. **Edit `08 - SYSTEM/CLAUDE.md`** — replace the `## Identity` placeholder with your name, domain, timezone, and languages. Replace `## Primary Intellectual Interests` with your top 5–10 topics.
3. **Open in Obsidian** — desktop or mobile. The folder structure is the vault.
4. **Install recommended plugins** (see `08 - SYSTEM/CLAUDE.md` §Sync Model for notes):
   - **Templater** — powers the 8 templates in `08 - SYSTEM/templates/`
   - **Dataview** — query notes by frontmatter properties
   - **Obsidian Git** — auto-commit and push
5. **Configure Templater** to point at `08 - SYSTEM/templates/`. Enable "Trigger on file creation" if you want templates to auto-apply.
6. **Capture** — drop raw thoughts in `00 - INBOX/`. Process them into permanent notes over time.

## What's in the box

- **9 top-level folders** — `00 - INBOX/` through `09 - MOCS/`. See `08 - SYSTEM/CLAUDE.md` for the navigation index.
- **8 note templates** — daily, permanent, project, literature, meeting, decision, decision-review, reference.
- **1 vault manual** — `08 - SYSTEM/CLAUDE.md`.
- **1 setup notes file** — `00 - INBOX/2026-06-24 — Vault Setup Notes.md`.
- **1 gitignore** — `.gitignore`, covers secrets, per-device state, heavy media.

## What's NOT in the box (intentionally)

- **No real notes.** This is a template; you bring the content.
- **No identity.** Placeholders only. Fill in `08 - SYSTEM/CLAUDE.md` Identity section.
- **No plugin configs.** Plugin data files are per-device; configure on each Obsidian install.
- **No skill implementations.** The folder structure for AI skills is reserved in `08 - SYSTEM/skills/`, but no code ships with the template.
- **No sync topology.** Where this vault physically lives is your choice. See `08 - SYSTEM/CLAUDE.md` for the model.

## Conventions

The full conventions are in `08 - SYSTEM/CLAUDE.md`. Short version:

- **Filename:** `YYYY-MM-DD-TYPE-slug.md` for dated; `topic-name.md` for evergreen.
- **Frontmatter:** `type`, `created`, `updated`, `tags` minimum.
- **Wikilinks:** `[[Note Name]]` for internal references.
- **No secrets in notes.** Drop in `attachments/secrets/` (gitignored).
- **One idea per permanent note.** Atomic notes are the unit of value.

## Why "hivemind"

The metaphor: many notes, one mind. Each note is a single neuron; the wikilinks, tags, and frontmatter are the synapses. The vault as a whole is the emergent mind — a structure that thinks better than any individual note could.

## License

MIT. Fork freely. Attribute if you redistribute. No warranty.

## Acknowledgements

- Built on top of the [Obsidian](https://obsidian.md) markdown editor.
- Designed for use with [Hermes Agent](https://hermes-agent.nousresearch.com/) or any AI agent that reads/writes markdown.
- Folder structure inspired by the masterclass PKM literature; conventions and templates refined through actual use.