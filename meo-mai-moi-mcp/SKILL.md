---
name: meo-mai-moi-mcp
description: "Connect, authorize, and safely use Meo Mai Moi through its OAuth MCP gateway for pet profiles, care and health records, habits, microchips, sharing, rehoming or placement, helper profiles, messages, groups, pet finances, notifications, and account workflows. Use when a user asks an OAuth-capable agent such as Codex, Cursor, or OpenClaw to connect or authorize Meo Mai Moi, manage Meo data, choose scopes, list pets, coordinate pet care, or automate a pet-related workflow. In OpenClaw, configure the native MCP entry and initiate OAuth for the user when execution is available. Do not use for direct REST API or personal-token integrations; use the separate meo-mai-moi skill instead."
---

# Meo Mai Moi MCP

Use the OAuth MCP gateway for Meo Mai Moi: pet management for nerds. Meo Mai
Moi remains authoritative for product rules, permissions, sharing, placement,
and financial state; MCP is the agent-facing adapter.

## Connect

1. Check whether the Meo MCP tools or a saved `meo-mai-moi` server are already
   available. Reuse a valid connection instead of registering a duplicate.
2. When the user explicitly asks to connect or authorize Meo, treat that as
   authority to configure only the client's `meo-mai-moi` MCP entry. Initiate
   OAuth yourself when the client and execution policy allow it; do not hand
   routine setup commands back to the user.
3. Configure the remote Streamable HTTP server at
   `https://mcp.meo-mai-moi.com/mcp`, authenticate through OAuth, and discover
   tools before calling them.
4. After authorization, start with `list_pets` for ordinary pet-management
   work. A newly configured client may need a reload or a fresh agent turn
   before its MCP tools appear.

Never ask for an OAuth access token, refresh token, or Meo personal API token.
Client-native OAuth belongs in the client. A headless OpenClaw flow has a
narrow private-chat exception for its short-lived PKCE authorization URL and
single-use code; follow the handling rules in the OpenClaw reference.

Read [references/connection.md](references/connection.md) for client-neutral
setup and scope selection. When running in OpenClaw, read and follow
[references/openclaw.md](references/openclaw.md). Read
[references/safety.md](references/safety.md) before consequential writes or
when a tool returns a structured error.

## Work safely

- Request the narrowest domain scopes; do not treat a consent screen as a broad
  permission grant for unrelated work.
- For an otherwise-unspecified request to “manage my pets,” use the documented
  initial pet-management grant covering pet profiles, health records,
  microchips, and finances. For a narrower named task, request only its domain
  pair.
- Discover current state and resolve stable IDs before mutating it.
- For updates and deletes, preserve the version returned by the matching read.
- Use a new idempotency key for each intended write. Reuse it only to retry the
  exact same write after an uncertain transport failure.
- Treat sharing, rehoming/placement, messages, groups, finance, invitations,
  medical media, and personal profile data as sensitive.
- Verify a completed write with the relevant read tool. Do not claim success
  solely because a request was accepted.

Tool descriptions and annotations help with planning, but enforcement comes
from OAuth scopes, Meo permissions, stable targets, concurrency checks, and
post-write verification.

## Choose the right integration

Use this skill when the agent can connect to a remote OAuth MCP server. If the
user explicitly wants a direct REST integration backed by a user-managed Meo
personal API token, stop here and use the separate `meo-mai-moi` skill. Do not
substitute one authorization model for the other.

## Use the public documentation

- Connection and client examples:
  `https://github.com/troioi-vn/meo-mcp/blob/main/docs/clients.md`
- Complete tool, scope, schema, error, and risk matrix:
  `https://github.com/troioi-vn/meo-mcp/blob/main/docs/tools.md`
- OAuth and token lifecycle:
  `https://github.com/troioi-vn/meo-mcp/blob/main/docs/oauth.md`
- Stable failure contract:
  `https://github.com/troioi-vn/meo-mcp/blob/main/docs/errors.md`

Keep personal pet records, messages, invitation material, health data, and
financial values out of chat unless the user needs them to make a decision.
