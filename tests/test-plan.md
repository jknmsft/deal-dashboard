# Deal Health Dashboard — Manual Test Plan

Manual checklist mapped to every requirement (R1–R14) in [specs/PRD.md](../specs/PRD.md). Open [app/index.html](../app/index.html) in a browser to execute. Each test follows Arrange / Act / Assert thinking expressed as Steps + Expected.

## Coverage Map

| Req | Tests |
|-----|-------|
| R1  | T1, T2, T26 |
| R2  | T3, T4 |
| R3  | T5, T6, T7, T31, EC3 |
| R4  | T8, T9, T10, T11 |
| R5  | T8, T12, T13 |
| R6  | T14, T15, EC5 |
| R7  | T16, T17, T27, EC1 |
| R8  | T18, T19 |
| R9  | T20, T21, T22, T28, EC2, EC3, EC4, EC5 |
| R10 | T23, T29, EC6 |
| R11 | T24, T25, T26, EC7 |
| R12 | T15 |
| R13 | T30 |
| R14 | T31, T32, T33, T34 |

Total: **34 tests**.

---

## Visual

- [ ] **T1** — Page renders header and deal table (covers R1)
  - Steps:
    1. Open `app/index.html` in a desktop browser.
    2. Observe the header and the deals table.
  - Expected: Title "Deal Health Dashboard", description paragraph, and a table listing 8 sample deals with columns Company, Value, Expected Close, Stage, Health.
  - Actual:

- [ ] **T2** — All 8 sample deals appear with required columns (covers R1)
  - Steps:
    1. Load the page fresh (clear `localStorage` if needed).
    2. Count rows in the deals table and inspect each row.
  - Expected: 8 rows, one per sample deal (Acme Renewal, Globex, Initech, Umbrella, Soylent, Hooli, Pied Piper, Stark). Each row shows non-empty company, formatted currency value, ISO close date, stage text, and a colored health badge.
  - Actual:

- [ ] **T3** — Health badge colors match status (covers R2)
  - Steps:
    1. Inspect the Health column for each of the 8 default deals.
  - Expected: Healthy deals show a green badge, At Risk show yellow, Critical show red. Colors are visually distinct.
  - Actual:

- [ ] **T4** — Each badge has correct label and dot (covers R2)
  - Steps:
    1. Hover/inspect each badge.
  - Expected: Badge text reads "Healthy", "At Risk", or "Critical" matching the row's status; the small colored dot matches the badge color.
  - Actual:

- [ ] **T14** — Layout fits within 1100px container on desktop (covers R6)
  - Steps:
    1. Open page in a 1280×800 desktop browser window.
  - Expected: Content is centered, max width ~1100px, no horizontal scrollbar, no overlapping elements.
  - Actual:

- [ ] **T15** — Responsive layout reflows on phone width (covers R6, R12)
  - Steps:
    1. Open DevTools and resize the viewport to 375px wide (iPhone SE).
    2. Scroll the page from top to bottom.
  - Expected: Toolbar stacks vertically, summary tiles collapse to one column, no horizontal page scroll, the table itself may scroll horizontally inside its card without clipping content.
  - Actual:

## Functional — Summary Tiles

- [ ] **T5** — Total pipeline value is correct (covers R3)
  - Steps:
    1. Reset to defaults.
    2. Read the first summary tile (Total Deals tile, sub-line shows pipeline value).
  - Expected: Sub-line reads "$880,000 · all deals".
  - Actual:

- [ ] **T6** — Status counts are correct (covers R3)
  - Steps:
    1. Reset to defaults.
    2. Inspect the per-status count tiles/text.
  - Expected: Healthy = 4, At Risk = 2, Critical = 2.
  - Actual:

- [ ] **T7** — Critical tile sub-line shows at-risk + critical pipeline value (covers R3)
  - Steps:
    1. Reset to defaults.
    2. Read the Critical tile sub-line.
  - Expected: $387,000 (75,000 + 22,000 + 200,000 + 90,000) labelled as "at-risk + critical".
  - Actual:

## Functional — Filtering

- [ ] **T16** — All filter shows every deal (covers R7)
  - Steps:
    1. Click the **All** filter button.
  - Expected: 8 rows visible. The All button is visually highlighted as active; the other three are not.
  - Actual:

- [ ] **T17** — Critical filter shows only red deals (covers R7)
  - Steps:
    1. Click the **Critical** filter button.
  - Expected: Only Soylent Migration and Hooli SaaS rows visible. Critical button is highlighted; All is no longer highlighted.
  - Actual:

- [ ] **T27** — Healthy and At Risk filters work (covers R7)
  - Steps:
    1. Click **Healthy**, observe rows.
    2. Click **At Risk**, observe rows.
  - Expected: Healthy shows the 4 green deals only; At Risk shows the 2 yellow deals only. Active highlight follows the click.
  - Actual:

## Functional — Sorting

