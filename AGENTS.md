# AGENTS.md

Instructions for the **GitHub Copilot coding agent** working autonomously on this repository.

## 1. Project Context

- This is a **Deal Health Dashboard** prototype.
- **Frontend-only** — no backend, no database, no server.
- Requirements live in [specs/PRD.md](specs/PRD.md).
- Task list lives in [specs/Tasks.md](specs/Tasks.md).
- The entire app is a single file: [app/index.html](app/index.html).

## 2. Build and Test

- **No build step.** Open `app/index.html` directly in a browser.
- Test by exercising all interactive features:
  - Filters (All / Healthy / At Risk / Critical) and sort options.
  - Click a row to expand the detail panel.
  - Edit stage, status, value, and notes — confirm changes persist after reload (`localStorage`).
  - Click **Reset** to restore the sample data.
- Verify against the requirements in [specs/PRD.md](specs/PRD.md) before opening a PR.

## 3. Coding Standards

- **Vanilla JavaScript, HTML, and CSS only.** No frameworks, no libraries, no build tools.
- Keep all code in the single file [app/index.html](app/index.html).
- Follow all rules in [.github/copilot-instructions.md](.github/copilot-instructions.md).
- Work **one task at a time** from [specs/Tasks.md](specs/Tasks.md). Mark the task `[x]` and append a Progress Log entry describing what changed, the requirement satisfied, and browser test steps.

## 4. What NOT To Do

- Do **not** add backend services, servers, or databases.
- Do **not** add authentication, login, or user accounts.
- Do **not** install npm packages or any dependencies.
- Do **not** add external APIs, services, or secret keys.
- Do **not** add frameworks (React, Vue, Angular, etc.).
- Do **not** make changes outside the requirements in [specs/PRD.md](specs/PRD.md). If a new feature is requested, add it to the PRD and Tasks.md first.
