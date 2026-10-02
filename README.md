# Nexos for Codex and Claude Code

This public beta marketplace installs the `nexo` plugin for Codex and for Claude Code. The bundle contains only the plugin manifests, remote MCP configuration, skill instructions, and brand assets. It does not contain the Nexos application source code or any credential.

Installing the plugin does not grant access to a Nexos Workspace. A beta tester must first receive an invitation, join an active Workspace, create a personal access token in Nexos, and store it in the `NEXO_MCP_TOKEN` environment variable.

## Install the beta in Codex

These commands are verified against the Codex CLI version identified in the release notes:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref main
codex plugin add nexo@nexos
```

Restart Codex, open a new task, invoke `@nexo`, and ask it to show your current Nexos user.

## Install the beta in Claude Code

Set `NEXO_MCP_TOKEN` in your user environment before you start Claude Code, because the plugin reads it when Claude starts. In Windows PowerShell, this prompts for the token without echoing it or saving it in shell history:

```powershell
$token = Read-Host 'Nexos token' -AsSecureString
[Environment]::SetEnvironmentVariable('NEXO_MCP_TOKEN', [Net.NetworkCredential]::new('', $token).Password, 'User')
```

On macOS or Linux, export `NEXO_MCP_TOKEN` from your shell profile instead. Open a new terminal, then install the plugin:

```powershell
claude plugin marketplace add nvlsoft-ca/nexos-codex
claude plugin install nexo@nexos
```

Start Claude Code and ask it to show your current Nexos user. If `claude mcp list` reports `plugin:nexo:nexo` as failed with HTTP 401, the token is missing, expired, or revoked: create a new token in Nexos, set `NEXO_MCP_TOKEN` again, and restart Claude Code.

Claude Code is the supported Claude surface in this beta. Claude.ai chat and other Claude clients are not presented as supported, because the service authenticates with the personal token header described below.

## Update

In Codex:

```powershell
codex plugin marketplace upgrade nexos
codex plugin add nexo@nexos
```

In Claude Code:

```powershell
claude plugin marketplace update nexos
claude plugin update nexo@nexos
```

Restart the client and open a new task or session after every update.

## Install the fixed download

The Git marketplace is the primary channel. For diagnosis or offline transfer, download both files and verify the checksum before extracting the ZIP:

- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip`
- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip.sha256`

Add the extracted directory as a local marketplace, then install `nexo@nexos`:

```powershell
codex plugin marketplace add C:\path\to\extracted\nexos-codex
codex plugin add nexo@nexos
```

```powershell
claude plugin marketplace add C:\path\to\extracted\nexos-codex
claude plugin install nexo@nexos
```

## Authentication boundary

The plugin connects only to `https://mcp.dev.nexos.ca/mcp` and reads the token only from `NEXO_MCP_TOKEN`: Codex through `bearer_token_env_var`, and Claude Code through an `Authorization: Bearer ${NEXO_MCP_TOKEN}` header. Never paste a token into the plugin, a Git repository, `config.toml`, a Claude settings file, an Issue, or a support message. Tokens are personal, expire, and can be revoked from Nexos.

## Version rollback

Git tags and GitHub releases are immutable. To reinstall a specific version in a clean marketplace configuration, use its release tag instead of `main`, for example:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref v0.1.0-beta.4
codex plugin add nexo@nexos
```

```powershell
claude plugin marketplace add nvlsoft-ca/nexos-codex#v0.1.0-beta.4
claude plugin install nexo@nexos
```

Claude Code support starts with `0.1.0-beta.4`; earlier tags install only in Codex.

See [RELEASE_NOTES.md](RELEASE_NOTES.md) for compatibility and [SECURITY.md](SECURITY.md) for reporting guidance.
