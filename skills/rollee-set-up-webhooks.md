---
name: Set up Rollee webhooks for async data delivery
description: >-
  Configure Rollee webhooks so your service is notified when accounts connect and
  when fresh income/employment data has been fetched, instead of polling.
api: Rollee User API (https://api.getrollee.com/api/v0.5)
operations:
  - PUT /webhooks/accounts
  - PUT /webhooks/endpoints
  - PUT /webhooks/multi-endpoints
  - GET /webhooks
  - DELETE /webhooks/{type}
source: https://developers.getrollee.com/recipes/set-up-webhooks
---

# Set up Rollee webhooks

Rollee fetches datasource data asynchronously after a worker connects an account.
Subscribe to webhooks so you react to `update_endpoint` events rather than polling.

## Auth
JWT bearer token, same as all API calls.

## Steps
1. **Register an account-events webhook** — `PUT /webhooks/accounts` with `url`,
   `method` (`POST`|`PUT`), `enabled: true`, and an optional `secret`. This is a full
   replace — send every parameter each time.
2. **Register an endpoint-events webhook** — `PUT /webhooks/endpoints` (or
   `PUT /webhooks/multi-endpoints` to batch several data endpoints) to be notified
   when new data lands for an account.
3. **Verify** — `GET /webhooks` lists all configured webhooks (enabled and disabled).
4. **Remove** — `DELETE /webhooks/{type}` where type is `accounts`, `users`,
   `endpoints`, `multi_endpoints`, or `rollee_connect_events`.

## Verifying signatures
If you set a `secret`, Rollee signs each delivery with **HMAC SHA-512** and sends the
signature in the **`X-Signature`** header. Recompute and compare before trusting a
payload.

## Delivery semantics
Respond `2xx` within 10 seconds. Slow or non-2xx responses are retried up to 3 times
at ≥30-second intervals, then discarded. On an `update_endpoint` event, call the
matching `GET /accounts/{id}/{endpoint}` to read the freshly fetched data.
