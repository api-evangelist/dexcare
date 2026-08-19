---
name: dexcare-omni-search-care
description: Run a natural-language care search across a health system's clinicians and clinics with DexCare Omni Search, and hydrate the results.
api: DexCare Omni Search API
generated: '2026-08-15'
method: generated
source: openapi/dexcare-omni-search-openapi.yml
operations:
  - Omni_Search
  - Get_Data
  - Facet_Search
  - Synonym_Search
  - Get_Slugs
  - Sitemap
  - Submit_Analytics_Events
---

# Search for care in natural language with DexCare Omni Search

Omni Search turns a single free-text input ("where to find the nearest flu shot") into a
structured query using rules-based NLP: tokenizing, stemming, synonym expansion,
spelling correction and intent detection. It returns clinicians and departments.

## Before you start

- **Host.** Omni Search is deployed per brand. The published servers are templated:
  `https://${brand}omni.azurewebsites.net` (prod), with `dev`, `qa` and `uat` prefixes
  for the lower tiers. Substitute your brand.
- **Version in the path.** Endpoints carry a version path segment, e.g.
  `/api/OmniSearch/{version}`. It is technically optional, but omitting it pins you to
  1.x behaviour and hides newer functionality. The published document is version
  `2.21.0`.
- **Auth.** The specification declares `security: [{}]` — no scheme. Treat the surface
  as unauthenticated but brand-scoped.

## Steps

1. **Run the search.**
   `Omni_Search` (`GET /api/OmniSearch/{version}`). `type` is the one **required**
   parameter. Pass the user's text as `search`. Then narrow:
   - Location: `latitude` + `longitude`, or `location`/`locations`, with `distance`;
     or set `inferLocationFromSearch` to let the parser pull it out of the text.
   - Result mix: `clinicians`, `departments`, `clinicVisits`, `videoVisits`,
     `virtualCare`.
   - Availability: `startDate`, `endDate`, `days`, `time`, `soonest`,
     `acceptingNewPatients`.
   - Paging: `top` and `skip`.
   - Presentation: `highlightPreTag` / `highlightPostTag` to wrap matched terms.
   - `includeFallbackResults` to keep a thin result set from coming back empty.
   Any facet-able field can additionally be passed by name as a filter parameter.

2. **Read the search context back.**
   The response's `info` object (`SearchInfo`) carries the `facets` the search used and
   the `filters` it derived — each filter records its `value`, the matched `text`, the
   `facet` it belongs to, and `auto: true` when the NLP inferred it rather than you
   supplying it. Render these as removable chips so the user can see what was inferred.

3. **Hydrate individual records.**
   `Get_Data` (`GET /api/OmniData/{version}`) retrieves clinician or department
   documents by identifier — `ids`, `npis`, `departmentIds`, `departmentUrls`, `urls` —
   with `select` to trim fields and `allLocations` to widen the location set.

4. **Build the filter UI.**
   `Facet_Search` (`GET /api/OmniSearchFacets`) returns the facets available for
   clinicians or departments, optionally with result `count` per facet and restricted to
   `stringvalues`.

5. **Explain a match.**
   `Synonym_Search` (`GET /api/OmniSynonyms`) returns the clinical synonyms behind a
   keyword — how "daily persistent headache" reaches "chronic migraines". DexCare
   maintains this library and health systems can customise it.

6. **Feed SEO and analytics.**
   `Get_Slugs` (`GET /api/OmniSlugs`) and `Sitemap` (`GET /api/OmniSitemap`) support
   generating public profile pages; `Submit_Analytics_Events`
   (`GET /api/OmniAnalytics`) and `Search_Analytics` (`GET /api/OmniSearchAnalytics`)
   submit interaction data back.

## Chaining to booking

Omni Search finds care options; it does not book. Take the `departmentId` /
`npi` off a result and continue with `dexcare-find-and-book-in-person-care`, which uses
the Care Options and Slots Availability APIs to fetch bookable timeslots.

## Errors

Every Omni Search operation declares `400` and a `default` error response modelled by
the `ErrorResponse` schema; several also declare `500`, and `Get_Slugs` declares `404`.
`Sitemap` declares `501` when sitemap generation is not enabled for the brand. There is
no problem+json envelope. See `errors/dexcare-problem-types.yml`.
