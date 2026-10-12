# Security

Do not report tokens, authorization headers, private Issue content, or other secrets in a public GitHub Issue.

Revoke an exposed Nexos personal access token immediately from the personal access token page. Send a sanitized private report through [GitHub Security Advisories](https://github.com/nvlsoft-ca/nexos-codex/security/advisories/new) with the affected plugin version, reproduction steps, and non-secret evidence.

The public plugins never contain a personal access token. `nexos` reads its token only from the `NEXOS_MCP_TOKEN` environment variable and `nexos-dev` only from `NEXO_MCP_TOKEN`, including through the `Authorization` header of the Claude Code configuration. Installing one does not grant access to a Nexos Workspace; authorization is evaluated by the hosted Nexos service for the current user on every request.
