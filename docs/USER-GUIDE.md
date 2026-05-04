# Deal Health Dashboard — User Guide

Welcome! This guide shows you how to use the Deal Health Dashboard. No technical background needed.

## 1. What the Deal Health Dashboard is for

The Deal Health Dashboard is a single page that shows all your active sales deals in one place. It uses simple colors so you can tell **at a glance** which deals are doing well and which ones need your attention today.

Use it to answer questions like:

- How much money is in my pipeline right now?
- Which deals are about to slip?
- What should I work on first?

There is no login and no setup. Open the page and you see your deals.

## 2. How to view the deals

To open the dashboard:

1. Open the file `app/index.html` in any modern web browser (a *browser* is the program you use to visit websites — Chrome, Edge, Firefox, or Safari all work).
2. The page loads instantly with 8 sample deals.

What you will see, from top to bottom:

- **Title and description** at the very top.
- **Four summary tiles**:
  - **Total Deals** — the count of deals you're currently looking at, with the combined dollar value below.
  - **Healthy** — how many deals are on track, with their combined value.
  - **At Risk** — how many deals need attention soon, with their combined value.
  - **Critical** — how many deals are likely to slip, with the combined "needs attention" value (At Risk + Critical) below.
- **A toolbar** with filter buttons, a sort dropdown, and a Reset button.
- A small **"Last updated"** line just below the toolbar showing when you last edited a deal (or **"Original sample data"** if you haven't edited anything yet).
- **A table of deals**. Each row shows:
  - **Company** — the customer name.
  - **Value** — the deal size in dollars.
  - **Expected Close** — the date the deal is expected to close.
  - **Stage** — where the deal is in your sales process.
  - **Health** — a colored badge: 🟢 green (Healthy), 🟡 yellow (At Risk), 🔴 red (Critical), with a short reason underneath (for example, "No activity 10 days").

By default, the **deals that need the most attention appear at the top** — Critical first, then At Risk, then Healthy.

## 3. How to filter and sort

### Filter by status

Use the four buttons in the toolbar:

1. Click **All** to see every deal.
2. Click **Healthy** to see only green deals.
3. Click **At Risk** to see only yellow deals.
4. Click **Critical** to see only red deals.

The button you choose turns dark so you can tell which filter is active.

### Sort the list

Use the **Sort by** dropdown next to the filter buttons:

- **Needs attention first** (the default) — Critical at the top, then At Risk, then Healthy.
- **Value (high to low)** — biggest deals first.
- **Value (low to high)** — smallest deals first.
- **Close date (soonest)** — deals closing first.
- **Close date (latest)** — deals closing last.

Pick any combination of filter and sort. They work together.

## 3a. How to read the summary tiles

The four tiles at the top of the page give you a snapshot of your pipeline. They are designed to answer "how am I doing right now?" in one glance.

### What the summary shows

From left to right:

1. **Total Deals** — The big number is **how many deals** you're looking at. The smaller line underneath shows their **combined dollar value** and a label telling you the scope ("all deals" or "filtered: <Status>").
2. **Healthy** — The big number is **how many green deals**. The line underneath is their combined value.
3. **At Risk** — The big number is **how many yellow deals**. The line underneath is their combined value.
4. **Critical** — The big number is **how many red deals**. The line underneath is the combined value of **both** At Risk and Critical deals — your "needs attention" total.

With the default sample data and no filter applied, you'll see Total Deals = 8, Healthy = 4, At Risk = 2, Critical = 2.

### How the summary changes with filters

The tiles always describe **whatever you're currently looking at**, not the whole pipeline. When you click a filter button, the tiles update to match.

- Click **All** → the tiles show totals for every deal (Total Deals = 8 with sample data).
- Click **Critical** → the tiles show only the red deals. Total Deals drops to 2, Healthy and At Risk drop to 0, and the Total Deals sub-line reads **"$290,000 · filtered: Critical"** so you know you're not looking at the full pipeline.
- Click **At Risk** or **Healthy** → same idea: the tiles show just that subset.

The scope label in the Total Deals tile is your reminder of what's included in the numbers.

### How to interpret the numbers

- The **Total Deals** count answers "how many deals match what I'm looking at right now?"
- The **dollar value** under Total Deals answers "how much money is in this view?"
- The **three status counts** (Healthy / At Risk / Critical) tell you the breakdown by health within your current view.
- The **Critical tile sub-line** ("at-risk + critical") is your fastest "how much of my pipeline is in trouble?" answer when you're looking at the full set.
- If you change a deal's Health (for example, from Critical to Healthy), the relevant tiles update **immediately** — no need to refresh.

## 4. How to view and edit deal details

### View details

1. Click any row in the deals table.
2. A detail panel opens just below that row, showing **risk factors**, **last contact date**, **next steps**, and **notes**.
3. Click the same row again, or click a different row, to close it. Only one detail panel is open at a time.

### Edit a deal

Inside the open detail panel you can change four things:

1. **Value** — type a new number to update the deal size.
2. **Stage** — type to update where the deal is in your process.
3. **Health** — pick Healthy, At Risk, or Critical from the dropdown.
4. **Notes** — type any free-text notes you want to remember.

You will see your changes immediately. The colored badge updates as soon as you change Health, and the Value column and Total Pipeline tile update as you type. If you are sorted by "Needs attention first", the deal jumps to its new place in the list.

**You don't need to save.** Every change is saved automatically in your browser. If you close the tab and come back later, your edits are still there.

## 5. How to reset the data

If you want to start over with the original 8 sample deals:

1. Click the **Reset to defaults** button in the toolbar.
2. A small confirmation box appears asking if you're sure.
3. Click **OK** to confirm. The original deals come back and all your edits are erased.
4. Click **Cancel** if you change your mind. Nothing happens and your edits stay.

Reset only affects this dashboard in this browser. There is no server and nothing is sent anywhere.

---

## Quick tips

- **Try a demo flow:** Click **Critical** → click a red deal → change its Health to **Healthy** → watch it move out of the Critical view.
- **Refresh anytime.** Pressing F5 reloads the page; your edits stay.
- **Works on phones and tablets.** Narrow your browser window — the layout reflows to fit.

## Troubleshooting

- **The page is blank.** Make sure you opened `app/index.html` (not a folder) and that JavaScript is enabled in your browser.
- **My edits disappeared.** This usually means your browser cleared its storage. Click **Reset to defaults** to bring back the sample deals.
- **The Reset button doesn't do anything.** Make sure you clicked **OK** on the confirmation dialog.

---

*Last Updated: May 4, 2026*
