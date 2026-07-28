---
name: Trip Ninja SmartFlights — report a booking and a cancellation
description: >-
  Close the SmartFlights loop by reporting a booked itinerary to /v3/report/book/ and, if the trip is
  later cancelled, reporting it to /v3/report/cancel/. These calls drive Trip Ninja's billing and its
  markup models; they do not book or cancel anything with an airline.
api: openapi/webjet-tripninja-smartflights-report-book-openapi.yml
also_uses: openapi/webjet-tripninja-smartflights-report-cancel-openapi.yml
operations:
  - 'POST /v3/report/book/'
  - 'POST /v3/report/cancel/'
generated: '2026-07-28'
method: generated
source: >-
  openapi/webjet-tripninja-smartflights-report-book-openapi.yml,
  openapi/webjet-tripninja-smartflights-report-cancel-openapi.yml,
  https://devhub.tripninja.io/smartflights/report-booking/,
  https://devhub.tripninja.io/smartflights/report-cancellation/
---

# Trip Ninja SmartFlights — report a booking and a cancellation

> `operationId` is `""` in both published documents. Anchor on METHOD + PATH.

**What these operations do NOT do:** they do not create, hold, ticket or cancel an airline booking.
The actual booking happens through your own GDS/NDC/aggregator. These are outbound *reports* to Trip
Ninja about something that already happened in the real world — which is exactly why an agent must
treat them as consequential.

## Step 1 — POST /v3/report/book/

Body is `BookingReportRequest`:

- `trip_id` (**required**) — the unmodified value from `/v3/get-searches/`.
- `itinerary_id` (**required**) — from `/v3/generate-solutions/`.
- `customer_booking_reference_id` (optional) — **your own** booking reference, maximum 255 characters.
  Longer values fail with `IE45`.
- `is_retry` (optional, boolean, default `false`) — set `true` when re-sending after a previous
  failed attempt. This is a flag, **not** an idempotency key: it does not guarantee at-most-once.
- `final_price` (optional, number) — the final base price actually booked, excluding markup. Send it
  when the price moved between search and booking.

Success is `{"status": 200, "message": "success"}`.

## Step 2 — POST /v3/report/cancel/ (only if the trip is cancelled)

Body is `CancelRequest`; `trip_id` is the only required member. Success is
`{"status": 200, "message": "Trip has been cancelled."}`.

## Rules an agent must respect

- **Do not retry blindly.** There is no idempotency key. Reporting the same booking twice is not
  defined as safe by the provider.
- **Cancellation is not idempotent.** Cancelling an already-cancelled itinerary returns
  `IE41`: *"Invalid itinerary status, you cannot set a cancelled itinerary to a cancelled itinerary.
  Make sure you are following the workflow stated in documentation."* Treat `IE41` as "already in the
  target state", not as a transient failure to retry.
- Follow the documented order: search -> generate -> book report -> cancel report. Reporting a
  cancellation for a trip that was never reported as booked is outside the published workflow.
- Failure codes to expect: `IE52` (trip_id not found), `IE41` (invalid trip_id or invalid itinerary
  status), `IE45` (`customer_booking_reference_id` too long), `IE55` (neither `pricing_solution_ids`
  nor `itinerary_id` supplied), `IE38` (itinerary_id not found), `IE69` (pricing_solution_ids not
  found), `IE44` (unauthorized). Full list in `errors/webjet-error-codes.yml`.
- If the call returns `401`, the 90-day access token has expired — run the
  `webjet-tripninja-rotate-access-token` skill, then retry once.
- Escalate to a human before reporting a booking if the `trip_id`/`itinerary_id` pair did not come
  from this session's own `/v3/get-searches/` and `/v3/generate-solutions/` responses.
