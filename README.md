# Meo Mai Moi MCP Skill

**Pet management for nerds.** A portable agent skill for safely connecting to
the Meo Mai Moi OAuth MCP gateway.

It helps Codex, Cursor, OpenClaw, and other `SKILL.md`-compatible agents use
`https://mcp.meo-mai-moi.com/mcp` for pet profiles, care and health records,
rehoming, sharing, messaging, groups, finances, notifications, and account
workflows.

The skill does not contain credentials and never asks users to paste tokens.
It guides an agent through native OAuth, narrow scope selection, discovery,
read-first workflows, stable targets, idempotency, and verification.

## Install

Install the `meo-mai-moi-mcp/` folder using your agent's normal skill mechanism,
then start a new session. For OpenClaw, use the ClawHub listing after publication.
For Codex and Cursor, install the same portable folder in the client's skills
location.

ClawHub listing:
[`@troioi-vn/meo-mai-moi-mcp`](https://clawhub.ai/troioi-vn/skills/meo-mai-moi-mcp)

The MCP server itself is configured separately as a remote Streamable HTTP
server at:

```text
https://mcp.meo-mai-moi.com/mcp
```

Authenticate through Meo OAuth, discover tools, and start with a narrow read
such as `list_pets`.

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