- [ ] **T8** — Default sort is "Needs attention first" (covers R4, R5)
  - Steps:
    1. Reset to defaults and reload the page.
    2. Read the order of rows in the table.
  - Expected: Critical deals appear first (Soylent, Hooli), then At Risk (Initech, Umbrella), then Healthy. Within a group, higher value comes first.
  - Actual:

- [ ] **T9** — Sort by Value (high to low) (covers R4)
  - Steps:
    1. Choose "Value (high to low)" from the Sort dropdown.
  - Expected: Stark ($310k) is row 1, then Soylent ($200k), Globex ($120k), Hooli ($90k), Initech ($75k), Acme ($48k), Umbrella ($22k), Pied Piper ($15k).
  - Actual:

- [ ] **T10** — Sort by Value (low to high) (covers R4)
  - Steps:
    1. Choose "Value (low to high)".
  - Expected: Pied Piper ($15k) first; Stark ($310k) last.
  - Actual:

- [ ] **T11** — Sort by Close date asc and desc (covers R4)
  - Steps:
    1. Choose "Close date (soonest)"; observe order.
    2. Choose "Close date (latest)"; observe order.
  - Expected: Soonest puts 2026-05-10 (Umbrella) first and 2026-07-01 (Stark) last. Latest reverses that order.
  - Actual:

## Data — Highlight Needs Attention

- [ ] **T12** — Reason text shows under each badge (covers R5)
  - Steps:
    1. Reset to defaults.
    2. Inspect each row's Health cell.
  - Expected: A short reason note appears with the badge (e.g. "No activity 10 days" for Initech, "Champion left" for Soylent). Every default deal has a non-empty reason.
  - Actual:

- [ ] **T13** — Critical and At Risk float to top by default (covers R5)
  - Steps:
    1. Reset to defaults; default sort = "attention".
  - Expected: Top two rows are Critical, next two are At Risk, remaining four are Healthy.
  - Actual:

## Interaction — Detail Panel

- [ ] **T18** — Click a row to expand detail panel (covers R8)
  - Steps:
    1. Click the Acme Renewal row.
  - Expected: An expanded detail panel appears (typically as an extra row beneath) showing risk factors, last contact date, next steps, and notes for Acme.
  - Actual:

- [ ] **T19** — Click another row collapses the previous (covers R8)
  - Steps:
    1. Click Acme to expand.
    2. Click Globex to expand.
  - Expected: Acme panel collapses, Globex panel is shown. Only one detail panel is visible at a time. Clicking Globex again should collapse it.
  - Actual:

## Interaction — Editing

- [ ] **T20** — Edit health changes badge instantly (covers R9)
  - Steps:
    1. Expand Soylent Migration (Critical).
    2. Change Health select from Critical to Healthy.
  - Expected: Soylent row's badge turns green and label "Healthy" without page reload. Sort order updates if the active sort is "attention".
  - Actual:

- [ ] **T21** — Edit value updates row and totals (covers R9)
  - Steps:
    1. Expand Acme Renewal.
    2. Change Value from 48000 to 60000.
  - Expected: Row shows the new value formatted as currency. Total Pipeline tile increases by $12,000 (to $892,000) immediately.
  - Actual:

- [ ] **T22** — Edit stage and notes persists in the panel (covers R9)
  - Steps:
    1. Expand Globex Expansion.
    2. Change Stage to "Negotiation" and edit Notes to "Legal review".
  - Expected: Stage cell in the row updates to "Negotiation" instantly; the Notes textarea retains the typed text while the panel is open.
  - Actual:

- [ ] **T28** — Editing one deal does not affect others (covers R9)
  - Steps:
    1. Edit Hooli Health from Critical to Healthy.
    2. Inspect Soylent row.
  - Expected: Soylent stays Critical with original value and reason. Only Hooli row changed.
  - Actual:

## Interaction — Persistence

- [ ] **T23** — Edits persist across page reload (covers R10)
  - Steps:
    1. Edit Acme Notes to "PERSISTENCE-CHECK".
    2. Press F5 to reload the page.
    3. Expand Acme.
  - Expected: Notes still read "PERSISTENCE-CHECK". `localStorage["dealDashboard.deals.v1"]` exists in DevTools → Application.
  - Actual:

- [ ] **T29** — Edits persist across browser restart (covers R10)
  - Steps:
    1. Edit Stark Industries value to 999999.
    2. Close the browser tab and reopen `app/index.html`.
  - Expected: Stark value is still 999999. Total Pipeline reflects the edited value.
  - Actual:

## Interaction — Reset

- [ ] **T24** — Reset button asks for confirmation (covers R11)
  - Steps:
    1. Click **Reset to defaults**.
  - Expected: A browser `confirm()` dialog appears. Clicking **Cancel** leaves data unchanged.
  - Actual:

- [ ] **T25** — Reset restores original 8 sample deals (covers R11)
  - Steps:
    1. Edit several deals (change value, status, notes).
    2. Click **Reset to defaults** and confirm.
  - Expected: All edits gone; the 8 default deals appear with original values, statuses, reasons. Summary tiles return to $880,000 / 4 / 2 / 2 / $387,000. `dealDashboard.deals.v1` storage key is removed.
  - Actual:

