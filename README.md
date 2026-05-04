# Deal Health Dashboard

A lightweight, single-page prototype that gives a sales manager an at-a-glance view of every active deal — so it's instantly obvious which deals are on track and which need attention.

The entire app lives in one file: [app/index.html](app/index.html). No backend, no build step, no dependencies.

## Features

- **Deal list** — Company, value, close date, stage, and health for every deal.
- **Color-coded health** — 🟢 Healthy, 🟡 At Risk, 🔴 Critical badges driven by the deal's status.
- **Summary tiles** — Total deals, total pipeline value, status breakdown, and Critical totals at the top of the page.
- **Filter-aware summary** — Tiles recalculate to match the active filter, with a scope label ("all deals" or "filtered: …").
- **Filter buttons** — All / Healthy / At Risk / Critical, with the active filter highlighted.
- **Sort** — Default "needs attention" order (Critical → At Risk → Healthy), or sort by value or close date.
- **Expandable detail panel** — Click any row to reveal risk factors, last contact date, next steps, and notes.
- **Inline editing** — Edit value, stage, health, and notes; the table, badges, and totals update live.
- **Persistent storage** — Edits survive a page refresh via `localStorage`.
- **Last Updated timestamp** — Shows when the data was last edited; clears after a reset.
- **Reset to defaults** — One click restores the original sample data.
- **Responsive layout** — Reflows cleanly from desktop down to ~375px phone widths.

## Run It

Open [app/index.html](app/index.html) in any modern browser. That's it.

## Project Structure

- [app/index.html](app/index.html) — The entire application (HTML, CSS, JS, sample data).
- [specs/PRD.md](specs/PRD.md) — Product requirements (R1–R14) and verification report.
- [specs/Tasks.md](specs/Tasks.md) — Task list and progress log.
- [docs/USER-GUIDE.md](docs/USER-GUIDE.md) — End-user walkthrough.
- [docs/CODE-REVIEW.md](docs/CODE-REVIEW.md) — Code review findings.
- [tests/test-plan.md](tests/test-plan.md) — Manual test plan.

## Sample Data

The dashboard ships with 8 hardcoded sample deals (Acme, Globex, Initech, Umbrella, Soylent, Hooli, Pied Piper, Stark) totalling $880,000 in pipeline. No real customer data.

## Constraints

Vanilla HTML, CSS, and JavaScript only — no frameworks, no npm packages, no backend, no authentication. See [.github/copilot-instructions.md](.github/copilot-instructions.md) and [AGENTS.md](AGENTS.md) for the full ground rules.
