# Meo Mai Moi MCP Skill

**Pet management for nerds.** A portable agent skill for safely connecting to
the Meo Mai Moi OAuth MCP gateway.

It helps Codex, Cursor, OpenClaw, and other `SKILL.md`-compatible agents use
`https://mcp.meo-mai-moi.com/mcp` for pet profiles, care and health records,
rehoming, sharing, messaging, groups, finances, notifications, and account
workflows.

The skill does not contain credentials and never asks users to paste tokens.
It guides an agent through native OAuth, named scope presets, discovery,
read-first workflows, stable targets, idempotency, and verification. In
OpenClaw, an explicit “connect” request lets the agent configure its own native
MCP entry, complete OAuth, probe the server, and ask you to start a new session
(`/new` or `/reset`) before the first Meo call.

## Install

Install the `meo-mai-moi-mcp/` folder using your agent's normal skill mechanism,
then start a new session. For OpenClaw:

```bash
openclaw skills install @troioi-vn/meo-mai-moi-mcp
```

Then tell the agent: “Connect and authorize Meo Mai Moi.” The agent should add
the native MCP server with the Everyday care preset, give you the Meo consent
link, probe the connection, ask you to `/new`, and begin with `list_pets` in
that fresh session. For Codex and Cursor, install the same portable folder in
the client's skills location.

ClawHub listing:
[`@troioi-vn/meo-mai-moi-mcp`](https://clawhub.ai/troioi-vn/skills/meo-mai-moi-mcp)

The remote Streamable HTTP endpoint is:

```text
https://mcp.meo-mai-moi.com/mcp
```

Authenticate through Meo OAuth, discover tools, and start with a narrow read
such as `list_pets`.

## Scope presets

- **Everyday care** (default for “connect Meo Mai Moi”): pets, health, habits,
  and microchips read/write.
- **Full management**: all gateway scopes, only after explicit selection and a
  clear warning about sensitive domains.
- A named narrower task still requests only that task's scopes.

## Direct API versus MCP

This is the OAuth MCP skill. For a direct REST integration using a user-managed
personal API token, use the separate
[Meo Mai Moi REST API skill](https://github.com/troioi-vn/meo-mai-moi-skill).
The two authorization models are intentionally not combined.

## Documentation

- [Gateway README](https://github.com/troioi-vn/meo-mcp)
- [Client connection guide](https://github.com/troioi-vn/meo-mcp/blob/main/docs/clients.md)
- [Tool and scope catalog](https://github.com/troioi-vn/meo-mcp/blob/main/docs/tools.md)
- [OAuth design](https://github.com/troioi-vn/meo-mcp/blob/main/docs/oauth.md)

## License

MIT
