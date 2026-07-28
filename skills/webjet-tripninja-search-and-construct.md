---
name: Trip Ninja SmartFlights — search and construct itineraries
description: >-
  Run the two-call SmartFlights flight-construction loop: post a traveller search to /v3/get-searches/,
  execute the returned content queries against your OWN GDS/NDC/aggregator, then post the compressed
  results to /v3/generate-solutions/ to receive constructed (split-ticket / virtual-interlined)
  itineraries with markups.
api: openapi/webjet-tripninja-smartflights-get-searches-openapi.yml
also_uses: openapi/webjet-tripninja-smartflights-generate-solutions-openapi.yml
operations:
  - 'POST /v3/get-searches/'
  - 'POST /v3/generate-solutions/'
generated: '2026-07-28'
method: generated
source: >-
  openapi/webjet-tripninja-smartflights-get-searches-openapi.yml,
  openapi/webjet-tripninja-smartflights-generate-solutions-openapi.yml,
  https://devhub.tripninja.io/smartflights/get-searches/,
  https://devhub.tripninja.io/smartflights/generate-solutions/
---

# Trip Ninja SmartFlights — search and construct itineraries

> The four SmartFlights OpenAPI documents publish `operationId: ""` (empty string) on every
> operation. Steps below are therefore anchored on METHOD + PATH, which is what the provider actually
> publishes. Do not invent operationIds.

## Before you start

- You need a Trip Ninja **access token** issued from the Admin Panel (`https://app.tripninja.io`)
  under Developer Resources. There is no self-serve signup.
- Your source IP must be **allow-listed by Trip Ninja**. Without that every call returns
  `403 {"message":"Forbidden"}` regardless of the token.
- Trip Ninja **supplies no air content**. You must already hold at least one active content-source
  integration (GDS, NDC or aggregator) and be able to book multiple PNRs for one traveller itinerary.
- Base URL: `https://sandbox.tripninja.io` for development (capped at 5,000 requests/day).
  Production is `https://api.tripninja.io`, issued only after certification.
- Auth header on every call: `Authorization: Token <access_token>`. Accounts not created through the
  Admin Panel use HTTP Basic instead (`Authorization: Basic base64(USERNAME:PASSWORD)`).

## Step 1 — POST /v3/get-searches/

Body is `GetSearchesRequest`. `segments` is the only required member; each segment requires `id`
(sequential from 1), `from_iata`, `to_iata` and `departure_date`.

- `from_type` / `to_type` are `"C"` (city, includes nearby airports) or `"A"` (airport). Default `C`.
- `cabin_class` is one of `E`, `PE`, `BC`, `FC`, `PFC` — set globally or per segment.
- `travellers` is an array of IATA passenger type codes: `ADT`, `CHD`, `INF`, `MIL`.
- `num_results` is an integer between 50 and 5000 (default 50).
- `currency` is an ISO 4217 code; `country_code` is a two-letter search origin.
- `time_value` trades price against duration: `True Cost = total_price + (time_value ÷ 60) × flight duration`. Leave at 0 to sort by price only.
- `markup_source` names a markup model configured in your Admin Panel.

Read from the response (`GetSearchesResponse`):

- `trip_id` — **echo it back unmodified** on every later call. Altering it returns `IE41`; an unknown
  value returns `IE52`.
- `datasource_requests[]` — each has a `datasource_request_id` and `datasource_segments[]`. These are
  the queries Trip Ninja wants you to run.

## Step 2 — run the queries against your own content sources

For each `datasource_request`, execute the described search against your GDS/NDC/aggregator and
collect the priced options. Trip Ninja never touches your content sources.

## Step 3 — POST /v3/generate-solutions/

Build a body whose `datasource_responses` is a **map keyed by `datasource_request_id`**, with your
formatted results as the value, plus the `trip_id` from step 1.

**The body must be compressed.** zlib-deflate the JSON string, then Base64-encode the compressed
bytes. Sending plain JSON fails with `IE08`. The first-party SDKs do this for you —
`tn_client.prepare_data_for_generate_solutions(json_string)` (Python, `pip install -U tn_sdk`) or
`tnClient.PrepareDataForGenerateSolutions(json)` (C#, `dotnet add package TripNinja.SDK`) — default
compression level 6. Manual .NET / Java / PHP / Python snippets are published in the docs.

Optional `metadata` on each query is echoed back inside the matching itinerary so the response is
display-ready; omitting it makes the payload much smaller but you must re-join your own metadata.

Read from the response: constructed itineraries, each with an `itinerary_id`, its
`pricing_solution_ids`, the per-segment breakdown, and the machine-learned `markup`.

## Step 4 — present and book

Present the itineraries in your own UI. Booking happens through **your** content sources, not through
Trip Ninja. Because SmartFlights returns split-ticket itineraries, display the Booking Codes (PNR
numbers) per ticket. Then report the outcome — see the
`webjet-tripninja-report-booking-and-cancellation` skill.

## Rules an agent must respect

- **There is no idempotency contract.** No idempotency key exists anywhere in this API. Do not
  auto-retry a POST on an ambiguous outcome without a human decision.
- No `X-RateLimit-*`, `RateLimit` or `Retry-After` header is published and no `429` response is
  declared. Sandbox is hard-capped at 5,000 requests/day; there is no published production limit.
- Errors are **not** RFC 9457. The envelope is `{"status": "IExx", "message": "..."}` on failure and
  `{"status": 200, "message": "success"}` on success — the `status` field changes type. The full
  code list is in `errors/webjet-error-codes.yml`.
- Common failures at this stage: `IE09` (date in the past), `IE11` (date not `YYYY-MM-DD`),
  `IE10` (segments out of chronological order), `IE58`/`IE59` (IATA code not exactly 3 characters),
  `IE60`/`IE61` (`from_type`/`to_type` not `A`/`C`), `IE62` (bad cabin class), `IE63` (bad traveller
  type), `IE27` (`num_results` outside 50–5000), `IE75` (more than 6 searched segments),
  `IE08` (uncompressed generate-solutions body), `IE70`/`IE71`/`IE72`/`IE73` (malformed
  `datasource_responses`).
- There is **no read, list or export operation**. Nothing you create here can be fetched back.
