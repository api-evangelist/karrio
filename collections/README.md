# Collections — regeneration required

The Postman and Open Collection files previously in this directory were generated from a
scaffolded OpenAPI that has since been removed. That scaffold described 7 invented
operations against `https://api.karrio.io`, a host that no longer resolves. The real
Karrio contract — harvested 2026-08-27 to `openapi/karrio-api-openapi.yml`, Karrio API
2026.1.32 — has 95 operations across 65 paths.

The stale collections were deleted rather than left in place, because a collection that
sends a developer at a dead host with invented request shapes is worse than no collection.

Regenerate with the network-wide pass:

    node all/0-working/openapi-to-collections.js

Karrio publishes no Postman collection or public workspace of its own.
