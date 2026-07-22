# Safety and error handling

## Consequential operations

Before writing, identify the exact resource and current state. Do not infer an
ID from a name when a discovery tool can resolve it.

For high-impact actions—deleting a pet or record, changing access, sending a
message, finalizing placement, changing finance data, or creating/revoking an
invitation—read the target immediately before the action. Supply every expected
field and version required by the schema. Let Meo reject stale or mismatched
state instead of guessing.

Read/preview before an action that can affect another person, long-lived access,
health history, finances, or a real-world handover. After a write, verify the
reported state through a read tool before summarizing it to the user.

## Structured failures

Use the stable error `code`, `retryable`, and any `upstream_status` in a tool's
structured error result. Do not branch on prose alone.

- `authorization_inactive` or upstream `401`: reconnect through OAuth.
- `target_mismatch` or `upstream_conflict`: re-read; the target changed or the
  agent's assumptions are stale.
- `duplicate_candidate`: inspect the existing candidate and ask whether the
  user intended a separate record.
- retryable upstream or rate-limit errors: back off and retry later; do not
  repeat a write with a new idempotency key.

Never paste access tokens, refresh tokens, callback parameters, personal API
tokens, invitation URLs/codes, authorization headers, or full personal records
into chat, prompts, issues, or logs.
