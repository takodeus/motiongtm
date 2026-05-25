# Plan: De-grid Calendar + reshape THE 30 in `public/GTM.html`

Scope is a static HTML file — all edits stay in `public/GTM.html` (no React/routes touched).

## 1. "The Calendar" section (`#matrix`) — less grid, more editorial

Today: rigid 4-column matrix (Event / 2024 / 2025 / 2026) with swatch + label per cell, separated by category header rows.

Change to a row-per-event layout that reads as an editorial timeline:

- Keep the category labels (`Industry · Technology`, `Multifamily`, etc.) as section dividers — tighter, no surrounding grid lines.
- Each event becomes a single horizontal block:
  - Left: event name (larger, distinct type weight).
  - Right: three year "pills" laid out inline (2024 · 2025 · 2026), each pill showing the swatch dot + short label + city when present. Absent years render as a muted em-dash pill so the cadence is still readable.
- Remove the column header row and most vertical/horizontal cell borders. Use a thin baseline divider between events only.
- Keep the existing legend; restyle to sit inline above the list.
- Preserve all current data, colors (teal/dim variants via the existing `--ch`, `sw detailed/hosted/sponsored/attended/absent` tokens), and the swatch component — only the container layout changes.

New CSS classes added alongside existing ones (old `.mx-*` rules can stay or be removed once unused):
`.cal-list`, `.cal-cat`, `.cal-item`, `.cal-item__name`, `.cal-years`, `.cal-year`, `.cal-year--absent`, `.cal-year__loc`.

Responsive: on narrow widths, year pills wrap to a second line under the event name instead of forcing a 4-column squeeze.

## 2. "THE 30" section (`#the30`) — remove the calendar view

Today: `.cal-grid` renders a time-slot × 2-room calendar table (`9:30 AM … 5:30 PM` rows × `Room A / Room B` columns) — visually a calendar grid.

Replace with a meeting list grouped by day, no time grid:

- Heading stays ("Realcomm 2024 · meeting room calendar, Day 1 (June 20)") but reframed as "Realcomm 2024 · Day 1 meetings (June 20)".
- Render meetings as a vertical list of cards/rows. Each row:
  - Small time chip (e.g. `10:30 AM`) on the left.
  - Company name (bold) + contact line under it.
  - Optional room tag (Room A / Room B) as a subtle right-aligned label, since we lose the column encoding.
- Drop all "empty / —" slots — list only filled meetings. The density story is now carried by the count + the `.cal-stats` block at the top, not by empty grid cells.
- Keep the `.cal-note` footnote as-is.
- Keep `.cal-stats` summary cards (Meetings booked / Named C-level / Pre-event sessions) unchanged.

New CSS: `.mtg-list`, `.mtg-row`, `.mtg-time`, `.mtg-co`, `.mtg-contact`, `.mtg-room`. Remove or stop using `.cal-grid`, `.cal-row`, `.cal-time`, `.cal-slot` for this section (CSS can remain dormant or be deleted).

## 3. Nav + copy

- `Calendar` nav link (`#matrix`) stays.
- Adjust the THE 30 intro sentence "The 2024 calendar appears below to illustrate the density." → "The 2024 meeting log appears below to illustrate the density." so the copy matches the new presentation.

## Out of scope

- No changes to The 200 accordions, YoY bars, Relationships, or Closing.
- No JS changes (existing accordion script untouched).
- No new dependencies; pure HTML/CSS edits inside `public/GTM.html`.
