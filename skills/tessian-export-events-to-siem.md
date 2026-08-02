---
name: Export Tessian security events to a SIEM
description: Poll the Tessian API on a schedule and stream deduplicated security events into a SIEM or datastore.
api: openapi/tessian-openapi-original.json
operations:
- insights.external_api.main.get_events
- insights.external_api.main.get_audits
---

# Export Tessian security events to a SIEM

Use this skill to pull Tessian security events for downstream analysis.

## Auth
Send every request with `Authorization: API-Token <your-api-token>`. Generate the token in the Tessian portal under Integrations > Tessian API (the creating user needs the "Logs" permission). The token is shown only once.

## Steps
1. Call `get_events` (`GET /api/v1/events`). On the first call, optionally pass `created_after` (ISO 8601 UTC) to bound the window and `limit` to cap page size.
2. Read `checkpoint` and `has_more` from the response. If `has_more` is true, call `get_events` again with `after_checkpoint=<checkpoint>`. Repeat until `has_more` is false. Never decide to stop by counting rows — always trust `has_more`.
3. Deduplicate: the same event `id` can be returned multiple times as fields fill in. Keep only the row with the latest `updated_at` per `id`.
4. Optionally call `get_audits` (`GET /api/v1/audits`) with the same checkpoint pattern to capture portal audit-log events.
5. Persist the deduplicated events to your SIEM/datastore. Run on a recurring schedule (e.g. hourly), carrying the last `checkpoint` forward between runs.

## Error handling
- 401/403: token missing/invalid/revoked or API not enabled — regenerate a token and confirm API access.
- 429: back off a few seconds and retry (honor RateLimit headers).
- 5xx: retry with exponential backoff.
