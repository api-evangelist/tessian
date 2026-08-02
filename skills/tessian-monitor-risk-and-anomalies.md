---
name: Monitor Tessian company risk and anomalies
description: Retrieve company risk scores, per-user monitoring status, and detected anomalies for reporting dashboards.
api: openapi/tessian-openapi-original.json
operations:
- insights.external_api.main.get_company_risk
- insights.external_api.main.get_users
- insights.external_api.main.get_anomalies
- insights.external_api.main.get_triggers
---

# Monitor Tessian company risk and anomalies

Use this skill to feed Tessian risk posture into internal dashboards.

## Auth
Send `Authorization: API-Token <your-api-token>` on every request.

## Steps
1. Call `get_company_risk` (`GET /api/v1/risk/company`) for company-level risk scores over time.
2. Call `get_users` (`GET /api/v1/monitoring/users`) for per-user onboarding/monitoring status (SyncState: Live, Onboarding, Queued, Out of date).
3. Call `get_anomalies` (`GET /reporting/anomalies/v1`) to retrieve detected anomalies; each anomaly carries `links.portal_url` for drill-down.
4. Call `get_triggers` (`GET /reporting/triggers/v1`) to retrieve DLP/security triggers by module (Defender/Guardian/Enforcer/Architect/Constructor).
5. Use checkpoint pagination (`after_checkpoint` / `checkpoint` / `has_more`) where the endpoint returns a checkpoint; deduplicate on `id` keeping the latest `updated_at`.

## Error handling
- 429: back off and retry.
- 5xx: retry with backoff.
