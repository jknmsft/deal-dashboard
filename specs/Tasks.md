# Deal Health Dashboard — Tasks

Requirements reference (from `specs/PRD.md`):
- R1: Deal list view
- R2: Color-coded health status (green / yellow / red)
- R3: Summary tiles (total count, pipeline value, counts by status, at-risk value)
- R4: Sort and filter by status / value / close date
- R5: Highlight needs-attention deals at top with reason
- R6: Responsive single-page layout in `app/index.html`
- R7: Filter buttons with active state highlight
- R8: Deal detail panel (risk factors, last contact, next steps, notes)
- R9: Edit deal info (value, stage, health, notes) with live updates
- R10: Persistent storage via `localStorage`
- R11: Reset to default sample data
- R12: Responsive layout for tablet and phone
- R13: Last Updated timestamp showing when data was last modified
- R14: Summary updates to reflect the active filter

## Tasks

- [x] Task 1: Scaffold `app/index.html` with page title, header, and an empty main container styled with basic CSS
    Satisfies: R6
    Done when: Opening `app/index.html` in a browser shows the "Deal Health Dashboard" title and an empty content area on desktop width.

- [x] Task 2: Hardcode the 8 sample deals from the PRD into a JavaScript array (name, customer, value, closeDate, status, reason)
    Satisfies: R1
    Done when: A `deals` array with 8 objects is defined in the page and logs correctly to the browser console.

- [x] Task 3: Render the deals as a list/table showing name, customer, value, close date, and owner/reason
    Satisfies: R1, R6
    Done when: All 8 sample deals appear on the page with their fields visible and readable.

- [x] Task 4: Add a colored status badge to each deal (green = Healthy, yellow = At Risk, red = Critical)
    Satisfies: R2
    Done when: Every deal row shows a clearly colored badge matching its status, verifiable by visual inspection.

- [x] Task 5: Add summary tiles at the top showing total pipeline value, count per status, and combined at-risk + critical value
    Satisfies: R3
    Done when: Tiles display correct totals computed from the `deals` array (verify by manual sum of sample data).

- [x] Task 6: Add filter buttons (All / Healthy / At Risk / Critical) with the active button visually highlighted, plus sort controls (by value, by close date)
    Satisfies: R4, R7
    Done when: Clicking "Critical" shows only red deals and the active button is highlighted; switching sort reorders the visible list accordingly.

- [x] Task 7: Sort at-risk and critical deals to the top by default and show their reason note inline
    Satisfies: R5
    Done when: On page load, red and yellow deals appear above green ones, each with its reason text visible next to the badge.

- [x] Task 8: Extend each deal object with `riskFactors`, `lastContact`, `nextSteps`, and `notes`; clicking a row expands a detail panel showing these fields
    Satisfies: R8
    Done when: Clicking any deal row reveals an expanded panel with risk factors, last contact date, next steps, and notes.

- [x] Task 9: Make the detail panel editable for value, stage, health status, and notes; changes update the table row and badge color immediately
    Satisfies: R9
    Done when: Changing a deal's health from Green to Red in the panel updates the table badge color instantly without a page refresh.

- [x] Task 10: Persist the deals array to `localStorage` on every change and rehydrate from it on page load
    Satisfies: R10
    Done when: Editing a deal and refreshing the page shows the edits still applied.

- [x] Task 11: Add a "Reset to defaults" button that clears `localStorage` and restores the original hardcoded sample data
    Satisfies: R11
    Done when: After making edits, clicking Reset returns the table to the original 8 sample deals.

- [x] Task 12: Add responsive CSS (media queries / flexible layout) so the dashboard works on tablet and phone widths
    Satisfies: R6, R12
    Done when: Narrowing the browser to phone width (~375px) shows the layout reflow without horizontal scrolling or overlapping elements.

- [x] Task 13: Show a human-readable "Last updated" timestamp that updates on every edit, persists across refresh, and clears on Reset
    Satisfies: R13
    Done when: Editing a deal updates the displayed timestamp immediately; refreshing the page preserves it; clicking Reset clears it back to "Original sample data".

