---
name: karrio-schedule-pickup-and-close-the-day
description: Book a carrier pickup for purchased labels and produce the end-of-day manifest — including the one Karrio action that cannot be undone.
api: Karrio Shipping API
spec: openapi/karrio-api-openapi.yml
spec_version: 2026.1.32
operations:
  - "$$$$create"
  - "$$$$retrieve"
  - "$$$$update"
  - "$$$$cancel"
  - "$$$$&&create"
  - "$$$$&&retrieve"
  - "$$$$&&document"
  - "$$$$$list"
generated: '2026-08-27'
method: generated
source: Grounded in openapi/karrio-api-openapi.yml. Every operationId above is verbatim from the spec.
---

# Schedule a pickup and close out the day

This is the end of the fulfilment day: labels are bought, parcels are packed, and the
carrier needs to come and take them. Two separate things happen, and only one of them can
be undone.

## Step 1 — find what is shipping

`$$$$$list` — `GET /v1/shipments`

Filter with `status`, `carrier_name`, `created_after`, `created_before`. Pagination is
`limit`/`offset`, limit capped at 100, newest first.

Collect the shipment ids you intend to hand over, grouped **by carrier**. Both a pickup
and a manifest are per-carrier — you cannot mix carriers in one of either.

## Step 2 — schedule the pickup

`$$$$create` — `POST /v1/pickups`

Required: `pickup_date`, `ready_time`, `closing_time`. Also send `carrier_code`,
`address`, `parcels_count`, `tracking_numbers`, and `instruction` or `package_location`
if the driver needs to be told where to look.

`ready_time` and `closing_time` define the window the driver may arrive in. Confirm both
with the human — a window the warehouse cannot staff is worse than no pickup.

A **424** here is the carrier refusing the booking: no capacity, a date outside their
window, an unserviceable address. `messages[].carrier_name` names which carrier said no.
Changing the date or the carrier is a legitimate retry; repeating the identical request
is not.

## Step 3 — amend or cancel the pickup

- `$$$$update` — `POST /v1/pickups/{id}` — change the date, window or parcel count.
- `$$$$cancel` — `POST /v1/pickups/{id}/cancel` — cancel outright.

Both work until the carrier's own cutoff, which Karrio does not publish; the carrier
decides and answers with a 424 if it is too late. Prefer **update** over cancel-and-rebook
— rebooking can lose the slot.

`$$$$retrieve` — `GET /v1/pickups/{id}` — re-read before acting, so you branch on the
real state.

Do not use `POST /v1/pickups/{carrier_name}/schedule`. It is marked `deprecated: true`
in the contract.

## Step 4 — the manifest, and the warning that goes with it

`$$$$&&create` — `POST /v1/manifests`

Required: `carrier_name`, `address`, `shipment_ids`. This produces the end-of-day SCAN
form the driver signs for the whole batch.

**THIS CANNOT BE UNDONE.** Karrio publishes no cancel, void, delete or amend operation
for a manifest. Once created it can only be retrieved (`$$$$&&retrieve` —
`GET /v1/manifests/{id}`) and its document regenerated (`$$$$&&document` —
`POST /v1/manifests/{id}/document`).

It is the only write operation in this API with no reversal path at all. Every other
money-moving or physical action — label purchase, proxy label, pickup, order — has one.

So: **get explicit human confirmation of the exact `shipment_ids` before calling it.** If
you manifest the wrong set, the remedy is not an API call; it is a conversation with the
carrier.

## Step 5 — hand over the document

`$$$$&&document` — `POST /v1/manifests/{id}/document` returns the printable manifest.
Give the human the document and the pickup window together — those two facts are what the
warehouse actually needs.

## Ordering

Manifest **after** the pickup is confirmed, not before. A manifest for parcels no carrier
is coming to collect is a document that has to be reissued, and reissuing it means
creating a second one you also cannot delete.
