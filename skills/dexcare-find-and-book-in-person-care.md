---
name: dexcare-find-and-book-in-person-care
description: Find open in-person appointment slots across a health system's clinics and book one through DexCare.
api: DexCare Care Options API + DexCare Slots Availability API + DexCare Visit Booking API
generated: '2026-08-15'
method: generated
source: openapi/dexcare-care-options-openapi.yml, openapi/dexcare-slots-availability-openapi.yml, openapi/dexcare-visit-booking-openapi.yml
operations:
  - getAggregatedSlotsV1
  - aggregatedSlotsV1
  - availableProvidersV1
  - getSlotsAvailability
  - getSingleProviderTimeslots
  - createQueuedGuestVisit
  - slotTaken
---

# Find and book in-person care with DexCare

DexCare calls this flow **Care Direct** (the patient wants care near a location and is
agnostic about the clinician) or **Provider Direct** (the patient wants a specific
clinician). Both end at the same booking call.

## Before you start

- **Host.** There is no shared DexCare base URL. Every health system is provisioned its
  own UAT and production host. Care Options and Slots Availability are published with
  the templated server `https://api.{customerShorthand}.dexcare.io/v1` and `/v5`;
  substitute the host your health system gave you.
- **Auth.** Care Options, Slots Availability and Visit Booking all use the `ApiKey`
  scheme — an `x-api-key` request header, issued by DexCare to the customer. Do not put
  it in a browser or mobile client. See `authentication/dexcare-authentication.yml`.
- **Tracing.** Every operation accepts an optional `correlation-id` request header, and
  the gateway returns a `correlation-id` on the response. Send your own on every call
  and log it — it is the only request-correlation handle DexCare exposes.
- **Product tagging.** Most read operations accept an optional `product` query parameter
  (`createQueuedGuestVisit` **requires** it). Set it to the use case you are calling
  from, e.g. `findadoctor` or `flushot`; DexCare uses it to isolate issues per use case.

## Steps

1. **Search for availability across clinics.**
   Call `getAggregatedSlotsV1` (`GET /availability/slots`). `daysOfSlots` is required.
   Narrow with `latitude` + `longitude` + `radius`, or `postalCode`, plus
   `visitTypeNames`, `specialty`, `clinicTypes`, `startDate`/`endDate`, and
   `newOrEstablishedPatient`. Pass `userTimezone` so returned times land in the
   patient's zone.
   For a filter set too large for a query string, use `aggregatedSlotsV1`
   (`POST /availability/slots`) instead.

2. **If the patient wants a specific clinician, resolve the provider first.**
   Call `availableProvidersV1` (`POST /availability/providers`) to find clinicians with
   matching availability, then `getSingleProviderTimeslots`
   (`GET /providers/{npid}/timeslots`) with the clinician's **NPI** as `npid`. That
   operation takes `daysOfSlots`, `visitTypeName`, `startDate`/`endDate`,
   `departmentEmrId`, `departmentEmrSystemId`, `departmentUrlName` and
   `newOrEstablishedPatient`.

3. **Or search slots directly by criteria.**
   `getSlotsAvailability` (`POST /slots/search`) takes a body whose only required member
   is `criteriaItems`, plus optional `startDate`, `endDate` and `visitTypeNames`.

4. **Resolve the patient.**
   Booking needs patient identity. For a signed-in patient use `GET /v1/patient` with
   their JWT; when booking on behalf of someone else, `POST /v1/patient/other` finds or
   creates the record. Both are documented on
   <https://developers.dexcarehealth.com/api/patient/>.

5. **Book the slot.**
   Call `createQueuedGuestVisit` (`POST /booking/queued-guest-visit`). `product` is a
   **required** query parameter. The body requires `patient`, `billingInfo` and
   `visitDetails`; `actor` and `additionalDetails` are optional. Send a
   `correlation-id` header.

6. **Release the slot back if you hold it in your own UI.**
   If your system owns the schedule and DexCare is reading it, notify DexCare with
   `slotTaken` (`POST /slots/slot-taken`) — body requires `npi`, `departmentId`,
   `visitTypeId`, `slotDateTime` and `ehrInstance` — or `slotReleased`
   (`POST /slots/slot-released`). Both return `204`.

## Errors and retries

DexCare does **not** use RFC 9457 problem+json. Errors come back as a bare JSON object,
either `{"error": "..."}` or `{"message": "..."}` depending on the service, with the
meaning carried by the HTTP status. See `errors/dexcare-problem-types.yml`.

- `400` — malformed request or a missing required field. Do not retry unchanged.
- `401` / `403` — bad or missing `x-api-key`, or an expired JWT. Re-issue credentials;
  do not retry.
- `404` — the department, provider or slot does not exist on this tenant.
- `500` — DexCare-side failure. Retry with backoff, reusing your `correlation-id`.

**There is no idempotency contract.** DexCare publishes no idempotency key header, so a
retried `createQueuedGuestVisit` can create a second visit. Retry booking only after
confirming the first attempt did not succeed.

**There are no published rate limits** and no `RateLimit-*` or `Retry-After` headers, so
throttle conservatively on your own budget rather than reacting to a signal.
