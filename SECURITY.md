# Security

Do not report tokens, authorization headers, private Issue content, or other secrets in a public GitHub Issue.

Revoke an exposed Nexos personal access token immediately from the personal access token page. Send a sanitized private report through [GitHub Security Advisories](https://github.com/nvlsoft-ca/nexos-codex/security/advisories/new) with the affected plugin version, reproduction steps, and non-secret evidence.

The public plugin never contains a personal access token. Installing it does not grant access to a Nexos Workspace; authorization is evaluated by the hosted Nexos service for the current user on every request.
