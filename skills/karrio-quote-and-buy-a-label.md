---
name: karrio-quote-and-buy-a-label
description: Compare carrier rates for a parcel and buy the winning shipping label through Karrio, safely — including what to do when the purchase call does not come back.
api: Karrio Shipping API
spec: openapi/karrio-api-openapi.yml
spec_version: 2026.1.32
operations:
  - "@@fetch_rates"
  - "$$$$$create"
  - "$$$$$rates"
  - "$$$$$purchase"
  - "$$$$$retrieve"
  - "$$$$$cancel"
  - "&&&list"
generated: '2026-08-27'
method: generated
source: Grounded in openapi/karrio-api-openapi.yml. Every operationId above is verbatim from the spec.
---

# Quote and buy a shipping label

Karrio's operationIds are sigil-prefixed by its generator (`$$$$$purchase`, `@@fetch_rates`).
They look wrong. They are not — they are exactly what the published contract declares.
Match on the method and path if your tooling cannot handle them.

## Before you start

- **The base URL is the caller's own Karrio instance.** There is no shared public host.
  Ask for it. Do not use `api.karrio.io` — it appears in Karrio's own documentation but
  does not resolve (DNS SERVFAIL, checked 2026-08-27).
- Authenticate with `Authorization: Token key_xxxxxxxx`, or Basic auth with the token as
  the username and no password.
- The credential decides test vs live mode. A test-mode key cannot see live objects and
  will 404 on them.

## Step 1 — confirm there is a carrier to ship with

`&&&list` — `GET /v1/connections`

Returns the carrier accounts configured on this instance. If it is empty, stop: Karrio
does not resell postage, so nothing can be rated or purchased until the operator connects
their own carrier account. Report that rather than guessing a `carrier_id`.

## Step 2 — quote without committing

`@@fetch_rates` — `POST /v1/proxy/rates`

Required: `shipper`, `recipient`, `parcels`. Optional: `carrier_ids` to restrict the
quote, `services` to restrict service levels.

This is the safe rehearsal step. It creates nothing and charges nothing. Use it whenever
the user is still choosing. Present the returned rates with carrier, service, total
charge and transit estimate, and let the human pick.

If the response is **HTTP 424**, a carrier rejected the quote. Read
`messages[].carrier_name` and `messages[].code` — that is the carrier's vocabulary, not
Karrio's. Retry only with a different carrier, service, or the field the carrier named.
Do not blind-retry.

## Step 3 — create the shipment

`$$$$$create` — `POST /v1/shipments`

Same required fields as the rate request (`shipper`, `recipient`, `parcels`). Rates are
computed and attached to the shipment on creation, so you usually do not need step 2
again — read `rates[]` off the created shipment.

Creating a shipment does **not** buy anything. Nothing has been charged yet.

If you need to re-rate an existing shipment (address corrected, parcel changed), use
`$$$$$rates` — `POST /v1/shipments/{id}/rates`.

## Step 4 — get explicit confirmation, then purchase

`$$$$$purchase` — `POST /v1/shipments/{id}/purchase`

Body: `selected_rate_id` (the id of the chosen rate from `rates[]`), optionally
`label_type` (PDF, ZPL, PNG), `payment`, `reference`, `metadata`.

**This spends real money.** Confirm the carrier, the service and the total charge with the
human before calling it. Karrio's own MCP server marks the equivalent tool destructive for
this reason.

## Step 5 — the retry rule that matters

**Karrio publishes no idempotency key.** There is no `Idempotency-Key` header on any of
the 95 operations in the contract.

So if `$$$$$purchase` times out or the connection drops, you do not know whether a label
was bought. **Do not retry it.** Instead:

1. `$$$$$retrieve` — `GET /v1/shipments/{id}`
2. Read `status`. If it is `purchased`, the label exists — read `label_url` and
   `tracking_number` and stop. If it is still `draft`, nothing was bought and it is safe
   to purchase once more.

Retrying the purchase blindly can buy a second label on the same shipment.

## Step 6 — undoing it

`$$$$$cancel` — `POST /v1/shipments/{id}/cancel`

Voids the label. This works **only until the carrier collects the parcel**. Once the
shipment is no longer cancellable the call returns **HTTP 409** — that is the expected
answer, not a bug. There is no other reversal path, so a label that has been collected is
a real cost the user has to absorb.

## Error handling summary

| Status | Meaning | Retry? |
|---|---|---|
| 400 | Payload failed Karrio's validation | No — read `errors[].details` and fix the field |
| 401 | Bad or missing token | No |
| 404 | Object missing, or wrong test/live mode | No |
| 409 | Object is not in a state that allows this | No — re-read and branch on `status` |
| 424 | The **carrier** rejected it; `messages[].carrier_name` names which | Only after changing something |
| 500 | Karrio instance failure | Yes, with backoff |

There is no 429 anywhere in this API, and no rate-limit headers. If you are being
throttled, it will surface as a 424 from the carrier.
