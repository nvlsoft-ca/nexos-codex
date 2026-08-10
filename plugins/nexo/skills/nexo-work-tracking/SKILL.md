---
name: nexo-work-tracking
description: Manage Nexos workspaces, teams, projects, milestones, issues, labels, comments, and attachment metadata through Nexos MCP. Use whenever a user invokes Nexos, asks to inspect or change Nexos work, or when presenting Nexos Issues or Milestones so canonical links and concurrency rules are applied.
---

# Nexos Work Tracking

Use Nexos MCP tools for Nexos product data. Respect the authenticated user's current workspace permissions and return the canonical affected entity after writes.

## Present Issue links

- Render every Nexos Issue identifier in user-visible prose as `[identifier](webUrl)`.
- Apply this rule to commentary and progress updates, final answers, summaries, lists, blockers, and next steps.
- Use the exact canonical `issue.webUrl` returned by Nexos. Preserve canonical Markdown links already returned by a tool.
- If only an identifier is known, call `get_issue` in the accessible workspace to obtain `issue.webUrl` before presenting the Issue.
- Never reconstruct, infer, or guess an Issue URL. If Nexos cannot return `webUrl`, show the plain identifier and state briefly that the link is unavailable.

Example: write `[NVL-42](https://nexo.example/.../issues/NVL-42)`, not `NVL-42`, when `webUrl` is available.

## Present Milestone links

- Render every Nexos Milestone name in user-visible prose as `[name](webUrl)`.
- Apply this rule to commentary and progress updates, final answers, summaries, lists, blockers, and next steps.
- Use the exact canonical `milestone.webUrl` returned by Nexos. Preserve canonical Markdown links already returned by a tool.
- If only a Milestone ID is known, call `get_milestone` in the accessible workspace to obtain its name and `milestone.webUrl` before presenting it.
- Never reconstruct, infer, or guess a Milestone URL. If Nexos cannot return `webUrl`, show the plain name or ID and state briefly that the link is unavailable.

Example: write `[Sprint 8](https://nexo.example/.../milestones/...)`, not `Sprint 8`, when `webUrl` is available.

## Use Nexos safely

- Discover accessible workspaces before making workspace-scoped calls when the active workspace is unknown.
- Paginate list and search tools until `nextCursor` is null when the user asks for complete results.
- Read the latest entity and use its opaque `version` before update, archive, or restore operations.
- Do not mutate Nexos while answering a read-only request.
- Do not expose authentication tokens or authorization headers.

## Use Labels

- Call `list_labels` to resolve active workspace Label IDs before classifying an Issue.
- `create_label` accepts a workspace and name, requires Owner/Admin access, and returns the API-generated initial color. Use Nexos Web for later recolor, rename, archive, or restore actions.
- Pass optional `labelIds` to `create_issue` to attach up to 25 active Labels atomically during creation. Labels must belong to the Issue workspace.

## Handle Issue attachments safely

- `list_issue_attachments` returns bounded canonical metadata only.
- `request_attachment_upload` accepts the original name, allowlisted media type,
  exact size, and lowercase SHA-256 digest. Never pass a local path, file bytes,
  base64 content, or a client-supplied URL.
- Transfer bytes directly to the exact temporary PUT target with every returned
  header, then call `confirm_attachment_upload`. Nexos MCP never reads or proxies
  the file.
- `request_attachment_download` returns a temporary read-only target. Use it
  immediately and never persist, log, quote, or expose the signed URL beyond the
  requested transfer.
- Attachment removal is intentionally unavailable through MCP. Use Nexos Web or
  the authorized public API workflow when removal is required.
