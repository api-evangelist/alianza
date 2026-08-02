---
name: Activate or port a telephone number
description: Search inventory, reserve a number, add it to an account, or initiate and trigger a port-in with the losing carrier — including E911 address handling.
api: openapi/alianza-openapi-original.yml
operations: [tnSearch, listTnInventory, retrieveTelephoneNumberAvailability, reserve, createOrAddTelephoneNumberToAccount, retrievePort, updatePort, triggerPort, cancelPort, getE911Correction, acceptE911Correction, acceptOriginalE911, getServiceActivationEvents]
---

# Activate or port a telephone number

Two paths lead to a working number on an account: take one out of the partition's own inventory,
or port one in from another carrier. Both end at the same place — a telephone number attached to
an account with a valid E911 address.

## Path A — activate from inventory

1. **Find a number.** `tnSearch` — `GET /v2/partition/{partitionId}/telephonenumber/search`, or
   `listTnInventory` (`GET /v2/partition/{partitionId}/telephonenumber`) to browse.
2. **Check it is free.** `retrieveTelephoneNumberAvailability` —
   `GET /v2/partition/{partitionId}/telephonenumber/{tn}/availability`.
3. **Reserve it.** `reserve` — `PUT /v2/partition/{partitionId}/telephonenumber/{tn}/reserve`.
   Release with `releaseReservation` (`DELETE` on the same path) if the order falls through.
4. **Attach it to the account.** `createOrAddTelephoneNumberToAccount` —
   `POST /v2/partition/{partitionId}/account/{accountId}/telephonenumber`.

## Path B — port in from another carrier

1. **Start the port.** The same operation, `createOrAddTelephoneNumberToAccount`, initiates a
   port when the request describes a number the partition does not own.
2. **Read the port request.** `retrievePort` —
   `GET /v2/partition/{partitionId}/account/{accountId}/port/{portId}`.
3. **Correct it if needed.** `updatePort` (`PUT`) or `patchPort` (`PATCH`) on the same path.
4. **Fire it at the carrier.** `triggerPort` —
   `PUT /v2/partition/{partitionId}/account/{accountId}/port/{portId}/triggerport`. **This has a
   real, externally visible carrier consequence. Require explicit human approval.**
5. **Back out if necessary.** `cancelPort` —
   `DELETE /v2/partition/{partitionId}/account/{accountId}/port/{portId}/cancelport`.

## Path C — dynamic inventory (number not known up front)

`createOrAddTelephoneNumberToAccountWithDynamicInventory`
(`POST /v2/partition/{partitionId}/account/{accountId}/telephonenumber/tn-requests`) reserves a
number the carrier will pick. Poll `getDynamicInventoryActivation`, and only fall back to
`refreshDynamicInventoryActivation` if the carrier webhook that normally pushes status to Alianza
has failed — the spec explicitly warns not to depend on that path.

## E911 — do not skip this

After activation Alianza may adjust the submitted address:

- `getE911Correction` — `GET .../telephonenumber/{tn}/911-address/correction` returns the
  adjusted address.
- `acceptE911Correction` — accept Alianza's corrected address, **or**
- `acceptOriginalE911` — insist on the address as submitted.

One of these must be resolved. `retrieveTelephoneNumberUsages`
(`GET .../telephonenumber/{tn}/911-address/usages`) tells you which users depend on the number
before you change or remove it.

## Rules

- **Track activation state** with `getServiceActivationEvents` —
  `GET /v2/partition/{partitionId}/account/{accountId}/reference/{referenceId}/type/{serviceType}`.
- **No idempotency key.** A retried activation or port initiation can create a duplicate order.
  Re-read with `retrieveAccountTelephoneNumbers` or `retrieveDynamicInventoryActivations` first.
- Errors arrive as `PublicApiException` (`{status, messages[], data}`) on `application/json`.