- [x] Task 14: Extend the summary tiles to display a clear **total deal count** alongside the existing per-status counts and pipeline values, and make the summary reflect the **active filter** (filtered totals when a status filter is applied; full totals when "All" is active)
    Satisfies: R3 (extended), R14
    Done when: A total-count value is visible in the summary section; clicking the Critical filter updates the summary to show only critical deals (e.g. Total = 2, Green 0 / Yellow 0 / Red 2); clicking All restores full totals; flipping a deal's health updates the affected counts immediately.

## Verification

Full R1–R12 verification completed on 2026-05-01. See the **Verification Report** section at the bottom of `specs/PRD.md`. All 12 requirements PASS. R12 was initially failing and was fixed in the same pass (see Task 12 progress log below).

## Progress Log

### Task 4 — Colored status badges (Satisfies R2)

**What changed:** Verified that `app/index.html` already includes pill-style status badges with a colored dot and matching background/text color for each health status. CSS classes `.badge.healthy` (green), `.badge.at-risk` (yellow), and `.badge.critical` (red) are defined in the `<style>` block, and the rendering loop emits the correct class per deal based on its `status` field. No code changes were required to satisfy this task.

**How to test in your browser:**
1. Open `app/index.html` in a browser (or via the local server you used before).
2. In the **Health** column of the table, confirm each row shows a rounded pill badge with a small colored dot and a label.
3. Verify the colors:
   - **Acme Corp**, **Globex**, **Pied Piper**, **Stark Industries** → **green "Healthy"** badge
   - **Initech**, **Umbrella Inc** → **yellow "At Risk"** badge
   - **Soylent**, **Hooli** → **red "Critical"** badge
4. The badge color should be visually distinct at a glance without reading the label.

### Task 5 — Summary tiles (Satisfies R3)

**What changed:** Added a 4-tile summary row above the deals table in `app/index.html`. New CSS styles `.summary`, `.tile`, and per-status accent colors. New JavaScript computes `totalPipeline`, per-status counts, per-status values, and `needsAttention` (at-risk + critical value), then renders the tiles.

**Tiles displayed:**
- **Total Pipeline:** $880,000 (8 active deals)
- **Healthy:** 4 deals ($493,000)
- **At Risk:** 2 deals ($97,000)
- **Needs Attention:** $387,000 (4 deals — At Risk + Critical)

**How to test in your browser:**
1. Open `app/index.html` (refresh if already open).
2. Confirm four tiles appear above the deals table.
3. Verify the **Total Pipeline** tile reads **$880,000** with **8 active deals**.
4. Verify the **Healthy** tile shows **4** with **$493,000** in green.
5. Verify the **At Risk** tile shows **2** with **$97,000** in yellow.
6. Verify the **Needs Attention** tile shows **$387,000** in red with **4 deals** subtext.
7. Resize the browser narrower than ~700px to confirm tiles reflow into a 2-column grid.

### Task 6 — Filter buttons and sort controls (Satisfies R4, R7)

**What changed:** Added a toolbar above the deals table in `app/index.html` containing:
- A pill-style **filter button group**: All, Healthy, At Risk, Critical (each colored-status button shows a matching dot). The active button is highlighted with a dark background.
- A **Sort by** dropdown with four options: Value high→low (default), Value low→high, Close date soonest, Close date latest.

New CSS for `.toolbar`, `.filters`, `.filter-btn`, `.sort-control`, and an `.empty-row` style for when no deals match. The render logic was refactored into a `renderDeals()` function driven by `activeFilter` and `activeSort` state, and click/change event handlers update the table live.

**How to test in your browser:**
1. Open (or refresh) `app/index.html`.
2. Confirm a toolbar appears between the summary tiles and the table, with four filter buttons on the left and a Sort by dropdown on the right.
3. Click **Critical** — only **Soylent** and **Hooli** should remain in the table, and the **Critical** button should appear highlighted (dark background).
4. Click **At Risk** — only **Initech** and **Umbrella Inc** should appear; the highlight moves to **At Risk**.
5. Click **Healthy** — only the four green deals appear.
6. Click **All** — all 8 deals return.
7. Change the Sort dropdown to **Close date (soonest)** — the topmost row should now be **Umbrella Inc** (2026-05-10).
8. Switch to **Value (low to high)** — topmost row should now be **Pied Piper** ($15,000).

### Task 7 — Default attention-first sort with inline reason (Satisfies R5)

