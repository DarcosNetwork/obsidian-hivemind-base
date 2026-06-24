# Second Brain

> Personal knowledge management vault. Single source of truth lives on the VPS, versioned via private GitHub repo, accessed over Tailscale from desktop and mobile.

## Quick links

- [[2026-06-24 — Vault Setup Notes]] — what this is, what was decided, what's open
- [[00-Inbox/]] — landing pad for anything not yet routed

## Conventions (draft — will evolve)

- **Filenames:** `YYYY-MM-DD — Title.md` for dated notes; `topic-name.md` for evergreen ones.
- **Wikilinks:** `[[Note Name]]` for internal references; Obsidian resolves them.
- **Frontmatter:** minimal at first. Add `tags`, `status`, `project` only when needed.
- **Sensitive material:** never in notes. If a secret must live somewhere, drop it in `attachments/secrets/` (gitignored) and link to the path from the note.

## How to work with this vault

- **From desktop/iPad/iPhone:** connect via Obsidian's SFTP remote over Tailscale. See setup notes for coordinates.
- **From Hermes on the VPS:** all file tools work against `$OBSIDIAN_VAULT_PATH` automatically once the env var is set.
- **Versioning:** `git add -A && git commit -m "msg" && git push` from the vault root. Cron can automate this later.