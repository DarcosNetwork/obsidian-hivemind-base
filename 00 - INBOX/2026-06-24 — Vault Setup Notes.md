# 2026-06-24 — Vault Setup Notes (Template)

> First note in the template vault. Captures *what was set up at template-creation time and what's open for the fork owner* so future-you (or future-Hermes) can reconstruct the decisions without asking.

> **You are reading the template version.** If you forked this vault for personal use, replace the contents of this file with your own setup notes after first clone.

## What's running in this template

- **Vault path:** wherever you cloned this repo.
- **Remote:** the GitHub repo you forked from.
- **Sync model:** depends on your topology (VPS source of truth, local source of truth, etc.). The `08 - SYSTEM/device-setup.md` doc captures the specifics.
- **Folder structure:** 9 top-level folders. See `08 - SYSTEM/CLAUDE.md` for the navigation index.

## What's open for the fork owner to decide

These are choices you need to make *for your fork*:

1. **Identity in CLAUDE.md.** Replace the placeholder `## Identity` section with your own name, domain, timezone, languages.
2. **Primary Intellectual Interests.** Populate the section with your top 5–10 topics. Review quarterly.
3. **Plugin install.** Templater, Dataview, and Obsidian Git are recommended for the workflow to work end-to-end. Add others as friction appears.
4. **Templater trigger settings.** Configure whether templates auto-apply on new note creation or only on demand.
5. **Sync topology.** Where does this vault physically live? Who's the source of truth? How do devices reach it?
6. **AI integration.** Are you wiring this to an AI agent (Hermes, Claude, etc.)? If so, which skills do you want to enable first?

## Conventions shipped with this template

- **Filenames:** `YYYY-MM-DD-TYPE-slug.md` for dated notes; `topic-name.md` for evergreen references.
- **Frontmatter:** `type`, `created`, `updated`, `tags` on every note. Note-type-specific additions documented in `CLAUDE.md`.
- **Wikilinks:** `[[Note Name]]` for internal references; Obsidian resolves them.
- **Sensitive material:** never in notes. If a secret must live somewhere, drop it in `attachments/secrets/` (gitignored) and link to the path from the note.

## Operating notes

- This vault is template-clean: no real notes, no identity information, no specific tooling wiring.
- After forking, your first action should be to edit `08 - SYSTEM/CLAUDE.md` Identity section and rename this setup-notes file.
- See `08 - SYSTEM/CLAUDE.md` for full conventions and intelligence instructions for AI agents.