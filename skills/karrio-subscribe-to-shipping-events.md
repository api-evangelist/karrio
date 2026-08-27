---
name: karrio-subscribe-to-shipping-events
description: Subscribe to Karrio webhook events with the event names the contract actually accepts — not the ones the documentation shows.
api: Karrio Shipping API
spec: openapi/karrio-api-openapi.yml
spec_version: 2026.1.32
operations:
  - "$$$$$$$create"
  - "$$$$$$$list"
  - "$$$$$$$retrieve"
  - "$$$$$$$update"
  - "$$$$$$$test"
  - "$$$$$$$remove"
  - "$$$$$$$$resend_webhooks"
generated: '2026-08-27'
method: generated
source: Grounded in openapi/karrio-api-openapi.yml. Event names taken from the Webhook.enabled_events enum in the spec.
---

# Subscribe to shipping events

## Read this before you write any code

Karrio's webhooks documentation page shows a subscription payload using dotted event
names — `shipment.created`, `shipment.purchased`, `tracking.status_updated`,
`tracking.delivered`, `order.fulfilled`.

**Four of those five do not exist.** The contract's `enabled_events` enum uses
snake_case with no dots, and there is no `shipment_created` and no `tracking_*`
namespace at all. Copying the documented payload gets you a validation error.

Use these, and only these:

```
all
shipment_purchased          shipment_cancelled          shipment_fulfilled
shipment_out_for_delivery   shipment_needs_attention    shipment_delivery_failed
tracker_created             tracker_updated
pickup_scheduled            pickup_cancelled            pickup_closed
order_created               order_updated               order_fulfilled
order_cancelled             order_delivered
batch_queued                batch_running               batch_completed
batch_failed
```

## Step 1 — create the subscription

`$$$$$$$create` — `POST /v1/webhooks`

Required: `url` and `enabled_events`. Optional: `description`, `disabled`.

Subscribe narrowly. `all` is available but means every event above, including the batch
lifecycle chatter, which is rarely what a consumer wants.

The response carries a `secret` — the header signature secret. Store it.

## Step 2 — prove the endpoint works before you rely on it

`$$$$$$$test` — `POST /v1/webhooks/{id}/test`

Fires a test delivery at the registered URL. Do this immediately after creating the
subscription. It is the only way to confirm the endpoint is reachable from the Karrio
instance, which matters more than usual here because self-hosted instances often sit
inside a private network.

## Step 3 — verification (know the gap)

Karrio signs deliveries with the `secret` on the webhook object, and the docs assert
"cryptographic signatures to verify authenticity".

**Karrio does not publish the header name or the signing algorithm.** No page found
states either. You cannot implement verification from published material alone — you have
to read `modules/events` in the source, or ask Karrio.

Until you can verify signatures, treat the payload as untrusted: use it as a *hint that
something changed*, then re-read the object from the API before acting on it. Never take
a state transition straight from an unverified webhook body.

## Step 4 — operate it

- `$$$$$$$list` — `GET /v1/webhooks` — audit what is subscribed. Each object carries
  `last_event_at`, delivery counts and last response status.
- `$$$$$$$update` — `PATCH /v1/webhooks/{id}` — change the URL or the event set. Set
  `disabled: true` to pause without losing the subscription.
- `$$$$$$$remove` — `DELETE /v1/webhooks/{id}`.

Karrio retries failed deliveries with exponential backoff and moves exhausted deliveries
to a dead-letter queue.

## Step 5 — recover missed events

`$$$$$$$$resend_webhooks` — `POST /v1/batches/webhooks`

Replays webhook deliveries. This is the recovery path after your endpoint was down —
use it rather than backfilling by polling every object.

## What events do not cover

Full event history, streaming and custom handlers are an Insiders-tier feature on the
GraphQL management API (`events` connection, cursor-paginated via `page_info`). Note the
inconsistency: the REST API paginates with `limit`/`offset`, the GraphQL events surface
paginates with cursors.
