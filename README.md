# Nexos for Codex

This public beta marketplace installs the `nexo` plugin for Codex. The bundle contains only the plugin manifest, remote MCP configuration, skill instructions, and brand assets. It does not contain the Nexos application source code or any credential.

Installing the plugin does not grant access to a Nexos Workspace. A beta tester must first receive an invitation, join an active Workspace, create a personal access token in Nexos, and store it in the `NEXO_MCP_TOKEN` environment variable.

## Install the beta

These commands are verified against the Codex CLI version identified in the release notes:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref main
codex plugin add nexo@nexos
```

Restart Codex, open a new task, invoke `@nexo`, and ask it to show your current Nexos user.

## Update

```powershell
codex plugin marketplace upgrade nexos
codex plugin add nexo@nexos
```

Restart Codex and open a new task after every update.

## Install the fixed download

The Git marketplace is the primary channel. For diagnosis or offline transfer, download both files and verify the checksum before extracting the ZIP:

- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip`
- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip.sha256`

Add the extracted directory as a local marketplace, then install `nexo@nexos`:

```powershell
codex plugin marketplace add C:\path\to\extracted\nexos-codex
codex plugin add nexo@nexos
```

## Authentication boundary

The plugin connects only to `https://mcp.dev.nexos.ca/mcp` and reads the token only from `NEXO_MCP_TOKEN`. Never paste a token into the plugin, a Git repository, `config.toml`, an Issue, or a support message. Tokens are personal, expire, and can be revoked from Nexos.

## Version rollback

Git tags and GitHub releases are immutable. To reinstall a specific version in a clean Codex marketplace configuration, use its release tag instead of `main`, for example:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref v0.1.0-beta.2
codex plugin add nexo@nexos
```

See [RELEASE_NOTES.md](RELEASE_NOTES.md) for compatibility and [SECURITY.md](SECURITY.md) for reporting guidance.
