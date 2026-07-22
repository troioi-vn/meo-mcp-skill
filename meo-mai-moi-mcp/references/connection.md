# Connection and scope selection

## Endpoint and OAuth

Configure the remote Streamable HTTP endpoint:

```text
https://mcp.meo-mai-moi.com/mcp
```

Use the client's native OAuth action. A normal flow is: configure the server,
authenticate, sign in to Meo Mai Moi, approve scopes, discover tools, then make
the smallest useful call. The public health endpoint is
`https://mcp.meo-mai-moi.com/health`.

Codex, Cursor, OpenClaw, and other OAuth-capable MCP clients can use the same
endpoint. Client configuration labels differ; do not invent a local token or
work around native OAuth support.

When the user explicitly asks the agent to connect or authorize Meo, prefer the
client's own MCP configuration surface and initiate OAuth on the user's behalf.
Fall back to user-run commands only when the client cannot modify its MCP
configuration or execution policy blocks the required command.

## Default pet-management grant

If the user asks only to “manage my pets” without naming a narrower task,
request this OAuth scope string:

```text
pets:read pets:write health:read health:write microchips:read microchips:write finance:read finance:write
```

This grant includes sensitive health and finance access and must remain visible
on the consent screen. It does not authorize habits, sharing, placement,
helpers, messages, groups, notifications, profile, or invitation workflows.
When the user names a narrower task, request only that task's read/write pair.
When a later tool reports `insufficient_scope`, merge only the missing domain
pair and reauthorize.

## Request scopes by task

| Task family | Start with |
|---|---|
| Pet profile, types, categories, photos, lifecycle | `pets:read` or `pets:read,pets:write` |
| Weights, vaccinations, medical records | `health:read` or `health:read,health:write` |
| Care routines | `habits:read` or `habits:read,habits:write` |
| Microchip identity | `microchips:read` or `microchips:read,microchips:write` |
| Sharing a pet | `sharing:read` or `sharing:read,sharing:write` |
| Rehoming, fostering, sitting, helpers | the relevant `placement:*` and/or `helpers:*` pair |
| Chats and messages | `messages:read` or `messages:read,messages:write` |
| Groups | `groups:read` or `groups:read,groups:write` |
| Ledgers, expenses, receipts | `finance:read` or `finance:read,finance:write` |
| Notifications | `notifications:read` or `notifications:read,notifications:write` |
| Own profile, locale, owner-weight history | `profile:read` or `profile:read,profile:write` |
| Account invitations | `invitations:read` or `invitations:read,invitations:write` |

Write tools require their matching read scope because a safe workflow begins by
reading the actual target. A tool may require multiple domain scopes; discover
its schema and description before requesting extra consent.

## First-call pattern

For a request such as “show my pets,” connect with `pets:read`, discover tools,
then call `list_pets` with `{}`. Summarize only the information needed for the
user's request.

For a requested change, first call the matching list/detail tool. Use the
returned stable ID and version in the write input, choose one new idempotency
key, then read the target again to verify the intended state.
