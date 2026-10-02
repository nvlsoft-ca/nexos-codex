# Release notes

## 0.1.0-beta.4

- Adds Claude Code manifests so the same bundle installs as `nexo@nexos` in Claude Code.
- The Claude MCP entry authenticates with an `Authorization: Bearer ${NEXO_MCP_TOKEN}` header read from the environment; the Codex configuration is unchanged.
- Documents installation, update, and tag rollback for Claude Code, the supported Claude surface in this beta.
- Manifest validation and a clean install from the exported repository were verified with Claude Code `2.1.283` on Windows; the install reports one `nexo` MCP server, a missing token is rejected with HTTP 401, and with a personal token the server connects and a read-only current-user call succeeds.
- Claude Code support starts with this version; earlier tags install only in Codex.

## 0.1.0-beta.3

- Adds Nexos-only feedback submission, personal status tracking, and reviewer public responses.
- Keeps anonymous feedback write-only and excludes internal backlog routing from MCP outputs.

## 0.1.0-beta.2

- Supersedes `0.1.0-beta.1` for tag-based installation.
- Preserves the reviewed bundle bytes in Git so `RELEASE.json` integrity checks remain valid.
- Closed beta for Codex Desktop and the Codex CLI.

## 0.1.0-beta.1

- Closed beta for Codex Desktop and the Codex CLI.
- Requires a personal `NEXO_MCP_TOKEN` issued by the Nexos development service.
- Connects to `https://mcp.dev.nexos.ca/mcp` using Streamable HTTP.
- Installation and marketplace update commands verified with Codex Desktop build `26.803.5235.0` and `codex-cli 0.147.0-alpha.6.5` on Windows.
- This release is not submitted to the universal OpenAI plugin directory.
