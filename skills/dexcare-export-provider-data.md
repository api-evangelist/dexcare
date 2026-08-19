---
name: dexcare-export-provider-data
description: Discover the schema of a DexCare Provider Data Management entity type and page through matching provider/department records.
api: DexCare Provider Data Management (Entities) API
generated: '2026-08-15'
method: generated
source: openapi/dexcare-provider-data-management-openapi.yml
operations:
  - getPdmEntitySchema
  - batchFindEntitiesByType
---

# Export provider data from DexCare PDM

DexCare's Provider Data Management (PDM+) keeps the golden record for clinicians,
departments, locations and business lines. The Entities Service exposes it as two
operations: one that hands you the schema, one that queries records against it. The
schema call first is not optional politeness — PDM is schema-driven and entity shapes
differ per health system.

## Before you start

- **Host.** The published server is `https://entities-service.{hostname}/v1`;
  substitute the hostname your health system was given.
- **Auth.** `ApiKey` — an `x-api-key` header issued by DexCare to the customer. Both
  operations declare it explicitly.
- **Contact.** The specification names `Product Config & SSP`
  (`configuration-ssp@dexcarehealth.com`) as the owning team.

## Steps

1. **Fetch the entity schema.**
   `getPdmEntitySchema` (`GET /pdm/schema/{namespace}/{name}`). Set
   `resolveReferences=true` to inline referenced schemas rather than receive bare
   references — do this once when you build your mapping, and cache the result. The
   response is a `PdmEntitySchema` whose properties are JSON Schema property
   descriptors, so your field mapping can be generated rather than hand-written.

2. **Query the records.**
   `batchFindEntitiesByType` (`POST /pdm/batch`). The body is a
   `PaginatedFilteredEntityRequest`; `type` and `namespace` are **required**
   (e.g. `type: department`, `namespace: healthSystem`).

3. **Filter server-side, not client-side.**
   `filters[]` takes `{field, value, operator}`. `field` may reach into a related
   entity with dot notation (`businessLine.name`). `operator` defaults to `EQUAL` and
   accepts `LIKE`, `GT`, `LT`, `BETWEEN`, `NOT BETWEEN`, `IS NULL`, `IS NOT NULL`,
   `CONTAINS` and `ANY`. `value` may be a string, number, boolean or array.

4. **Pull related entities in the same call.**
   List the related types you also want in `refs` (e.g. `["location"]`), or set
   `resolveAllRefs: true` to resolve every reference. This is how you avoid N+1
   round trips across a provider directory.

5. **Page through the whole set.**
   Use `limit` and `offset` in the request body. This is offset paging, so hold the
   filter set constant across pages; a directory that changes underneath you will
   shift rows between pages.

## Errors

`batchFindEntitiesByType` declares `400`, `401`, `404`, `500` and a `default`, all
via shared `components/responses` bound to the `ApiError` schema. Note a defect worth
guarding against: on `getPdmEntitySchema` the `404` response is wired to the `400`
response component in the published document, so a missing namespace/name may describe
itself as a bad request. Key your handling on the HTTP status, not on the body text.
See `errors/dexcare-problem-types.yml`.

There is no idempotency contract and no published rate limit; both operations are
reads, so retry on `500` with backoff.
