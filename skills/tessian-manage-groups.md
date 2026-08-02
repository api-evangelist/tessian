---
name: Manage Tessian address/domain groups
description: Create and maintain groups of addresses and domains used by Tessian policies.
api: openapi/tessian-openapi-original.json
operations:
- insights.external_api.main.endpoint_groups_read
- insights.external_api.main.endpoint_group_create
- insights.external_api.main.endpoint_group_read
- insights.external_api.main.endpoint_group_update
- insights.external_api.main.endpoint_group_delete
- insights.external_api.main.endpoint_group_update_add
- insights.external_api.main.endpoint_group_update_remove
---

# Manage Tessian address/domain groups

Use this skill to manage the address/domain groups Tessian uses in policies.

## Auth
Send `Authorization: API-Token <your-api-token>` on every request.

## Steps
1. List existing groups with `endpoint_groups_read` (`GET /api/v1/groups`).
2. Create a group with `endpoint_group_create` (`POST /api/v1/groups`), supplying the group metadata (address and domain members).
3. Read one group with `endpoint_group_read` (`GET /api/v1/groups/{id}`).
4. Update group metadata with `endpoint_group_update` (`PUT /api/v1/groups/{id}`).
5. Add members with `endpoint_group_update_add` (`POST /api/v1/groups/{id}/add_members`) and remove them with `endpoint_group_update_remove` (`POST /api/v1/groups/{id}/remove_members`).
6. Delete a group with `endpoint_group_delete` (`DELETE /api/v1/groups/{id}`).

## Notes
- These write operations are not idempotency-keyed; re-issuing a create will create another group. Read back with `endpoint_groups_read` to reconcile before retrying after a network error.
- On 429, pause a few seconds and retry.
