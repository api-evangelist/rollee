---
name: Connect a company account and read employer data with Rollee
description: >-
  Create a company, connect a company (employer/fleet) account via Rollee Connect,
  then read employees, absences, employment, and income for that company account.
api: Rollee Company API (https://api.getrollee.com/company/v0.5)
operations:
  - POST /companies
  - GET /companies/{id}/company-accounts
  - GET /company-accounts/{id}/employees
  - GET /company-accounts/{id}/absences
  - GET /company-accounts/{id}/income
source: https://developers.getrollee.com/recipes/connect-a-company-account-and-get-data-from-it
---

# Connect a company account and read employer data

Use the Company API when the subject is an employer/fleet rather than an individual
worker — e.g. HR verification, fleet management, or B2B payroll checks.

## Auth
Same JWT bearer scheme as the User API (`Authorization: Bearer <TOKEN>`), against the
company base path `https://api.getrollee.com/company/v0.5`.

## Steps
1. **Create the company** — `POST /companies` for the client.
2. **Connect via Rollee Connect** — run the Connect flow to link the company's
   payroll/HR datasource, producing a company account.
3. **List company accounts** — `GET /companies/{id}/company-accounts`; capture the
   company-account `id`.
4. **Read employees** — `GET /company-accounts/{id}/employees`.
5. **Read absences** — `GET /company-accounts/{id}/absences`.
6. **Read income / employment** — `GET /company-accounts/{id}/income` and
   `GET /company-accounts/{id}/employment`.

## Notes
- Company-account data endpoints mirror the User API (activity, banking-info,
  documents, performance, profiles, trips, vehicles, wallet).
- Company webhooks are configured at `PUT /company/v0.5/webhooks/accounts` and
  `PUT /company/v0.5/webhooks/endpoints`; contact Rollee before enabling them.