**What changed:** In `app/index.html`:
- Added a new default sort mode `"attention"` that orders rows Critical → At Risk → Healthy (ties broken by value, high to low). Set as the initial `activeSort` value.
- Added a new **"Needs attention first"** option at the top of the Sort by dropdown.
- Added a `.health-cell` flex layout and `.reason` style (small, muted, italic) so the reason note renders directly under the colored badge in the Health column.

**How to test in your browser:**
1. Refresh `app/index.html`.
2. Confirm the Sort by dropdown defaults to **"Needs attention first"**.
3. Confirm the table row order from top to bottom is:
   1. **Soylent** — Critical — "Champion left"
   2. **Hooli** — Critical — "Competitor selected"
   3. **Initech** — At Risk — "No activity 10 days"
   4. **Umbrella Inc** — At Risk — "Pricing pushback"
   5. **Stark Industries** — Healthy
   6. **Globex** — Healthy
   7. **Acme Corp** — Healthy
   8. **Pied Piper** — Healthy
4. In the Health column, confirm a small italic reason note appears underneath each badge (for example, "Champion left" under Soylent's red badge).
5. Switch the Sort dropdown to **Value (high to low)** and back to **Needs attention first** to confirm both modes work.

### Task 8 — Deal detail panel (Satisfies R8)

**What changed:** In `app/index.html`:
- Extended each of the 8 deals with `riskFactors` (array), `lastContact` (ISO date), `nextSteps` (string), and `notes` (string) — all realistic per-deal content.
- Added a chevron `›` indicator in the company column that rotates 90° when a row is open.
- Added a click handler on the table body so clicking any deal row toggles an expanded detail row beneath it (only one open at a time). Clicking the same row again collapses it.
- Added a `.detail-panel` two-column grid layout showing Risk Factors (bulleted list), Last Contact, Next Steps (full width), and Notes (full width). Collapses to one column under 700px.

**How to test in your browser:**
1. Refresh `app/index.html`.
2. In the Company column, confirm a small `›` chevron appears before each company name.
3. Click the **Soylent** row — a panel should expand below showing:
   - Risk Factors: "Champion left the company", "New stakeholder unknown", "No internal advocate"
   - Last Contact: Apr 15, 2026
   - Next Steps: "Identify new champion via LinkedIn outreach"
   - Notes: "Was our strongest Q2 deal. Need to rebuild relationship from scratch."
4. Confirm Soylent's chevron now points down (rotated).
5. Click another row (e.g., **Hooli**) — Soylent collapses and Hooli's panel opens with its own risk factors, contact date, next steps, and notes.
6. Click **Hooli** again — the panel collapses and the chevron rotates back.

### Task 9 — Editable detail panel with live updates (Satisfies R9)

**What changed:** In `app/index.html`:
- The detail panel's **Value**, **Stage**, **Health**, and **Notes** fields are now real form controls (number input, text input, select dropdown, textarea) instead of static text.
- Refactored summary-tile rendering into a reusable `renderSummary()` function so totals refresh after edits.
- Wired two listeners: `input` for stage/notes (inline, focus-preserving updates) and `change` for value/status (triggers full re-render so badge color, totals, and sort all refresh).
- Clicks inside the detail panel no longer collapse the row.

**How to test in your browser:**
1. Refresh `app/index.html`.
2. Click **Acme Corp** to expand it. You should see editable fields for Value, Stage, Health (dropdown), and Notes (textarea).
3. **Health change:** Change Acme Corp's Health dropdown from **Healthy** to **Critical**. Confirm:
   - The badge in the table row turns **red** with label "Critical".
   - The summary tiles update: Healthy count drops by 1, Critical count rises by 1, Needs Attention value increases by $48,000.
   - Acme Corp jumps to the top of the table (Critical sorts first).
4. **Value change:** Click Acme Corp again to re-expand it, change Value from `48000` to `90000`, then click outside the input. Confirm the row's value column shows **$90,000** and the Total Pipeline tile updated.
5. **Stage change:** Type into the Stage field (e.g., change "Contract Sent" to "Closed Won"). Confirm the table's Stage column updates live as you type, and the input keeps focus.
6. **Notes change:** Type into the Notes textarea. Confirm you can keep typing without losing focus.
7. **Refresh the page** — changes will revert (persistence comes in Task 10).

### Task 10 — Persistent storage via localStorage (Satisfies R10)

**What changed:** In `app/index.html`:
- Renamed the original hardcoded array to `defaultDeals` and added a `STORAGE_KEY` constant (`"dealDashboard.deals.v1"`).
- Added `loadDeals()` that reads and parses `localStorage` on page load, falling back to a deep clone of `defaultDeals` if missing or invalid.
- Added `saveDeals()` that serializes the current `deals` array back to `localStorage`.
- Both edit handlers (`input` for stage/notes, `change` for value/status) now call `saveDeals()` after applying the change.

**How to test in your browser:**
1. Refresh `app/index.html` to start clean.
2. Click **Acme Corp** to expand it.
3. Change Health from **Healthy** to **Critical** and edit Notes to something memorable (e.g., "PERSISTED TEST").
4. **Refresh the page (F5)**.
5. Confirm:
   - Acme Corp still shows a **red Critical** badge and is sorted near the top.
   - The summary tiles still reflect the new status counts.
   - Re-expand Acme Corp — the Notes field still contains "PERSISTED TEST".
6. (Optional) Open DevTools → Application → Local Storage → your origin, and confirm a `dealDashboard.deals.v1` key holds the JSON of your edited deals.
7. To reset manually for now: in DevTools console run `localStorage.removeItem("dealDashboard.deals.v1")` and refresh — the original 8 deals return. (A Reset button comes in Task 11.)

### Task 11 — Reset to defaults button (Satisfies R11)

**What changed:** In `app/index.html`:
- Added a **Reset to defaults** button in the toolbar, next to the Sort by dropdown (grouped under a new `.toolbar-right` container).
- Added `.reset-btn` styles with a subtle hover state that turns the button red, signaling it's a destructive action.
- Added a click handler that:
  1. Shows a confirmation dialog (`Reset all deals to the original sample data? Your edits will be lost.`).
  2. If confirmed, clears the `localStorage` key, replaces the in-memory `deals` array with a deep clone of `defaultDeals`, closes any open detail panel, and re-renders the summary and table.

**How to test in your browser:**
1. Refresh `app/index.html`.
2. Confirm a **Reset to defaults** button appears on the right side of the toolbar (after the Sort by dropdown).
3. Make a few edits: change **Acme Corp** Health to Critical, change **Stark Industries** value to 999999, and edit notes on any deal.
4. Refresh the page — confirm the edits persisted.
5. Click **Reset to defaults**, then click **OK** in the confirmation dialog.
6. Confirm:
   - The table returns to the original 8 deals (Acme Corp green again, Stark Industries back to $310,000).
   - The summary tiles show the original totals ($880,000 pipeline, 4 healthy, 2 at risk, $387,000 needs attention).
   - Refreshing the page keeps the defaults (because `localStorage` was cleared).
7. Click **Reset to defaults** and then **Cancel** — nothing should change.

### Task 12 — Responsive layout for tablet and phone (Satisfies R6, R12)

**What changed:** In `app/index.html`, added a `@media (max-width: 480px)` block at the end of the `<style>` section that:
- Reduces container padding from 40px/24px to 20px/12px and shrinks the header heading and intro text.
- Collapses the summary tiles to a single column.
- Stacks the toolbar vertically (filter buttons row above sort + reset row) so they no longer overflow.
- Allows the filter button row to scroll horizontally if needed and lets the toolbar-right group wrap.
- Sets `.card { overflow-x: auto }` so the deals table can scroll sideways instead of being clipped by the card's `overflow: hidden`.
- Tightens table cell padding (12px) and reduces table font-size to 13px.
- Tightens detail-panel padding for narrow widths.

**How to test in your browser:**
1. Refresh `app/index.html`.
2. Open DevTools, toggle the device toolbar, and pick **iPhone SE (375 × 667)** (or just narrow the browser window to ~375px wide).
3. Confirm:
   - The header, summary tiles (now stacked one per row), toolbar, and table all fit within the screen with no horizontal page scroll.
   - The toolbar shows filter buttons on one row and the Sort by dropdown + Reset button on a row below.
   - The deals table itself can be scrolled left/right inside its card if its content is wider than the screen — no columns are clipped or overlapping.
4. Tap (or click) **Soylent** to expand its detail panel and confirm Risk Factors, Last Contact, Next Steps, and Notes all stack in a single column with comfortable padding.
5. Resize back to desktop width and confirm the original two-row toolbar and 4-column summary tiles return.

### Task 13 — Last Updated timestamp (Satisfies R13)

**What changed:** In `app/index.html`:
- Added a new `TIMESTAMP_KEY = "dealDashboard.lastUpdated.v1"` `localStorage` key, separate from the deals payload so the timestamp can be cleared independently on Reset.
- `saveDeals()` now writes `new Date().toISOString()` to that key after every edit and calls a new `renderLastUpdated()` function.
- Added an `Intl.DateTimeFormat` formatter (`en-US`, e.g. "May 4, 2026, 1:30 PM") and a `renderLastUpdated()` function that reads the stored ISO string, defensively handles missing/corrupted values, and writes the result into a new `#last-updated` element.
- Added a small muted `.last-updated` element below the toolbar (`aria-live="polite"` so assistive tech announces updates).
- The Reset button handler now also removes `TIMESTAMP_KEY` and re-renders the timestamp, which falls back to **"Original sample data"**.
- Initial page load calls `renderLastUpdated()` once after `renderDeals()`.

**How to test in your browser:**
1. Open or refresh `app/index.html`.
2. Confirm a small line of muted text appears just below the toolbar reading **"Original sample data"** (no edits yet).
3. Expand any deal (e.g. **Acme Corp**) and edit its Notes field. As you type, the line should update to **"Last updated: <today's date>, <time>"**.
4. Refresh the page (F5). Confirm the timestamp line still shows your last edit time (it survives refresh because it's stored in `localStorage`).
5. Edit another deal's Health from a dropdown. Confirm the timestamp updates again to the new moment.
6. Click **Reset to defaults** and confirm. The line should immediately revert to **"Original sample data"**.
7. (Optional) In DevTools → Application → Local Storage, confirm the `dealDashboard.lastUpdated.v1` key holds an ISO 8601 string when edits exist, and is removed after Reset.

### Task 14 — Filter-aware summary tiles with total count (Satisfies R3 extended, R14)

**What changed:** In `app/index.html`:
- Refactored `totalPipeline`, `countBy`, `valueBy`, and `needsAttentionTotal` to accept a `data` argument so they summarize whatever subset is passed in.
- Added `getFilteredDeals()` which returns the active-filter subset of `deals` (used by both `renderSummary()` and `renderDeals()`).
- `renderSummary()` now calls `getFilteredDeals()` and prints a scope label ("all deals" or "filtered: <Status>") in the first tile's sub-line.
- Reworked the first tile to show **Total Deals** (count) as the headline metric with pipeline value as the sub-line — makes the count obvious per R3/R14.
- Renamed the fourth tile from "Needs Attention" to **Critical** (count metric, with at-risk + critical pipeline value as sub-line) so all three status tiles match the breakdown specified in R3.
- The filter button click handler now calls `renderSummary()` in addition to `renderDeals()` so tiles re-render whenever the filter changes.
- Moved the initial `renderSummary()` call below the `let activeFilter = "all"` declaration to avoid a Temporal Dead Zone reference error (the function now reads `activeFilter`).

**How to test in your browser:**
1. Hard-refresh `app/index.html` (Ctrl+F5).
2. Confirm the first summary tile reads **Total Deals = 8** with **"$880,000 · all deals"** below it.
3. Confirm the other three tiles show **Healthy = 4**, **At Risk = 2**, **Critical = 2** (matching the sample data).
4. Click the **Critical** filter button. The first tile should update to **Total Deals = 2** with **"$290,000 · filtered: Critical"**, the Healthy and At Risk tiles should drop to 0, and Critical should stay at 2.
5. Click **At Risk**. The first tile should update to **Total Deals = 2** with **"$97,000 · filtered: At Risk"**, At Risk = 2, others = 0.
6. Click **All**. All four tiles should return to the full-data totals from step 2/3.
7. Open any deal and change its Health from Critical to Healthy via the dropdown. Confirm the relevant tile counts update immediately.
