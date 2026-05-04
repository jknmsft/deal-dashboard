# Deal Health Dashboard — PRD

## Overview

The Deal Health Dashboard is a lightweight prototype that gives a sales manager a single-page view of all active deals. The goal is to make it instantly obvious which deals are on track and which ones need attention, so the manager can prioritize their day in seconds.

## Features

1. **Deal list view** — Display all deals in a card or table layout, showing deal name, customer, value, expected close date, and owner.
2. **Color-coded health status** — Each deal shows a clear status indicator:
   - 🟢 **Green — Healthy:** on track to close
   - 🟡 **Yellow — At Risk:** needs attention soon
   - 🔴 **Red — Critical:** likely to slip or lose
3. **Summary tiles** — Top of the page shows **total deal count**, total pipeline value, **count of deals by status (Green / Yellow / Red)**, and total at-risk + critical value. Numbers must always match the underlying data.
4. **Sort and filter** — Filter deals by status (all / healthy / at risk / critical) and sort by value or close date.
5. **Highlight needs-attention deals** — At-risk and critical deals appear at the top by default, with a short reason note (e.g. "no activity in 14 days").
6. **Responsive single-page layout** — Works in a desktop browser with no backend; everything lives in `app/index.html`.

## Sample Data

Hardcode 8–10 sample deals directly in the page. Suggested mix:

| Deal | Customer | Value | Close Date | Status | Reason |
|---|---|---:|---|---|---|
| Acme Renewal | Acme Corp | $48,000 | 2026-05-30 | Healthy | Contract sent |
| Globex Expansion | Globex | $120,000 | 2026-06-15 | Healthy | Champion engaged |
| Initech Platform | Initech | $75,000 | 2026-05-20 | At Risk | No activity 10 days |
| Umbrella Pilot | Umbrella Inc | $22,000 | 2026-05-10 | At Risk | Pricing pushback |
| Soylent Migration | Soylent | $200,000 | 2026-06-01 | Critical | Champion left |
| Hooli SaaS | Hooli | $90,000 | 2026-05-25 | Critical | Competitor selected |
| Pied Piper Add-on | Pied Piper | $15,000 | 2026-06-20 | Healthy | Verbal commit |
| Stark Industries | Stark | $310,000 | 2026-07-01 | Healthy | In procurement |

No real customer data — sample names only.

## Interactive Requirements

These requirements extend the prototype into an interactive demo.

### R7: Filter buttons

- Buttons to filter the deal list: **All**, **Healthy** (green), **At Risk** (yellow), **Critical** (red).
- The active filter button is visually highlighted.
- **How to verify:** Click "Critical" — only red deals appear in the table.

### R8: Deal detail panel

- Clicking a deal row reveals additional details for that deal.
- Shows: **risk factors**, **last contact date**, **next steps**, **notes**.
- **How to verify:** Click any deal — the expanded information appears.

### R9: Edit deal information

- The user can modify deal **value**, **stage**, **health status**, and **notes**.
- Changes appear immediately in the table without a page refresh.
- **How to verify:** Change a deal's health from Green to Red — the badge color updates instantly.

### R10: Persistent storage

- Changes survive a page refresh using browser storage (`localStorage`).
- **How to verify:** Edit a deal, refresh the page — the changes remain.

### R11: Reset to defaults

- A button restores the original sample data, clearing any user edits.
- **How to verify:** Make changes, click **Reset** — the original data returns.

### R12: Responsive layout

- Usable on tablet and phone screens; layout adapts gracefully to narrow widths.
- **How to verify:** Narrow the browser window — the layout reflows without horizontal scrolling or overlap.

### R13: Last Updated timestamp

- The dashboard shows when the deal data was last modified by the user (any edit to value, stage, status, or notes).
- The timestamp is human-readable (e.g. "Last updated: May 4, 2026, 1:30 PM").
- After clicking **Reset to defaults**, the timestamp clears (e.g. shows "Original sample data") because no user edits exist.
- The timestamp persists across page refreshes (stored in `localStorage`).
- **How to verify:** Edit any deal — the timestamp updates immediately. Refresh the page — the timestamp still shows. Click **Reset** — the timestamp clears.

### R14: Summary updates with filters

