# OpenClaw native MCP onboarding

Use OpenClaw's native MCP registry. Do not use the older `mcporter` registry for
this connection.

## Inspect before changing

Confirm that native MCP commands exist, then inspect the saved entry:

```bash
openclaw mcp --help
openclaw mcp show meo-mai-moi --json
```

If the command is unavailable, tell the user that this OpenClaw version lacks
native MCP management and give one compact upgrade or manual-configuration
fallback. If an existing `meo-mai-moi` entry points anywhere except
`https://mcp.meo-mai-moi.com/mcp`, stop and ask before replacing it.

If the correct entry is already authorized, probe it and reuse it. Do not force
reauthorization merely because the user asked whether Meo is connected.

## Configure and start OAuth

For a missing entry and an otherwise-unspecified “manage my pets” request, run:

```bash
openclaw mcp add meo-mai-moi \
  --url https://mcp.meo-mai-moi.com/mcp \
  --transport streamable-http \
  --auth oauth \
  --oauth-scope 'pets:read pets:write health:read health:write microchips:read microchips:write finance:read finance:write' \
  --no-probe
openclaw mcp login meo-mai-moi
```

Use `--no-probe` only for initial registration because the server cannot be
probed before OAuth. For a narrower named task, substitute the matching scope
pair. To change the requested scope on the correct existing entry, use
`openclaw mcp configure meo-mai-moi --auth oauth --oauth-scope '<scopes>'`.

Return the generated authorization link only in the same private one-to-one
conversation. Tell the user to approve it and send back only the `code` value
from the final browser redirect. A failed page load at `localhost:8989` is
expected in a remote or phone browser; the code remains visible in its address
bar.

Treat the code as a short-lived credential. Do not echo it. Exchange it once:

```bash
openclaw mcp login meo-mai-moi --code '<code>'
```

If it is rejected or expired, discard it and start a new login. Never request
or accept the URL or code in a group/public channel. In that case, ask the user
to finish the same command locally on the OpenClaw host.

## Verify and activate

After the code exchange succeeds, run:

```bash
openclaw mcp status --verbose
openclaw mcp doctor meo-mai-moi --probe
openclaw mcp reload
```

Do not add an OpenClaw tool include/exclude filter: expose the complete Meo tool
catalog and let OAuth plus each tool's scope enforcement control access. If the
current turn cannot see the newly projected tools, say that the connection is
ready and ask the user for one fresh message. On the next tool-enabled turn,
discover tools and call `list_pets` with `{}`.

If a later call returns `insufficient_scope`, identify the exact missing domain,
merge only its read/write pair into the configured scope string, run OAuth
again, and retry after authorization. Never respond by granting every Meo
scope.
