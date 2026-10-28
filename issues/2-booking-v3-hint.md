# booking-v3 hints MM/DD/YYYY but reads the typed date day-first

**Covers 33 reports** in #4471. A different bug from the parser (issue #1) — the queued
parser fix will not stop these reports.

## What happens

The `booking-v3` form prints a hint of `MM/DD/YYYY` under its date field
(`causes.ts`, `DATE_HINT`). People type what the hint tells them — month first. The form
then reads the typed date the **tenant's** way — day first — so the booking lands with
month and day flipped.

The tenant did nothing wrong and the parser is not involved: the form's own hint and the
form's own reading disagree.

## Evidence

- **33 of the 412 unique reports** in #4471, ids:
  R-41230, R-41241, R-41251, R-41259, R-41262, R-41275, R-41294, R-41305, R-41308,
  R-41366, R-41372, R-41384, R-41387, R-41396, R-41400, R-41406, R-41415, R-41416,
  R-41418, R-41428, R-41439, R-41441, R-41449, R-41453, R-41456, R-41460, R-41472,
  R-41485, R-41493, R-41539, R-41572, R-41582, R-41584.
- **Only 3 tenants**, all `booking-v3`, all day-first locales:
  alder.physio (de-DE), copper.studio (es-ES), pale.harbour (pt-PT).
- **Only channel `form`** — 33 of 33. No app or email report is this bug.
- First seen **2026-04-17**, last seen **2026-09-22** (R-41584, copper.studio: "Every
  booking we make shows the wrong day. 7 November is now 11 July.").

## Fix direction

Make `booking-v3` read what it hints, or hint what it reads. One line in
`DATE_HINT`/parsing, plus a data check for bookings already stored flipped.

## Why it hid in #4471

Intake tagged 15 of these 33 as `parser`, and every subject line reads "wrong day" —
decision 0001 merges on subject alone, so they merged with the parser bug.
