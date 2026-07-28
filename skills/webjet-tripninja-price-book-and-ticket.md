---
name: Trip Ninja — price, book, queue and ticket (GitHub-published surface)
description: >-
  Drive the fuller Trip Ninja booking surface that is published in Trip Ninja's own GitHub docs
  repository but is NOT documented on the current developer hub — price confirmation, booking
  creation, booking retrieval, ticketing-queue placement, ticketing and cancellation.
api: openapi/webjet-tripninja-pricing-booking-openapi.yml
also_uses: openapi/webjet-tripninja-flights-core-openapi.yml
operations:
  - PriceConfirm
  - CreateBooking
  - BookingDetail
  - ListBooking
  - AddBookingToTicketingQueue
  - Ticket
  - CancelBooking
  - FlightSearch
  - PriceConfirmationReport
  - BookingReport
  - TicketingReport
  - CancelBookingReport
generated: '2026-07-28'
method: generated
source: >-
  openapi/webjet-tripninja-pricing-booking-openapi.yml,
  openapi/webjet-tripninja-flights-core-openapi.yml,
  https://github.com/trip-ninja-inc/trip_ninja_api_docs
---

# Trip Ninja — price, book, queue and ticket

## Provenance warning — read before using

These operations come from `https://github.com/trip-ninja-inc/trip_ninja_api_docs`, a **public
repository in Trip Ninja's own GitHub organisation**, last updated **2023-12-14**. They are real,
first-party OpenAPI 3.0.0 documents with real `operationId`s — unlike the current developer-hub
documents, which publish `operationId: ""`. But the current developer hub does **not** document this
surface, so:

- Treat availability as **unverified**. Confirm with Trip Ninja before building against it.
- The server named in these documents is `https://preprodapi.tripninja.io` (live, AWS API Gateway,
  returns `403 {"message":"Missing Authentication Token"}` to non-allow-listed callers), not
  `sandbox.tripninja.io`.
- Nothing here is marked deprecated by Trip Ninja. It is simply unlinked.

## Prerequisites

Same gate as every Trip Ninja surface: a Trip Ninja-issued credential, an allow-listed source IP, and
a commercial agreement. See `authentication/webjet-authentication.yml`.

## Flow A — pricing and booking (`openapi/webjet-tripninja-pricing-booking-openapi.yml`)

1. **`PriceConfirm`** — `POST /price/{endpoint}/`. Re-price the selected itinerary before you commit.
   Body `PriceConfirmRequest`, response `PriceConfirmResponse`. Always re-price: air prices move
   between shopping and booking.
2. **`CreateBooking`** — `POST /book/{endpoint}/`. Body `CreateBookingRequest` (passengers, itinerary,
   credentials, frequent-flyer cards, baggage). This is the consequential step.
3. **`BookingDetail`** — `GET /book/trip/{super_trip_id}/?agency={agency}`. Read one booking back.
   `super_trip_id` is what groups the multiple PNRs of a split-ticket journey.
4. **`ListBooking`** — `GET /book/list/?agency={agency}&user={agent_email}&offset={offset}&limit={limit}`.
   The **only paginated operation** anywhere in the Trip Ninja corpus.
5. **`AddBookingToTicketingQueue`** — `POST /queue/`. Body `AddBookingToTicketingQueueRequest`.
6. **`Ticket`** — `POST /ticket/`. Body `TicketingRequest`. Issuing tickets is irreversible and costs
   real money.
7. **`CancelBooking`** — `DELETE /book/`. Body `CancelRequest`.

## Flow B — search-and-report core (`openapi/webjet-tripninja-flights-core-openapi.yml`)

`FlightSearch` (`POST /search/flights/{endpoint}/`) then the four report operations —
`PriceConfirmationReport` (`POST /pre-booking/`), `BookingReport` (`POST /booking/`),
`TicketingReport` (`POST /ticketing/`) and `CancelBookingReport` (`POST /cancel/`). This is the older
shape of the same reporting loop the current v3 surface expresses as `/v3/report/book/` and
`/v3/report/cancel/`, plus a ticketing report the v3 surface no longer has.

## Rules an agent must respect

- **`CreateBooking` and `Ticket` are money-moving, irreversible actions.** Require explicit human
  approval per call. Never batch them, never retry them automatically.
- There is still **no idempotency key** on this surface. `is_retry`-style flags do not make a retry
  safe.
- No `securitySchemes` block is declared in these documents either; auth is the same
  `Authorization: Token <access_token>` / HTTP Basic contract described in
  `authentication/webjet-authentication.yml`.
- `ListBooking` is the only read operation with paging — use `offset`/`limit` and do not assume any
  other operation pages.
- Error handling is the same proprietary `{status, message}` envelope with `IExx` codes documented in
  `errors/webjet-error-codes.yml`. The MSDP spec additionally declares its own typed error schemas
  (`MSDPSearchErrorDetails`, `MSDPOTABookingErrorDetails`, and siblings).
