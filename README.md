# Webjet (webjet)

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

Webjet Group Limited (ASX: WJL) is the Australian consumer travel company created when Webjet Limited demerged in September 2024 — the B2C half, while the B2B WebBeds bed bank went to Web Travel Group (ASX: WEB). It operates Webjet OTA (webjet.com.au / webjet.co.nz), which the company describes as the number one online travel agency position in Australia and New Zealand; the GoSee vehicle-rental brands Airport Rentals and Motorhome Republic; and Trip Ninja, a travel-technology business selling flight-construction software to other travel platforms.

Webjet sits downstream of airline distribution — it resells airline content sourced through GDS and NDC connections rather than owning inventory, and holds ATIA/ATAS accreditation A17325 with an IATA accredited agent entity (Webjet Marketing). Its API posture is honestly lopsided: the consumer brands publish no developer documentation and no public API at all, while the group's only public API surface is Trip Ninja's SmartFlights developer hub at devhub.tripninja.io — freely readable docs with eleven OpenAPI 3.0.0 documents rendered in-page, but zero self-serve access.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/webjet/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/webjet/refs/heads/main/apis.yml)

## Tags

- Travel
- Australia
- OTA
- Aviation
- Booking
- Distribution
- Flight Search
- Car Rental
- New Zealand
- Travel Technology

## Timestamps

- **Created:** 2026-07-28
- **Modified:** 2026-07-28

## APIs

### Trip Ninja SmartFlights API

SmartFlights is Trip Ninja's current (v3) flight-construction API. A travel platform posts a traveller search to `/v3/get-searches/`, Trip Ninja returns the set of optimised content queries to run against the platform's own content sources (GDS, NDC or aggregator), the platform posts those zlib-compressed provider responses to `/v3/generate-solutions/`, and SmartFlights returns unified — including split-ticket and virtual-interlined — itineraries with machine-learned markups. Booked and cancelled itineraries are reported back via `/v3/report/book/` and `/v3/report/cancel/`. Trip Ninja supplies no air content of its own.

