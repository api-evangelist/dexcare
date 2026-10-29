# DexCare

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

DexCare is a healthcare navigation and care-orchestration platform, launched from within Providence Health, that connects patients to available care across fragmented health systems while helping providers fill capacity and reduce wait times. Its products span Search & Schedule, Virtual On Demand, Provider Data Management (PDM+), Optimize AI, and Acquire. DexCare publishes six OpenAPI definitions on its own developer portal covering 27 operations across the Visit Service, Visit Booking, Care Options, Slots Availability, Omni Search and Provider Data Management services, alongside prose references for the Patient and Reporting APIs and native iOS, Android and JavaScript SDKs. There is no shared base URL: every health system is provisioned its own UAT and production hosts, and the published specifications use templated or per-tenant servers. Public directory, availability and search endpoints are open, while PHI/PII endpoints require an OAuth 2.0-issued JWT bearer token and server-to-server services require a DexCare-issued x-api-key. DexCare operates as a HIPAA business associate. This profile was enriched from DexCare's public developer surface as part of the API Evangelist network (originally surfaced as an ICONIQ Capital portfolio lead).

**Areas:** Company · Healthcare · Health IT · Patient Access · Scheduling · Virtual Care · Telehealth · Care Navigation · Provider Data · Search · SDK

## DexCare on APIs.io

Everything profiled for DexCare is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/dexcare/](https://apis.io/providers/dexcare/)** — or go straight to one of the APIs or properties below.

### APIs (20)

- [DexCare REST API](https://apis.io/apis/dexcare/dexcare-rest-api/) — RESTful service for accessing business information and performing actions against DexCare-managed healthcare environments. The umbrella reference covering…
- [DexCare Assignmentqualifiers API](https://apis.io/apis/dexcare/dexcare-assignmentqualifiers-api/) — The assignmentqualifiers API from DexCare — 1 operation(s) for assignmentqualifiers.
- [DexCare External API](https://apis.io/apis/dexcare/dexcare-external-api/) — The external API from DexCare — 2 operation(s) for external.
- [DexCare Modalities API](https://apis.io/apis/dexcare/dexcare-modalities-api/) — The modalities API from DexCare — 1 operation(s) for modalities.
- [DexCare Omni Analytics API](https://apis.io/apis/dexcare/dexcare-omnianalytics-api/) — The OmniAnalytics API from DexCare — 1 operation(s) for omnianalytics.
- [DexCare Omni Data API](https://apis.io/apis/dexcare/dexcare-omnidata-api/) — The OmniData API from DexCare — 1 operation(s) for omnidata.
- [DexCare Omni Search API](https://apis.io/apis/dexcare/dexcare-omnisearch-api/) — The OmniSearch API from DexCare — 1 operation(s) for omnisearch.
- [DexCare Omni Search Analytics API](https://apis.io/apis/dexcare/dexcare-omnisearchanalytics-api/) — The OmniSearchAnalytics API from DexCare — 1 operation(s) for omnisearchanalytics.
- [DexCare Omni Search Facets API](https://apis.io/apis/dexcare/dexcare-omnisearchfacets-api/) — The OmniSearchFacets API from DexCare — 1 operation(s) for omnisearchfacets.
- [DexCare Omni Sitemap API](https://apis.io/apis/dexcare/dexcare-omnisitemap-api/) — The OmniSitemap API from DexCare — 1 operation(s) for omnisitemap.
- [DexCare Omni Slugs API](https://apis.io/apis/dexcare/dexcare-omnislugs-api/) — The OmniSlugs API from DexCare — 1 operation(s) for omnislugs.
- [DexCare Omni Synonyms API](https://apis.io/apis/dexcare/dexcare-omnisynonyms-api/) — The OmniSynonyms API from DexCare — 1 operation(s) for omnisynonyms.
- [DexCare Pdm API](https://apis.io/apis/dexcare/dexcare-pdm-api/) — The Pdm API from DexCare — 2 operation(s) for pdm.
- [DexCare Providers API](https://apis.io/apis/dexcare/dexcare-providers-api/) — The Providers API from DexCare — 1 operation(s) for providers.
- [DexCare Queued Guest Visit API](https://apis.io/apis/dexcare/dexcare-queued-guest-visit-api/) — The queued-guest-visit API from DexCare — 1 operation(s) for queued-guest-visit.
- [DexCare Slots API](https://apis.io/apis/dexcare/dexcare-slots-api/) — The Slots API from DexCare — 3 operation(s) for slots.
- [DexCare Tokbox API](https://apis.io/apis/dexcare/dexcare-tokbox-api/) — The tokbox API from DexCare — 1 operation(s) for tokbox.
- [DexCare Virtual Visits API](https://apis.io/apis/dexcare/dexcare-virtual-visits-api/) — The virtual-visits API from DexCare — 1 operation(s) for virtual-visits.
- [DexCare Visits API](https://apis.io/apis/dexcare/dexcare-visits-api/) — The visits API from DexCare — 3 operation(s) for visits.
- [DexCare Waittimes API](https://apis.io/apis/dexcare/dexcare-waittimes-api/) — The waittimes API from DexCare — 2 operation(s) for waittimes.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [DexCare (candidate MCP server)](https://apis.io/servers/dexcare/dexcare-candidate-mcp-server/)

#### Overlays (6)

OpenAPI overlays, including conversational phrasing for each operation.

- [Dexcare Care Options Overlay](https://apis.io/overlays/dexcare/dexcare-care-options-overlay/)
- [Dexcare Omni Search Overlay](https://apis.io/overlays/dexcare/dexcare-omni-search-overlay/)
- [Dexcare Provider Data Management Overlay](https://apis.io/overlays/dexcare/dexcare-provider-data-management-overlay/)
- [Dexcare Slots Availability Overlay](https://apis.io/overlays/dexcare/dexcare-slots-availability-overlay/)
- [Dexcare Visit Booking Overlay](https://apis.io/overlays/dexcare/dexcare-visit-booking-overlay/)
- [Dexcare Visit Service Overlay](https://apis.io/overlays/dexcare/dexcare-visit-service-overlay/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Dexcare Authentication](https://apis.io/security/dexcare/dexcare-authentication/)
- [Dexcare Domain Security](https://apis.io/security/dexcare/dexcare-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Dexcare Plans Pricing](https://apis.io/plans/dexcare/dexcare-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Dexcare Rate Limits](https://apis.io/rate-limits/dexcare/dexcare-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://dexcare.com/)
- [Documentation](https://developers.dexcarehealth.com/)
- [Getting started](https://developers.dexcarehealth.com/jssdk/getting-started/)
- [API reference](https://developers.dexcarehealth.com/api/)
- [Support](https://developers.dexcarehealth.com/home/support)
- [Blog](https://dexcare.com/news/)
- [Compliance](https://dexcare.com/privacy-policy/)
- [Terms of service](https://dexcare.com/terms-of-service/)
- [Privacy policy](https://dexcare.com/privacy-policy/)
- [GitHub](https://github.com/DexCare)

## API Evangelist rating

**Kin Score 47.3** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 78.6 |
| Contract quality | 49.7 |
| Contract governance | 18.2 |
| Operational transparency | 26.3 |
| Developer ergonomics | 73.2 |
| Access clarity | 28.9 |

**Agent readiness 27.7** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 23.4** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/dexcare/](https://apis.io/providers/dexcare/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for DexCare: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/dexcare/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
