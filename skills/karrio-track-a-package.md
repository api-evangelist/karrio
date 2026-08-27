---
name: karrio-track-a-package
description: Register a tracking number with Karrio and read its status and event history, including packages shipped outside Karrio.
api: Karrio Shipping API
spec: openapi/karrio-api-openapi.yml
spec_version: 2026.1.32
operations:
  - "$$$$$$add"
  - "$$$$$$retrieve"
  - "$$$$$$list"
  - "$$$$$$remove"
  - "&&list"
generated: '2026-08-27'
method: generated
source: Grounded in openapi/karrio-api-openapi.yml. Every operationId above is verbatim from the spec.
---

# Track a package

Tracking in Karrio is stateful. You do not "look up" a tracking number — you create a
**tracker**, a persistent object Karrio polls the carrier for and raises webhook events
against. That is why the first call is a POST.

## Step 1 — create the tracker

`$$$$$$add` — `POST /v1/trackers`

Required: `tracking_number` and `carrier_name`. Optional: `account_number`, `reference`,
`metadata`.

If you do not know the carrier, list the catalog first with `&&list` —
`GET /v1/carriers` — and match. Karrio does not guess the carrier from the number shape
on this endpoint.

This works for parcels shipped outside Karrio: you do not need a Karrio shipment to track
a number.

If the tracker already exists for that number, expect a **409**. Fall through to step 2
rather than treating it as a failure.

## Step 2 — read it

`$$$$$$retrieve` — `GET /v1/trackers/{identifier}`

`identifier` accepts either the tracker id or the tracking number, which is what makes
this usable without storing Karrio ids.

Returns `status`, an `info` block, and `events[]` — a chronological history with
timestamp, description and location. Present the current status and the most recent
events; do not dump the whole history unless asked.

`messages[]` may be present **on a successful response**. Those are carrier warnings
surfaced inline — for example, a carrier that has the number but no scan events yet.
Read them; they usually explain an empty `events[]`.

## Step 3 — keep it fresh

Do not poll in a loop. Subscribe instead:

`POST /v1/webhooks` with `enabled_events` containing `tracker_updated`,
`shipment_out_for_delivery`, `shipment_delivery_failed`.

Use the **snake_case** names above. Karrio's webhooks documentation page shows dotted
names like `tracking.status_updated`; those are not in the contract's enum and will be
rejected.

## Step 4 — clean up

`$$$$$$remove` — `DELETE /v1/trackers/{identifier}`

Stops polling and discards the tracker. Use `$$$$$$list` — `GET /v1/trackers` — with
`limit`/`offset` (max 100) to find stale trackers.

## Deprecated paths — do not use

The contract still carries these but marks them `deprecated: true`:

- `GET /v1/trackers/{carrier_name}/{tracking_number}` — use `POST /v1/trackers`
- `GET /v1/proxy/tracking/{carrier_name}/{tracking_number}` — use `POST /v1/proxy/tracking`

Karrio publishes no Sunset or Deprecation header and no removal date, so there is no way
to know how long they will keep working. Move off them.
