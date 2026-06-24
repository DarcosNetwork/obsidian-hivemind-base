# 2026-06-24 — Vault Setup Notes

> First note in the vault. Captures *what was set up, why, and what's open* so future-you (or future-Hermes) can reconstruct the decisions without asking.

## What's running

- **Vault path:** `/root/Documents/Obsidian Vault/`
- **Remote:** private GitHub repo under the `DarcosNetwork` account (HTTPS)
- **Sync model:** VPS is source of truth. Desktop/iPad/iPhone reach it via Tailscale + SSH/SFTP. Versioning through git commits (you push after each session).
- **Mobile access:** Obsidian mobile clients connect over Tailscale to `vps:100.96.209.60:22` (SFTP). Same `git` workflow for version history.

## What was decided

| Decision | Choice | Why |
|---|---|---|
| Vault location | VPS, served via Tailscale | Matches "VPS is single-tenant, Tailscale-only ingress" model |
| Versioning | Private GitHub repo | Auditable history, branchable experiments, multi-device |
| File format | Markdown (.md) | Hermes reads/writes these natively, no Obsidian-binary lock-in |
| Ignored in git | `.obsidian/workspace.json`, plugins data, secrets folder | Per-device local state + sensitive material |

## What's intentionally NOT decided yet

- **Folder structure (PARA vs Zettelkasten vs hybrid)** — deferred. Will revisit after the vault has a few weeks of real content. Empty structure is easy to redesign; a half-built one is painful to migrate.
- **Templates** — none yet. Wait until we know what recurring note types we actually produce.
- **Plugins** — vanilla Obsidian for now. Add only when a specific friction point surfaces.
- **Hermes routing** — `obsidian` skill loads but doesn't auto-route notes anywhere yet. Until structure is decided, all notes land in `00-Inbox/`.

## Open questions for next session

1. **Repo name on GitHub** — `obsidian-vault`? `second-brain`? `darcos-pkm`? Your call.
2. **Commit cadence** — daily auto-commit via cron, or manual?
3. **Should `attachments/` be committed or excluded?** Currently committed by default except heavy media. Easy to flip later.
4. **Folder structure conversation** — when ready, we'll go through PARA/Zettelkasten/hybrid properly. No rush.

## Operating notes

- Hermes reads `OBSIDIAN_VAULT_PATH` from `~/.hermes/.env` — set this session.
- To open the vault in Obsidian desktop over Tailscale: SFTP host `vps`, port `22`, key = your Tailscale SSH key, path `/root/Documents/Obsidian Vault`.
- iOS Obsidian: add remote SFTP folder with the same coordinates. Files appear in Obsidian's "Open vault → SFTP" picker.