# Karrio

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Karrio is an open-source, headless multi-carrier shipping platform for developers and logistics teams. Its unified REST API and GraphQL management API abstract more than thirty carrier integrations behind one resource-oriented interface covering live rating, label generation and purchase, package tracking, pickups, manifests, orders, customs documents and webhooks. Karrio can be self-hosted from the Apache-2.0 source, run as the managed Karrio Platform, or embedded and white-labelled under a commercial license.

**Areas:** Shipping · Logistics · Label Generation · Package Tracking · Carrier · Fulfillment · Open Source · Multi-Carrier · Ratings · Webhook

## Karrio on APIs.io

Everything profiled for Karrio is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/karrio/](https://apis.io/providers/karrio/)** — or go straight to one of the APIs or properties below.

### APIs (17)

- [Karrio Addresses API](https://apis.io/apis/karrio/karrio-addresses-api/) — This is an object representing your Karrio shipping address. You can retrieve all addresses related to your Karrio account. Address objects are linked to…
- [Karrio Auth API](https://apis.io/apis/karrio/karrio-auth-api/) — API authentication resources.
- [Karrio Batches API](https://apis.io/apis/karrio/karrio-batches-api/) — This is an object representing your Karrio batch operation. You can retrieve all batch operations historically for your Karrio account.
- [Karrio Carriers API](https://apis.io/apis/karrio/karrio-carriers-api/) — This is an object representing your Karrio carrier extension. You can retrieve all supported carrier extensions available.
- [Karrio Connections API](https://apis.io/apis/karrio/karrio-connections-api/) — This is an object representing your Karrio carrier connections. You can retrieve all carrier connections available to your account. The `carrier_id` is a…
- [Karrio Documents API](https://apis.io/apis/karrio/karrio-documents-api/) — This is an object representing your Karrio document upload record. A Document upload record keep traces of shipping trade documents uploaded to carriers to…
- [Karrio Manifests API](https://apis.io/apis/karrio/karrio-manifests-api/) — This is an object representing your Karrio manifest details. Some carriers require manifests to be created after labels are generated. A manifest is a…
- [Karrio Orders API](https://apis.io/apis/karrio/karrio-orders-api/) — This is an object representing your Karrio order. You can create Karrio orders to organize your shipments and ship line items separately.
- [Karrio Parcels API](https://apis.io/apis/karrio/karrio-parcels-api/) — This is an object representing your Karrio shipping parcel. Parcel objects are linked to your shipment history, and can be used for recurring shipping using…
- [Karrio Pickups API](https://apis.io/apis/karrio/karrio-pickups-api/) — This is an object representing your Karrio pickup booking. You can retrieve all pickup booked historically for your Karrio account shipments.
- [Karrio Products API](https://apis.io/apis/karrio/karrio-products-api/) — This is an object representing your Karrio product template. Product templates are reusable commodity definitions that can be used in customs declarations…
- [Karrio Proxy API](https://apis.io/apis/karrio/karrio-proxy-api/) — In some scenarios, all we need is to send request to a carrier using the Karrio unified API. The Proxy API comes handy for that as it turn Karrio into a…
- [Karrio Shipments API](https://apis.io/apis/karrio/karrio-shipments-api/) — This is an object representing your Karrio shipment. A Shipment guides you through process of preparing and purchasing a label for an order. A Shipment…
- [Karrio Trackers API](https://apis.io/apis/karrio/karrio-trackers-api/) — This is an object representing your Karrio shipment tracker. A shipment tracker is an object attached to a shipment by it's tracking number. The tracker…
- [Karrio Webhooks API](https://apis.io/apis/karrio/karrio-webhooks-api/) — This is an object representing your Karrio webhook. You can configure webhook endpoints via the API to be notified about events happen in your Karrio account.
- [Karrio Karrio API](https://apis.io/apis/karrio/karrio-karrio-api-api/) — The Karrio API API from Karrio — 1 operation(s) for karrio api.
- [Karrio References API](https://apis.io/apis/karrio/karrio-references-api/) — The References API from Karrio — 1 operation(s) for references.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Karrio Webhooks](https://apis.io/asyncapis/karrio/karrio-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Karrio MCP Server](https://apis.io/servers/karrio/karrio-mcp-server/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Karrio Agentic Access](https://apis.io/agentic-access/karrio/karrio-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Karrio Api Overlay](https://apis.io/overlays/karrio/karrio-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Karrio Authentication](https://apis.io/security/karrio/karrio-authentication/)
- [Karrio Domain Security](https://apis.io/security/karrio/karrio-domain-security/)
- [Karrio Vulnerability Disclosure](https://apis.io/security/karrio/karrio-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Karrio Scopes](https://apis.io/scopes/karrio/karrio-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Karrio Plans Pricing](https://apis.io/plans/karrio/karrio-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Karrio Rate Limits](https://apis.io/rate-limits/karrio/karrio-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Karrio Finops](https://apis.io/finops/karrio/karrio-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://karrio.io/)
- [Documentation](https://karrio.io/docs)
- [API reference](https://karrio.io/docs/api-reference)
- [Pricing](https://karrio.io/platform)
- [Support](https://github.com/orgs/karrioapi/discussions)
- [Blog](https://karrio.io/blog)
- [Terms of service](https://karrio.io/terms-of-service)
- [Privacy policy](https://karrio.io/privacy-policy)
- [GitHub](https://github.com/karrioapi)

## API Evangelist rating

**Kin Score 52.2** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 65.0 |
| Contract quality | 55.0 |
| Contract governance | 4.5 |
| Operational transparency | 36.8 |
| Developer ergonomics | 58.9 |
| Access clarity | 63.2 |

**Agent readiness 39.1** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | derived (partial credit) |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | documented (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | partial |
| Operation Distinctiveness | distinct |

**Regulatory alignment 40.0** — Telecommunications

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/karrio/](https://apis.io/providers/karrio/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Karrio: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/karrio/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
