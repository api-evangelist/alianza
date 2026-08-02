---
name: Configure a SIP trunk
description: Create a registration-based SIP trunk on an account, set credentials and forwarding behaviour, and verify registration status.
api: openapi/alianza-openapi-original.yml
operations: [validateSipTrunkIpPort, createSipTrunk, retrieveSipTrunk, updateSipTrunk, retrieveSipTrunkList, getShareableCallingPlans, retrieveSipTrunkForwardBehavior, updateSipTrunkForwardBehavior, getRegistrationStatus, unlockSipTrunk, deleteSipTrunk]
---

# Configure a SIP trunk

Alianza SIP trunks are registration-based and live under an account. Note the resource path is
`siptrunk_2` — the original `siptrunk` operations are deprecated.

## Steps

1. **Pre-validate the IP/port pair.** `validateSipTrunkIpPort` —
   `GET /v2/partition/{partitionId}/account/{accountId}/siptrunk_2/validate`. Do this before
   creating; there is no idempotency key to protect a retry.

2. **Check shareable calling plans.** `getShareableCallingPlans` —
   `GET /v2/partition/{partitionId}/account/{accountId}/siptrunk_2/shared-plan-lookup` returns
   the plans a trunk can share.

3. **Create the trunk.** `createSipTrunk` —
   `POST /v2/partition/{partitionId}/account/{accountId}/siptrunk_2`. Keep the `sipTrunkId`.

4. **Set forwarding behaviour.** `retrieveSipTrunkForwardBehavior` (`GET .../{sipTrunkId}/forward`)
   then `updateSipTrunkForwardBehavior` (`PUT` on the same path).

5. **Verify registration.** `getRegistrationStatus` —
   `GET /v2/partition/{partitionId}/account/{accountId}/siptrunk_2/{sipTrunkId}/registrationstatus`.
   If registration failed repeatedly the trunk may be locked; clear it with `unlockSipTrunk`
   (`PUT .../{sipTrunkId}/unlock`).

6. **Amend or remove.** `updateSipTrunk` (`PUT`), `patchSipTrunk` (`PATCH`), `deleteSipTrunk`
   (`DELETE`). Use `retrieveSipTrunkList` (`GET .../siptrunk_2`) to enumerate.

## Rules

- Use the `_2` resource path. Operations on the bare `siptrunk` path are marked `deprecated: true`
  in the published description and receive no further updates.
- Deleting a trunk drops live service for everything behind it. Confirm with a human.
- Errors are `PublicApiException` — read `messages[]`. A `409` means an IP/port or identity
  conflict with an existing trunk.
