# 2026-06-24 — Vault Setup Notes

> First note in the vault. Captures *what was set up, why, and what's open* so future-you (or future-Hermes) can reconstruct the decisions without asking.

## What's running

- **Vault path:** `/root/Documents/Obsidian Vault/`
- **Remote:** `https://github.com/DarcosNetwork/obsidian-hivemind` (private, created 2026-06-24)
- **Sync model:** VPS is source of truth. Desktop/iPad/iPhone reach it via Tailscale SSH/SFTP. Versioning through git commits (auto-commit via Obsidian Git plugin, manual push or auto-push).
- **Mobile access:** Obsidian mobile clients connect over Tailscale to `100.96.209.60:22` (SSH/SFTP). Auth = Tailscale identity (Option A from the plan).
- **Folder structure:** 9 top-level folders per spec section 2. See `08 - SYSTEM/CLAUDE.md` (Phase 2) for the navigation index.

## What was decided

| Decision | Choice | Why |
|---|---|---|
| Vault location | VPS, served via Tailscale | Matches "VPS is single-tenant, Tailscale-only ingress" model |
| Versioning | Private GitHub repo (`obsidian-hivemind`) | Auditable history, branchable experiments, multi-device |
| File format | Markdown (.md) | Hermes reads/writes these natively, no Obsidian-binary lock-in |
| Ignored in git | `.obsidian/workspace.json`, plugins data, secrets folder | Per-device local state + sensitive material |
| Folder structure | Full 9-folder spec (sections 2 of the masterclass proposal) | Deferred-then-resolved: spec was chosen over v0-minimal after the plan-phase conversation |
| Plugin scope (initial) | Templater, Dataview, Obsidian Git only | Add the rest of the spec's 14 plugins per-friction-need |
| Hermes skill scope | None at launch; skills live in `~/.hermes/skills/` with specs in `08 - SYSTEM/skills/specs/` | Hermes convention for code, vault convention for docs |

## What's intentionally NOT decided yet

- **Hermes vault-aware skills** (`vault-morning-brief`, `inbox-processor`, `project-health`, `connection-finder`, `weekly-synthesis`) — all deferred to Phase 5 of the plan. Trigger conditions documented in `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md`.
- **Active theses in CLAUDE.md** — populated after 4+ weeks of real thinking captured.
- **First MOC in `09 - MOCS/`** — emerges when a topic has ≥15–20 permanent notes. Currently empty.
- **Commit cadence** — Obsidian Git will auto-commit every 30 min; auto-push on. Override on device if needed.

## Where the plan lives

Full plan with phases, tasks, risks, open questions, and execution log: `/root/.hermes/plans/2026-06-24_obsidian-second-brain.md` (on the VPS, not in the vault — plans are Hermes-internal, not vault content).

## Operating notes

- Hermes reads `OBSIDIAN_VAULT_PATH` from `~/.hermes/.env` — set this session.
- To open the vault in Obsidian desktop over Tailscale: SSH host `100.96.209.60`, port `22`, user `root`, path `/root/Documents/Obsidian Vault`. (Device-setup guide lands in `08 - SYSTEM/device-setup.md` during Phase 4.)
- iOS Obsidian: add remote SFTP folder with the same coordinates. Files appear in Obsidian's "Open vault → SFTP" picker.