- [ ] **T26** — Reload after reset still shows defaults (covers R1, R11)
  - Steps:
    1. Perform T25.
    2. Reload the page.
  - Expected: Same 8 default deals reappear; no leftover edits.
  - Actual:

## Edge Cases

- [ ] **EC1** — Empty filter result behaves gracefully (covers R7)
  - Steps:
    1. Click **Critical**.
    2. Edit both Critical deals (Soylent and Hooli) to Healthy.
  - Expected: With Critical filter still active, the table body is empty (or shows an empty state message) without JavaScript errors. Switching filter to All shows all 8 deals.
  - Actual:

- [ ] **EC2** — Edit value to 0 (covers R9)
  - Steps:
    1. Expand any deal.
    2. Set Value to 0.
  - Expected: Row displays $0. Summary totals decrease accordingly. No NaN or error.
  - Actual:

- [ ] **EC3** — Edit value to a very large number (covers R9, R3)
  - Steps:
    1. Set a deal's value to 9999999999.
  - Expected: Value formats as currency without breaking layout; Total Pipeline reflects the change; no overflow / clipping in the row.
  - Actual:

- [ ] **EC4** — Negative or non-numeric value input (covers R9)
  - Steps:
    1. In a Value input, enter `-100` then `abc`.
  - Expected: Field is `type=number`; `abc` is rejected by the input. `-100` either is rejected by `min` or is stored as -100 without breaking totals (no NaN).
  - Actual:

- [ ] **EC5** — Long notes text does not break layout (covers R9, R6)
  - Steps:
    1. Paste a 1000-character note into Notes.
  - Expected: Textarea grows or scrolls; row layout remains intact; no horizontal page scroll on desktop.
  - Actual:

- [ ] **EC6** — Corrupted `localStorage` recovers (covers R10)
  - Steps:
    1. In DevTools, set `localStorage["dealDashboard.deals.v1"] = "not-json"`.
    2. Reload the page.
  - Expected: Page loads with the 8 default deals (graceful fallback) instead of a blank page or JS exception in the console.
  - Actual:

- [ ] **EC7** — Cancelling Reset keeps edits (covers R11)
  - Steps:
    1. Edit any deal.
    2. Click **Reset to defaults**, then click **Cancel** on the confirm dialog.
  - Expected: Edits remain intact; nothing is reset.
  - Actual:

## Interaction — Last Updated Timestamp

- [ ] **T30** — Last Updated timestamp updates on edit, persists on refresh, clears on reset (covers R13)
  - Steps:
    1. Reset to defaults. Confirm the line below the toolbar reads **"Original sample data"**.
    2. Open Acme Renewal and edit the Notes field.
    3. Refresh the page (F5).
    4. Click **Reset to defaults** and confirm.
  - Expected: After step 2, the line reads "Last updated: <today's date>, <time>". After step 3, the same timestamp is still shown. After step 4, the line reverts to "Original sample data".
  - Actual:

## Functional — Filter-Aware Summary

- [ ] **T31** — Total Deals tile shows count of all deals on load (covers R3, R14)
  - Steps:
    1. Reset to defaults.
    2. Read the first summary tile.
  - Expected: Headline metric reads **8**, label reads **Total Deals**, sub-line reads **"$880,000 · all deals"**.
  - Actual:

- [ ] **T32** — Summary updates when Critical filter is applied (covers R14)
  - Steps:
    1. Reset to defaults.
    2. Click the **Critical** filter button.
    3. Read all four summary tiles.
  - Expected: Total Deals = 2 with sub-line "$290,000 · filtered: Critical"; Healthy = 0; At Risk = 0; Critical = 2 with sub-line showing $290,000 at-risk + critical.
  - Actual:

- [ ] **T33** — Summary updates when Healthy and At Risk filters are applied, and resets on All (covers R14)
  - Steps:
    1. Click **Healthy**. Read tiles.
    2. Click **At Risk**. Read tiles.
    3. Click **All**. Read tiles.
  - Expected:
    - After step 1: Total = 4, Healthy = 4, At Risk = 0, Critical = 0, scope "filtered: Healthy".
    - After step 2: Total = 2, Healthy = 0, At Risk = 2, Critical = 0, scope "filtered: At Risk".
    - After step 3: Total = 8, Healthy = 4, At Risk = 2, Critical = 2, scope "all deals".
  - Actual:

- [ ] **T34** — Summary updates immediately when a deal's health is changed (covers R14)
  - Steps:
    1. Reset to defaults.
    2. Click the **All** filter (if not already active).
    3. Open Soylent Migration (Critical) and change Health to **Healthy** in the dropdown.
  - Expected: Critical tile count drops 2→1, Healthy tile count rises 4→5, Total Deals stays at 8. No page refresh required.
  - Actual:

