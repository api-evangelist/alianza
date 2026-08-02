---
name: Provision and swap a device line
description: Create a device line on an account, pull its provisioning data, verify SIP registration, and swap the physical hardware behind it.
api: openapi/alianza-openapi-original.yml
operations: [listDevices, createDevice, getDevice, provisioningInfo, provisioningStatus, registrationStatus, swapPrevalidate, swapDevice, getProvisioningByMacAddress, validateDeviceProperties, listDeviceLinesForMac, generateAdminPassword, removeDevice]
---

# Provision and swap a device line

A **device line** binds a physical or soft phone to a line on an Alianza account. Devices are
identified by MAC address; device lines have their own ids.

## Provision

1. **Validate the hardware is usable.** `validateDeviceProperties` —
   `GET /v2/partition/{partitionId}/account/{accountId}/device/{macAddress}/validateproperties`
   confirms the device is available before you claim it.

2. **Create the device line.** `createDevice` —
   `POST /v2/partition/{partitionId}/account/{accountId}/deviceline`. Keep the `deviceId`.

3. **Fetch provisioning data.** `provisioningInfo` —
   `GET .../deviceline/{deviceId}/provisioning`, or by hardware with
   `getProvisioningByMacAddress` (`GET /v2/partition/{partitionId}/deviceprovisioning/{macAddress}`).

4. **Watch it come up.** `provisioningStatus` (`GET .../deviceline/{deviceId}/provisioningstatus`)
   then `registrationStatus` (`GET .../deviceline/{deviceId}/registrationstatus`). Registration is
   the signal that the phone is actually talking to the platform.

5. **Enumerate.** `listDevices` (`GET .../deviceline`) for the account, or `listDeviceLinesForMac`
   (`GET .../device/{macAddress}`) for everything on one piece of hardware.

## Swap hardware (RMA / upgrade)

1. **Pre-validate the replacement MAC.** `swapPrevalidate` —
   `GET .../deviceline/{deviceId}/swap/prevalidate`. Never skip this; the swap itself is not
   idempotent.
2. **Swap.** `swapDevice` — `POST .../deviceline/{deviceId}/swap`.
3. **Re-verify.** `registrationStatus` again on the same `deviceId`.

## Device administration

`generateAdminPassword`
(`POST .../device/{macAddress}/metadata/generateadminpassword`) and `resetEncryptionKey`
(`POST .../device/{macAddress}/metadata/resetencryptionkey`) rotate device secrets. Both are
security-sensitive and will disrupt a live device — require human confirmation and log the
result.

## Rules

- The `.../user/{userId}/device/...` operation family is **deprecated**; use the
  `.../deviceline/...` family in this skill.
- No idempotency key: re-read with `listDevices` before retrying a failed `createDevice`.
- `removeDevice` (`DELETE .../deviceline/{deviceId}`) takes the line out of service immediately.
