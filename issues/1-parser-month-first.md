# Parser reads dates month-first for day-first locales

**Covers 374 reports** in #4471 (the bug the ticket was opened for — but only these, not all 412).

## What happens

The shared date parser reads every date **month first** (`src/lib/kb/causes.ts`: "The shared
parser reads every date month first"). Tenants in day-first locales write `DD/MM/YYYY`, so
`3 October` comes out as `10 March`: month and day flip.

## Evidence

- **374 of the 412 unique reports** in #4471 are this bug.
- **11 tenants**, i.e. every tenant on file — all day-first locales (en-GB, en-IE, nl-NL,
  cs-CZ, it-IT, fr-FR, de-DE, pt-PT, pl-PL, da-DK, es-ES). Includes the booking-v3 tenants
  on their app/email channels; only their `form` reports are the hint bug (issue #2).
- **Every channel**: app (204), email (150), form (25).
- First seen **2026-04-01** (R-41200, harbour.dental: "Our 7 April reminder went out on
  4 July"), last seen **2026-10-04**.
- Bodies describe month/day swaps, never one-day shifts, e.g. R-41206: "4 March turned into
  3 April"; R-41290: "Is there a setting for 'my country writes the day first'."
- This is the "known bug" from #4471's title; the queued fix addresses exactly this.

## Fix direction

Parse per tenant locale (`dayFirst(locale)` already exists in `causes.ts`), or store dates
unambiguous (ISO) at intake and format only for display.

## Not this ticket

- The `booking-v3` hint bug — 33 reports, issue #2. Same symptom, different cause, and the
  parser fix will not touch it.
- 5 one-day moves footnoted on the #4471 page — issue #3. Those are not swaps.
