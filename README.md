# #4471 — two problems, not one

Deliverable for the MEGA.dev course challenge **S01E03 "what it shows"**.

The support desk merged 412 reports about wrong dates into one ticket (#4471) and marked it
known: "one bug, one fix queued". Reading every report shows **two distinct problems** —
the bug #4471 was opened for, and a second bug hiding inside it:

| Ticket | Problem | Reports |
|--------|---------|---------|
| [#1](https://github.com/8lueFox/kb-4471-what-it-shows/issues/1) | Shared parser reads dates month-first; day-first locales get month/day flipped (5 one-day reports footnoted under it, counted where they're filed) | **379** |
| [#2](https://github.com/8lueFox/kb-4471-what-it-shows/issues/2) | `booking-v3` form hints `MM/DD/YYYY` but reads the typed date day-first | **33** |

379 + 33 = **412** — every report in #4471 lands in exactly one ticket.

## Contents

This repo is the desk's kb (from the 2026-10-04 export) with my page added, plus the tickets.

- [`src/routes/kb/incidents/4471-what-it-shows/+page.svx`](src/routes/kb/incidents/4471-what-it-shows/+page.svx) —
  **my page**: the kb page about the problems, added where the kb's index finds it
  (`import.meta.glob('/src/routes/kb/*/**/+page.svx')`). Runbook layout, signed frontmatter,
  ends with `<RenderStamp {signature} />`; desk ref derived from the signature:
  **DR-8MR2-K2PE**. `#lib` components are the kb export's own, now in this repo.
- [`desk-view.html`](desk-view.html) — the page as the desk will see it: the runbook
  frame, the page body, the render stamp.
- [`issues/`](issues/) — the bodies of the tickets, one file per problem.

## How the split was computed

From the kb export (`reports-4471.json`, 427 rows → 412 unique by id; 15 export duplicates),
per `src/lib/kb/causes.ts` (`causeOf`: tenant locale × channel × form version):

- **Parser** — 379 reports: the shared parser reads month-first in day-first tenants.
  Includes 5 reports the desk's own notes (`desk-notes.json`) read differently (one-day /
  timezone moves, no swap in the body): footnoted on ticket #1, counted where they're
  filed, the desk's convention.
- **Hint** — 33 reports: `booking-v3` hints `MM/DD/YYYY` but reads day-first. A different
  bug with its own fix; the queued parser fix will not stop these reports.

## Process notes

- Intake tags disagree with the real cause in 38/412 reports (23 tagged `hint` are parser;
  15 tagged `parser` are hint).
- Decision 0001 (merge on subject line alone) is what glued two bugs into #4471.
