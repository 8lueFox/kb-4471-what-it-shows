# One-day moves: off-by-one / timezone, not swaps

**Covers 5 reports** in #4471. The #4471 page counts these under the parser with a footnote
("counted where they're filed") — but not one of them describes a month/day swap. A
different problem: the date moves by **one day**, or the same meeting lands on different
weekdays in different time zones.

## The 5 reports

| Report | Tenant | Channel | What the reporter said |
|--------|--------|---------|------------------------|
| R-41202 | kettle.books | email | "Invite says Tuesday 12 May. Calendar says Monday 11 May. My client turned up on the Monday." |
| R-41209 | alder.physio | app | "Every booking in June is one day off. Only June. Why only June." |
| R-41236 | nine.lanterns | app | "The invite arrived before I made the booking. I would like to know how." |
| R-41344 | pale.harbour | email | "Client in Lisbon got Tuesday. Client in Toronto got Monday. Same meeting. We held it twice." |
| R-41620 | lantern.row | email | "Tuesday 6 October showed as Monday. I moved it myself. I have stopped reporting these but…" |

All five are also in the desk's own `desk-notes.json` as read differently from the parser
cause — the desk knew, and footnoted them into #4471 anyway.

## Evidence this is not the parser bug

- No report mentions a month/day flip; the deltas are one day, or a timezone boundary
  (Lisbon vs Toronto), or an ordering anomaly (invite before the booking).
- Channels are email and app — the hint bug (#2) never appears outside `form`, and these
  are not form reports.

## Fix direction

Timezone handling in invite generation: UTC midnight boundaries, DST transitions ("only
June"), and per-recipient timezone rendering for shared meetings. Needs a fix of its own;
the queued parser fix will not move these dates.

## Count note

With this split, #4471 = 374 (parser, issue #1) + 33 (hint, issue #2) + 5 (this issue) = 412
reports, each in exactly one ticket.
