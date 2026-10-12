# Nexos for Codex and Claude Code

This public beta marketplace, `nexos`, offers one plugin per Nexos environment for Codex and for Claude Code:

| Plugin | Connects to | Token variable |
| --- | --- | --- |
| `nexos` | Nexos at `https://nexos.ca` | `NEXOS_MCP_TOKEN` |
| `nexos-dev` | The Nexos development service at `https://dev.nexos.ca` | `NEXO_MCP_TOKEN` |

Install the plugin of the environment where you have an account. The `nexos` plugin connects once the Nexos service at nexos.ca opens; until then, closed-beta testers use `nexos-dev`. The bundle contains only the plugin manifests, remote MCP configuration, skill instructions, and brand assets. It does not contain the Nexos application source code or any credential.

Installing a plugin does not grant access to a Nexos Workspace. You must first receive an invitation, join an active Workspace, create a personal access token in the Nexos site of that environment, and store it in that plugin's token variable. A token works only in the environment that issued it.

The commands below use `nexos`. For the development service, use `nexos-dev` and `NEXO_MCP_TOKEN` instead.

## Install the beta in Codex

Set the token variable in your user environment before you start Codex. In Windows PowerShell, this prompts for the token without echoing it or saving it in shell history:

```powershell
$token = Read-Host 'Nexos token' -AsSecureString
[Environment]::SetEnvironmentVariable('NEXOS_MCP_TOKEN', [Net.NetworkCredential]::new('', $token).Password, 'User')
```

These commands are verified against the Codex CLI version identified in the release notes:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref main
codex plugin add nexos@nexos
```

Restart Codex, open a new task, invoke `@nexos`, and ask it to show your current Nexos user.

## Install the beta in Claude Code

Set the token variable as shown above before you start Claude Code, because the plugin reads it when Claude starts. On macOS or Linux, export it from your shell profile instead. Open a new terminal, then install the plugin:

```powershell
claude plugin marketplace add nvlsoft-ca/nexos-codex
claude plugin install nexos@nexos
```

Start Claude Code and ask it to show your current Nexos user. If `claude mcp list` reports `plugin:nexos:nexos` (or `plugin:nexos-dev:nexos-dev`) as failed with HTTP 401, the token is missing, expired, or revoked: create a new token in Nexos, set the variable again, and restart Claude Code.

Claude Code is the supported Claude surface in this beta. Claude.ai chat and other Claude clients are not presented as supported, because the service authenticates with the personal token header described below.

## Update

In Codex:

```powershell
codex plugin marketplace upgrade nexos
codex plugin add nexos@nexos
```

In Claude Code:

```powershell
claude plugin marketplace update nexos
claude plugin update nexos@nexos
```

Restart the client and open a new task or session after every update.

## Move from nexo@nexos

Versions up to `0.1.0-beta.4` installed one plugin named `nexo` for the development service. It is replaced by `nexos-dev`, which reads the same `NEXO_MCP_TOKEN`. Update the marketplace and install `nexos-dev`, then remove the old `nexo` plugin from Codex so only one development server loads:

```powershell
codex plugin marketplace upgrade nexos
codex plugin add nexos-dev@nexos
```

In Claude Code:

```powershell
claude plugin marketplace update nexos
claude plugin uninstall nexo@nexos
claude plugin install nexos-dev@nexos
```

## Install the fixed download

The Git marketplace is the primary channel. For diagnosis or offline transfer, download the ZIP and its checksum from the Nexos site you use, for example:

- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip`
- `https://dev.nexos.ca/downloads/nexos-codex-latest.zip.sha256`

Every Nexos site serves the same ZIP, which contains both plugins. Verify the checksum before extracting it, add the extracted directory as a local marketplace, and install the plugin of your environment:

```powershell
codex plugin marketplace add C:\path\to\extracted\nexos-codex
codex plugin add nexos@nexos
```

```powershell
claude plugin marketplace add C:\path\to\extracted\nexos-codex
claude plugin install nexos@nexos
```

## Authentication boundary

`nexos` connects only to `https://mcp.nexos.ca/mcp` and reads its token only from `NEXOS_MCP_TOKEN`. `nexos-dev` connects only to `https://mcp.dev.nexos.ca/mcp` and reads its token only from `NEXO_MCP_TOKEN`. Codex reads the variable through `bearer_token_env_var`, and Claude Code through an `Authorization: Bearer` header that names the variable. Never paste a token into the plugin, a Git repository, `config.toml`, a Claude settings file, an Issue, or a support message. Tokens are personal, expire, and can be revoked from Nexos.

## Version rollback

Git tags and GitHub releases are immutable. To reinstall a specific version in a clean marketplace configuration, use its release tag instead of `main`, for example:

```powershell
codex plugin marketplace add nvlsoft-ca/nexos-codex --ref v0.2.0-beta.1
codex plugin add nexos@nexos
```

```powershell
claude plugin marketplace add nvlsoft-ca/nexos-codex#v0.2.0-beta.1
claude plugin install nexos@nexos
```

The `nexos` and `nexos-dev` plugins start with `0.2.0-beta.1`. Earlier tags contain only `nexo`; Claude Code support starts with `0.1.0-beta.4`.

See [RELEASE_NOTES.md](RELEASE_NOTES.md) for compatibility and [SECURITY.md](SECURITY.md) for reporting guidance.
