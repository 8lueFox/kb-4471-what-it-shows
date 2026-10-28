# #4471 — three problems, not one

Deliverable for the MEGA.dev course challenge **S01E03 "what it shows"**.

The support desk merged 412 reports about wrong dates into one ticket (#4471) and marked it
known: "one bug, one fix queued". Reading every report shows **three distinct problems**:

| Ticket | Problem | Reports |
|--------|---------|---------|
| [#1](https://github.com/8lueFox/kb-4471-what-it-shows/issues/1) | Shared parser reads dates month-first; day-first locales get month/day flipped | **374** |
| [#2](https://github.com/8lueFox/kb-4471-what-it-shows/issues/2) | `booking-v3` form hints `MM/DD/YYYY` but reads the typed date day-first | **33** |
| [#3](https://github.com/8lueFox/kb-4471-what-it-shows/issues/3) | One-day moves (off-by-one / timezone), not swaps | **5** |

374 + 33 + 5 = **412** — every report in #4471 lands in exactly one ticket.

## Contents

- [`issues/`](issues/) — the bodies of the three tickets, one file per problem.
- [`src/routes/kb/incidents/4471-what-it-shows/+page.svx`](src/routes/kb/incidents/4471-what-it-shows/+page.svx) —
  the kb page about the three problems, written in the support desk's kb conventions
  (`.svx`, runbook layout, signed frontmatter, ends with `<RenderStamp {signature} />`;
  desk ref derived from the signature: **DR-8MR2-K2PE**). The desk can drop it into their
  kb's `src/routes/kb/incidents/` as-is; `#lib` components come from the kb export.
- [`desk-view.html`](desk-view.html) — the page as the desk will see it: the runbook
  frame, the page body, the render stamp.

## How the split was computed

From the kb export (`reports-4471.json`, 427 rows → 412 unique by id; 15 export duplicates):

- **Parser vs hint** per `src/lib/kb/causes.ts` (`causeOf`: tenant locale × channel × form version).
- **One-day moves** by reading the bodies of the reports the desk's own notes
  (`desk-notes.json`) read differently: no month/day swap, a shift of one day or a
  cross-timezone weekday mismatch. The desk's #4471 page counts these 5 under the parser
  with a footnote; here they are their own ticket.

## Process notes

- Intake tags disagree with the real cause in 38/412 reports (23 tagged `hint` are parser;
  15 tagged `parser` are hint).
- Decision 0001 (merge on subject line alone) is what glued three bugs into #4471.
