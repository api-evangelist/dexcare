---
name: dexcare-create-virtual-visit
description: Check virtual care wait times, create an on-demand virtual visit for a patient, and follow it to the waiting room.
api: DexCare Visit Service API
generated: '2026-08-15'
method: generated
source: openapi/dexcare-visit-service-openapi.yml
operations:
  - getAvailabilityAndWaitTimes
  - getRegionAvailabilityAndWaitTimes
  - getModalities
  - getAssignmentQualifiers
  - createVisitV9
  - updateAcceptTerms
  - getEstimatedVirtualVisitWaitTime
  - visitSummaryV9
---

# Create an on-demand virtual visit with DexCare

DexCare's on-demand virtual (ODV) platform books and runs telemedicine visits. This
flow lets a health system build its own booking experience on top of it.

## Before you start

- **Host.** The Visit Service is published with per-tenant servers of the form
  `https://ecvapi.<tenant>/api/9`. DexCare's own demo tenant in the published
  specification is `https://ecvapi.demo-uat.dex.care/api/9`. Use the host your health
  system was given.
- **Auth.** This API declares two bearer schemes, and they are not interchangeable:
  - `PatientJWT` — the patient's own token, for creating and following their visit.
  - `StaffJWT` — a staff token, required for `visitSummaryV9`.
  Both are `Authorization: Bearer <JWT>`, obtained through the health system's identity
  provider using an OAuth 2 authorization-code flow. The read-only availability
  operations declare **no** security and can be called anonymously.
- **Tracing.** Every operation accepts an optional `correlation-id` request header.

## Steps

1. **Read what care is available.**
   `getAvailabilityAndWaitTimes` (`GET /regions/waittimes`) returns availability and
   estimated wait across regions; filter with `regionCodes`, `assignmentQualifiers`,
   `visitTypeNames`, `practiceId`, `homeMarket`. For a single region use
   `getRegionAvailabilityAndWaitTimes` (`GET /regions/{regionCode}/waittimes`).
   Neither requires auth, so both are safe to call from a public care-finder page.

2. **Load the supporting vocabularies.**
   `getModalities` (`GET /modalities`) lists supported care modalities.
   `getAssignmentQualifiers` (`GET /assignmentqualifiers`) lists the qualifiers DexCare
   uses to route a visit to an eligible clinician. Cache both; they change rarely.

3. **Resolve the patient.**
   `GET /v1/patient` with the patient's JWT returns their DexCare `id` (a GUID) plus
   the PII the visit needs — `addresses`, `email`, `homePhone`. When an authorized
   caregiver books for someone else, `POST /v1/patient/other` finds or creates that
   record. See <https://developers.dexcarehealth.com/api/patient/>.

4. **Create the visit.**
   `createVisitV9` (`POST /visits`) with the `PatientJWT`. The body requires `patient`,
   `billingInfo` and `visitDetails`; `actor` and `additionalDetails` are optional.
   Handle `409` explicitly — it means a conflicting visit already exists for this
   patient, and is the response you will hit on a naive retry.

5. **Record terms acceptance.**
   `updateAcceptTerms` (`PUT /visits/{visitId}/acceptTerms`) with the `PatientJWT`.

6. **Follow the visit into the waiting room.**
   Poll `getEstimatedVirtualVisitWaitTime` (`GET /visits/{visitId}/waittime`) with the
   `PatientJWT`. Back off between polls — there is no published polling budget and no
   rate-limit header to read.

7. **Retrieve the summary afterwards (staff only).**
   `visitSummaryV9` (`GET /visits/{visitId}/summary`) requires the `StaffJWT`. A
   `PatientJWT` will be rejected here.

## PHI handling

Everything from step 3 onward carries protected health information. DexCare operates as
a HIPAA business associate and relies on the health system to manage that information —
log the `correlation-id`, never the payload.

## Errors and retries

Errors are bare JSON objects (`{"error": ...}` / `{"message": ...}`) with meaning in the
HTTP status; there is no RFC 9457 envelope. See `errors/dexcare-problem-types.yml`.

- `400` — invalid body. Fix and resend.
- `401` — missing/expired JWT; `403` — wrong token class for the operation.
- `404` — unknown `visitId` on this tenant.
- `409` — conflicting visit state (create, or accept-terms after the fact).
- `500` — DexCare-side failure; retry with backoff.

**No idempotency key exists.** `createVisitV9` is not safe to blind-retry: check for the
`409` and reconcile rather than issuing a second create.
