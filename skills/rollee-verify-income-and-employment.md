---
name: Verify a worker's income and employment with Rollee
description: >-
  Create a user, run the Rollee Connect flow so the worker links a gig/payroll/tax
  account, then retrieve normalized income and employment data for underwriting.
api: Rollee User API (https://api.getrollee.com/api/v0.5)
operations:
  - POST /users
  - POST /users/{id}/sessions/new
  - GET /users/{id}/accounts
  - GET /accounts/{id}/income
  - GET /accounts/{id}/employment
source: https://developers.getrollee.com/recipes/connect-an-account-and-get-data-from-it
---

# Verify a worker's income and employment

Use this to pull real-time, normalized income + employment data for a salaried or
self-employed worker across Rollee's 70+ gig-economy, payroll, tax, and wallet
datasources.

## Auth
All requests use a Rollee-issued JWT as `Authorization: Bearer <TOKEN>`. Refresh a
near-expiry token with `GET /token/refresh`. Sandbox and production use different
tokens and different base URLs (`https://api.sand.getrollee.com/api/v0.5` vs
`https://api.getrollee.com/api/v0.5`).

## Steps
1. **Create the user** — `POST /users` with optional `metadata`. Store the returned
   user `id` (a UUID).
2. **Start a Connect session** — `POST /users/{id}/sessions/new` and hand the session
   to the Rollee Connect SDK (React / React Native / Web JS / Flutter). The worker
   authenticates with their chosen platform inside Connect. In sandbox, Connect
   auto-creates a fake account with random credentials.
3. **List connected accounts** — `GET /users/{id}/accounts` and pick the connected
   account `id`(s).
4. **Read income** — `GET /accounts/{id}/income`.
5. **Read employment** — `GET /accounts/{id}/employment`.

## Conventions
- IDs are UUIDs; datetimes are RFC 3339 strings in UTC.
- A `null` field means the datasource does not support it or returned no data.
- Prefer webhooks (`update_endpoint` / `update_multi_endpoint`) over polling — data
  is fetched asynchronously after the account connects; wait for the event before
  reading.

## Errors
- `401 Unauthorized` → refresh the JWT.
- Connection failures surface as Rollee Connect events with a
  `connection_status` / `status_description`, not as HTTP problem documents.
