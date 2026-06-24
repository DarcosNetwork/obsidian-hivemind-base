# Device Setup

> How to connect desktop/mobile clients to this vault. This file is **fork-specific**: the sync topology (where the vault lives, how devices reach it, authentication) varies per fork. Configure after first clone.

> **Template note:** This is a stub. The real device-setup.md in a working fork will document the specific host, auth method, and credentials needed for that fork's topology. Forks using Tailscale for example would document the Tailscale IP, SSH key model, and SFTP credentials here.

## What goes in this file (when configured)

- **Vault source-of-truth location:** where the canonical vault lives (VPS, local, NAS).
- **Authentication model:** how devices authenticate (Tailscale SSH, dedicated keypair, etc.).
- **Desktop setup:** how to open the vault in Obsidian desktop (SFTP host, port, path, key).
- **iOS setup:** same coordinates for Obsidian mobile on iPhone/iPad.
- **Git credentials:** how to wire GitHub credentials on each device so Obsidian Git can push.
- **Verification:** end-to-end test that confirms sync works.

## Why it's fork-specific

The sync topology is the single most opinionated part of a working vault. Every choice — VPS vs. local, Tailscale vs. public, SSH vs. Obsidian Sync — has different setup steps. A generic template can't prescribe one without excluding the others.

Configure this file before opening the vault on a second device.