- When a filter is active (Healthy / At Risk / Critical), the summary tiles reflect only the visible (filtered) deals.
- When the **All** filter is active, the summary shows totals for the full data set.
- **How to verify:** Click the **Critical** filter — the total count and breakdown update to show only Critical deals (Green = 0, Yellow = 0, Red = count of critical). Click **All** — the summary returns to full totals.

## Demo Script

A 2–3 minute walkthrough showing the prototype end-to-end:

1. **Open the dashboard.** Point out the title, description, and the table of deals with color-coded health badges (R1, R2, R6).
2. **Scan the summary tiles.** Highlight total pipeline value and the at-risk + critical totals — this is the "what needs attention" snapshot (R3).
3. **Filter to Critical.** Click the **Critical** filter button to show only red deals; note that the active filter is highlighted (R7).
4. **Open a deal.** Click a critical deal row to expand the detail panel — show risk factors, last contact date, next steps, and notes (R8).
5. **Edit the deal.** Update the notes, change the stage, and flip the health from Critical back to At Risk. Watch the badge color update live in the table (R9).
6. **Refresh the page.** The edits persist thanks to browser storage (R10).
7. **Reset the demo.** Click **Reset** to restore the original sample data for the next viewer (R11).
8. **Resize the window.** Narrow the browser to phone width to show the responsive layout (R12).

## Verification Report

Verified on 2026-05-01 against `app/index.html`.

| Requirement | Status | Evidence |
|-------------|--------|----------|
| R1 | PASS | The `defaultDeals` array holds 8 deals and `renderDeals()` emits a row per deal with company, value, close date, stage, and health columns. |
| R2 | PASS | Each row renders a `.badge.healthy` (green), `.badge.at-risk` (yellow), or `.badge.critical` (red) pill driven by the deal's `status` field, with matching CSS variables. |
| R3 | PASS | `renderSummary()` computes total pipeline ($880,000), per-status counts and values, and at-risk + critical total ($387,000), rendered into the four `.tile` elements. |
| R4 | PASS | Filter buttons (All / Healthy / At Risk / Critical) update `activeFilter` and the Sort by dropdown offers value and close-date sort modes that re-run `renderDeals()`. |
| R5 | PASS | Default `activeSort = "attention"` orders Critical → At Risk → Healthy with ties by value desc, and each row's `.reason` text appears under the badge. |
| R6 | PASS | Single-page `app/index.html` with no backend; container caps at 1100px and existing media queries reflow summary tiles and detail panel. |
| R7 | PASS | Filter buttons toggle a `.filter-btn.active` class with a dark background, verified visually on each click handler. |
| R8 | PASS | Clicking a row sets `expandedCompany` and renders a `.detail-panel` row showing risk factors, last contact, next steps, and notes. |
| R9 | PASS | Detail panel exposes editable Value (number), Stage (text), Health (select), and Notes (textarea); `input` and `change` listeners update `deals`, badge, totals, and sort live. |
| R10 | PASS | `loadDeals()` reads `localStorage["dealDashboard.deals.v1"]` on init; `saveDeals()` writes after every edit, so changes survive refresh. |
| R11 | PASS | The `#reset-btn` click handler confirms with the user, removes the storage key, replaces `deals` with `structuredClone(defaultDeals)`, and re-renders. |
| R12 | PASS | Added a `@media (max-width: 480px)` block that stacks the toolbar, collapses the summary to one column, tightens paddings, and lets the table scroll horizontally inside `.card { overflow-x: auto }` so phone widths (~375px) reflow without overlap. |

### Failures and fixes

- **R12 was initially FAIL.** The deals table has 5 columns with 20px cell padding wrapped in a `.card` with `overflow: hidden`, so at ~375px the rightmost columns were clipped and the toolbar's filter row + sort dropdown + reset button overflowed horizontally. **Smallest fix:** added a single `@media (max-width: 480px)` block in `app/index.html` that (1) stacks the toolbar vertically, (2) collapses the summary tiles to one column, (3) reduces container and table-cell padding, (4) sets `.card { overflow-x: auto }` so the table can scroll instead of clipping, and (5) shrinks the header type. Implemented and re-verified PASS.
