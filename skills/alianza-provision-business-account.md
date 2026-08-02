---
name: Provision a business cloud communications account
description: Create an Alianza One account inside a partition, add end users, attach a calling plan, and send the welcome email — the core service-provider onboarding flow.
api: openapi/alianza-openapi-original.yml
operations: [logUserIn, validateAccountProperties, createAccount, getAvailableUserProductPlans, createEndUser, getPlans, addPlan, sendWelcomeEmail, getStatus]
---

# Provision a business cloud communications account

Use this when a service provider needs a new business or residential voice account stood up on
the Alianza One platform. Everything happens inside a **partition** — the service provider's
tenant — so you always need a `partitionId` before you start.

## Before you begin

- Base URL: `https://api.alianza.com` (production) or `https://api.b2.alianza.com` (Beta).
  **Build and certify against Beta first** — Alianza requires certification before production
  access.
- You need admin-portal credentials from an Alianza account manager. There is no self-service
  sign-up.
- All request and response bodies are `application/json`.

## Steps

1. **Authenticate.** `logUserIn` — `POST /v2/authorize` with the username and password. Read
   `authToken` from the response and send it as the `X-AUTH-TOKEN` header on every subsequent
   call. Re-run this step on any `401` (`Expired Auth Token`).

2. **Pre-validate the account.** `validateAccountProperties` —
   `GET /v2/partition/{partitionId}/account/validateproperties`. This catches property conflicts
   before you write anything. **Do this first**: there is no idempotency key on this API, so a
   failed-then-retried create can produce a duplicate account.

3. **Create the account.** `createAccount` — `POST /v2/partition/{partitionId}/account`. Keep the
   returned account id. If the call times out or errors ambiguously, do **not** blindly retry —
   call `accountSearch` (`GET /v2/partition/{partitionId}/account/search`) to check whether the
   account already landed.

4. **Check what the account can buy.** `getAvailableUserProductPlans` —
   `GET /v2/partition/{partitionId}/account/{accountId}/user-product-plans` returns the user
   product plans available to this account.

5. **Create end users.** `createEndUser` —
   `POST /v2/partition/{partitionId}/account/{accountId}/user` for each seat. Validate the email
   first with `prevalidateEmailAddress`
   (`GET /v2/partition/{partitionId}/account/{accountId}/user/prevalidate/email/{email}`) so you
   do not burn a create on a bad address.

6. **Attach calling plans.** `getPlans` —
   `GET /v2/partition/{partitionId}/account/{accountId}/callingplan` to read the current plans,
   then `addPlan` (`PUT` on the same path) to add one for a given reference.

7. **Send welcome email.** `sendWelcomeEmail` —
   `POST /v2/partition/{partitionId}/account/{accountId}/user/{userId}/send-welcome-email`, or
   `sendWelcomeEmailToAll` to cover everyone who has not been emailed yet. This is an
   externally-visible side effect — confirm with a human before firing it in production.

8. **Confirm.** `getStatus` — `GET /v2/partition/{partitionId}/account/{accountId}/status`.

## Rules

- **No idempotency.** Alianza publishes no `Idempotency-Key`. Every `POST` in this flow is unsafe
  to retry. Re-query before re-creating.
- **Errors** come back as `{"status": <int>, "messages": ["..."], "data": {}}`
  (`PublicApiException`), not RFC 9457 problem+json. Read `messages[]` for the human cause.
  `400` means invalid request parameters, `404` means the partition, account or user was not
  found, `403` means insufficient permission for this management user.
- **Pagination is inconsistent.** Most list operations return everything. Where paging exists it
  is either `firstResultIndex` / `maxResult` or `pageNum` / `pageSize` — check the operation.
- **Deleting is irreversible.** `deleteAccount` and `deleteEndUser` have no undo through the API.