- **Human URL:** [https://devhub.tripninja.io/smartflights/overview/](https://devhub.tripninja.io/smartflights/overview/)
- **Base URL:** `https://sandbox.tripninja.io` (pre-production; the production base URL is not published)

#### Tags

- Flight Search
- Shopping
- Offers
- Booking
- Reporting
- Virtual Interlining

#### Properties

- [OpenAPI](openapi/webjet-tripninja-smartflights-get-searches-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-smartflights-generate-solutions-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-smartflights-report-book-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-smartflights-report-cancel-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://devhub.tripninja.io/smartflights/overview/)
- [Documentation](https://devhub.tripninja.io/smartflights/setup/)
- [Authentication](https://devhub.tripninja.io/smartflights/authentication/)
- [Documentation](https://devhub.tripninja.io/smartflights/certification/)
- [API Reference](https://devhub.tripninja.io/smartflights/get-searches/)
- [API Reference](https://devhub.tripninja.io/smartflights/generate-solutions/)
- [API Reference](https://devhub.tripninja.io/smartflights/report-booking/)
- [API Reference](https://devhub.tripninja.io/smartflights/report-cancellation/)
- [Postman Collection](https://www.postman.com/tripninjadevteam/trip-ninja-public/collection/5uzd69o/smartflights-api)
- [SDK](https://pypi.org/project/tn-sdk/)
- [SDK](https://www.nuget.org/packages/TripNinja.SDK)

### Trip Ninja Admin Panel API

The single documented Admin Panel operation, `POST /adminpanel/refresh-token/`, exchanges a refresh token for a new access token. Access tokens expire 90 days after issue; refresh tokens have an indefinite lifespan until regenerated. Tokens are minted by a human in the Trip Ninja Admin Panel at app.tripninja.io, not by a self-serve signup.

- **Human URL:** [https://devhub.tripninja.io/smartflights/authentication/](https://devhub.tripninja.io/smartflights/authentication/)
- **Base URL:** `https://sandbox.tripninja.io`

#### Tags

- Authentication
- Tokens

#### Properties

- [OpenAPI](openapi/webjet-tripninja-adminpanel-refresh-token-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://devhub.tripninja.io/smartflights/authentication/)
- [Documentation](https://devhub.tripninja.io/resources/admin-panel)
- [Portal](https://app.tripninja.io)

### Trip Ninja FareStructure API (deprecated)

FareStructure is the deprecated v2 predecessor to SmartFlights, still published under `devhub.tripninja.io/deprecated/farestructure/`. It automates split ticketing across multiple content sources to build multi-city itineraries priced below a single GDS search.

- **Human URL:** [https://devhub.tripninja.io/deprecated/farestructure/overview](https://devhub.tripninja.io/deprecated/farestructure/overview)
- **Base URL:** `https://sandbox.tripninja.io`

#### Tags

- Flight Search
- Split Ticketing
- Deprecated

#### Properties

- [OpenAPI](openapi/webjet-tripninja-farestructure-get-searches-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-farestructure-generate-solutions-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-farestructure-report-book-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-farestructure-report-cancel-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://devhub.tripninja.io/deprecated/farestructure/overview)
- [Documentation](https://devhub.tripninja.io/deprecated/farestructure/setup/)
- [Postman Collection](https://www.postman.com/tripninjadevteam/workspace/trip-ninja-public/collection/20885222-8f976a0a-6fba-4bd1-ba03-ac8325501bff)

### Trip Ninja Virtual Interlining API (deprecated)

Virtual Interlining is the deprecated v2 product that combines segments from carriers with no interline agreement into a single sellable itinerary. Its capability is now folded into SmartFlights.

- **Human URL:** [https://devhub.tripninja.io/deprecated/virtual-interlining/overview](https://devhub.tripninja.io/deprecated/virtual-interlining/overview)
- **Base URL:** `https://sandbox.tripninja.io`

#### Tags

- Virtual Interlining
- Flight Search
- Deprecated

#### Properties

- [OpenAPI](openapi/webjet-tripninja-virtual-interlining-get-searches-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/webjet-tripninja-virtual-interlining-generate-solutions-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://devhub.tripninja.io/deprecated/virtual-interlining/overview)
- [Documentation](https://devhub.tripninja.io/deprecated/virtual-interlining/setup/)
- [Postman Collection](https://www.postman.com/tripninjadevteam/workspace/trip-ninja-public/collection/20885222-9fe46e47-a94a-4866-b2bf-f0c087666451)

### Trip Ninja DataStream API

DataStream is a separately-credentialled Trip Ninja product with its own documentation section and its own public Postman collection. It uses the same Token Authentication scheme as SmartFlights, with a DataStream-specific token generated in the Admin Panel. No OpenAPI document and no base URL for DataStream are published on the developer hub, so none is recorded.

- **Human URL:** [https://devhub.tripninja.io/data-stream/overview/](https://devhub.tripninja.io/data-stream/overview/)

#### Tags

- Data
- Streaming

#### Properties

- [Documentation](https://devhub.tripninja.io/data-stream/overview/)
- [Documentation](https://devhub.tripninja.io/data-stream/setup/)
- [Authentication](https://devhub.tripninja.io/data-stream/authentication/)
- [Postman Collection](https://www.postman.com/tripninjadevteam/workspace/trip-ninja-public/collection/20885222-5fccfe6e-479a-429f-a497-d42a0bb859c9)

## Common Properties

- [Website](https://www.webjetgroup.com/)
- [Documentation](https://devhub.tripninja.io/)
- [Portal](https://app.tripninja.io)
- [GitHub Organization](https://github.com/Webjet)
- [LinkedIn](https://www.linkedin.com/company/webjet-group)
- [Blog](https://www.webjetgroup.com/news/)
- [Terms of Service](https://www.tripninja.io/legal/qt-tc)
- [Privacy Policy](https://www.tripninja.io/legal/privacy-policy)
- [Compliance](https://www.tripninja.io/legal/gdpr)
- [Data Processing Agreement](https://www.tripninja.io/legal/data-processing)
- [Support](https://www.tripninja.io/contact)
- [Website](https://www.tripninja.io/)
- [Website](https://www.motorhomerepublic.com/)
- [Website](https://www.airportrentals.com/)

## Switching Cost

Recorded in full in [review.yml](review.yml).

| Dimension | Finding |
| --- | --- |
| Interface shape | `proprietary-documented` — a Trip Ninja-specific REST/JSON contract with `Authorization: Token <access_token>`. No NDC, OpenTravel/OTA or HTNG anywhere. |
| Second source | `alternatives-with-migration` — comparable flight-construction vendors exist, none on this interface. |
| Exit path | `export-on-request` — no export operation in any spec; 60-day contractual return of Customer Personal Data, but the trained markup models stay with the vendor. |
| Identifier portability | IATA airport/city, carrier and passenger-type codes plus PNR record locators; vendor IDs are session-scoped. |
| Contractual lock-in | 14-day pilot at $0, 1-month auto-renewing term, 30 days' termination notice. Unusually light for travel. |
| Access gate | `commercial-agreement` — docs are open to all; production needs a signed SaaS agreement, Trip Ninja-issued Admin Panel credentials, IP allow-listing and a certification pass. |
| Distribution model | `aggregator-reseller` — Webjet OTA resells sourced content; Trip Ninja carries no content at all. |
| NDC posture | Not an airline. NDC appears only as an acceptable upstream content source a customer must already hold. No certification claimed, no endpoint published. |

## Maintainers

- Kin Lane — kin@apievangelist.com
