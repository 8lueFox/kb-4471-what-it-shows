# Parser reads dates month-first for day-first locales

**Covers 379 reports** in #4471 — the bug the ticket was opened for, including 5 one-day
reports footnoted below.

## What happens

The shared date parser reads every date **month first** (`src/lib/kb/causes.ts`: "The shared
parser reads every date month first"). Tenants in day-first locales write `DD/MM/YYYY`, so
`3 October` comes out as `10 March`: month and day flip.

## Evidence

- **379 of the 412 unique reports** in #4471 are filed under this cause.
- **11 tenants**, i.e. every tenant on file — all day-first locales (en-GB, en-IE, nl-NL,
  cs-CZ, it-IT, fr-FR, de-DE, pt-PT, pl-PL, da-DK, es-ES). Includes the booking-v3 tenants
  on their app/email channels; only their `form` reports are the hint bug (issue #2).
- **Every channel**: app, email, form.
- First seen **2026-04-01** (R-41200, harbour.dental: "Our 7 April reminder went out on
  4 July"), last seen **2026-10-04**.
- Bodies describe month/day swaps, e.g. R-41206: "4 March turned into 3 April"; R-41290:
  "Is there a setting for 'my country writes the day first'."
- This is the "known bug" from #4471's title; the queued fix addresses exactly this.

## Footnote: 5 reports read differently

The desk's own notes (`desk-notes.json`) read 5 of these reports differently — not swaps,
but one-day or timezone shifts, footnoted here because the desk counts reports where
they're filed:

| Report | Tenant | Channel | What the reporter said |
|--------|--------|---------|------------------------|
| R-41202 | kettle.books | email | "Invite says Tuesday 12 May. Calendar says Monday 11 May. My client turned up on the Monday." |
| R-41209 | alder.physio | app | "Every booking in June is one day off. Only June. Why only June." |
| R-41236 | nine.lanterns | app | "The invite arrived before I made the booking. I would like to know how." |
| R-41344 | pale.harbour | email | "Client in Lisbon got Tuesday. Client in Toronto got Monday. Same meeting. We held it twice." |
| R-41620 | lantern.row | email | "Tuesday 6 October showed as Monday. I moved it myself. I have stopped reporting these but…" |

If the parser fix ships and these keep arriving, they are a separate timezone problem —
split them out then, with their own page.

## Fix direction

Parse per tenant locale (`dayFirst(locale)` already exists in `causes.ts`), or store dates
unambiguous (ISO) at intake and format only for display.

## Not this ticket

The `booking-v3` hint bug — 33 reports, issue #2. Same symptom, different cause, and the
parser fix will not touch it.
