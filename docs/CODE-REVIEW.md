# Code Review — `app/index.html`

**Reviewer:** review-agent (Senior Developer)
**Date:** May 1, 2026
**Reviewed against:**
- Requirements: [specs/PRD.md](../specs/PRD.md) (R1–R12)
- Project rules: [.github/copilot-instructions.md](../.github/copilot-instructions.md)
- Frontend rules: [.github/instructions/app.instructions.md](../.github/instructions/app.instructions.md)

---

## 1. Summary

The dashboard is a tight, well-structured single-file vanilla JS prototype that satisfies every requirement R1–R12 functionally. The CSS architecture uses design tokens cleanly, the data layer has graceful `localStorage` fallback, and the editing flow is thoughtfully split between `input` (focus-preserving) and `change` (full re-render) handlers. The code is readable and the variable names are meaningful.

The main concerns are **HTML escaping** (a real bug today: any user-typed `"` or `<` in stage/notes will break rendering, with a security smell that becomes a real XSS vector if data ever crosses a trust boundary) and a few **frontend-instruction violations** (touch targets below 44px, desktop-first instead of mobile-first CSS, no `aria-pressed` on filter toggles).

## 2. What's Good

- **Single-file, vanilla JS, no frameworks.** Fully compliant with [.github/copilot-instructions.md](../.github/copilot-instructions.md) and [AGENTS.md](../AGENTS.md).
- **CSS custom properties** for all colors (`--green`, `--red`, `--bg`, etc.) — matches the styling rule in [app.instructions.md](../.github/instructions/app.instructions.md).
- **ES6+ idiomatic JS** — `const`/`let`, arrow functions, template literals, `structuredClone`, `Intl.NumberFormat`, `localeCompare`.
- **Defensive `loadDeals()`** — try/catch with graceful fallback to defaults if `localStorage` is corrupted (covers EC6 from the test plan).
- **Smart event-handler split** — `input` for stage/notes preserves focus by mutating only the affected cell; `change` for status/value triggers full re-render so totals/sort/badge update. Non-obvious correct design.
- **Semantic HTML** — `<header>`, `<section>`, `<table>` with `<thead>`/`<tbody>`, real `<button>` and `<select>` elements (keyboard-accessible by default).
- **ARIA group** on the filters container with a meaningful `aria-label`.
- **Tabular numerics** (`font-variant-numeric: tabular-nums`) so currency and dates align — small but professional touch.
- **`structuredClone(defaultDeals)`** on reset — avoids accidental mutation of the seed array.
- **Comments where they add value** — the `input` vs `change` split is commented to explain *why*, matching the "explain why, not what" rule.

## 3. Issues Found

### High severity

- **H1 — Unescaped user input in `innerHTML`.**
  `renderDeals()` interpolates `d.company`, `d.stage`, `d.reason`, `d.notes`, `d.nextSteps`, and each `d.riskFactors[i]` directly into a template literal that becomes `tbody.innerHTML`. The same applies to `data-company="${d.company}"` and `value="${d.stage}"` attributes.
  - **Functional impact (today):** A user editing Stage to `Sales "POC"` corrupts the `<input value="...">` attribute and the row breaks. Editing Notes to text containing `</textarea>` exits the textarea early.
  - **Security impact (latent):** Today the data only comes from the user themselves, but the moment data is imported, shared, or seeded from another source, any `<script>` or `<img onerror>` in a string is an XSS. Violates OWASP A03:2021 (Injection) baseline.
  - **Fix:** Use `textContent` for user-controlled strings, or build rows with `document.createElement` + `.textContent`/`.value`, or pipe strings through an `escapeHtml()` helper before interpolation.

### Medium severity

- **M1 — Touch targets are below 44×44 px.**
  The frontend rule in [app.instructions.md](../.github/instructions/app.instructions.md) states "Minimum touch target size: 44x44 pixels". Measured:
  - `.filter-btn`: `padding: 6px 12px` + 13px font ≈ **~26 px tall**.
  - `.reset-btn`: same padding ≈ **~26 px tall**.
  - `.sort-control select`: `padding: 6px 28px 6px 10px` ≈ **~28 px tall**.
  - The `@media (max-width: 480px)` block *shrinks* paddings further, making the gap wider.
  - **Fix:** Bump padding to roughly `padding: 10px 14px` (or set `min-height: 44px`) on all interactive controls.

- **M2 — Desktop-first CSS, but the rule says mobile-first.**
  Base styles target ~1100 px; phone styles live in `@media (max-width: …)` overrides. The instruction file calls for mobile-first (`@media (min-width: …)` overrides instead). Functionally identical, but contrary to the stated standard.
  - **Fix path (low cost):** Leave as-is and update the instruction file, *or* invert the breakpoints in a future refactor pass — don't refactor inside an unrelated task.

- **M3 — Filter buttons don't expose toggle state to assistive tech.**
  The active filter is communicated only by a `.active` class (background color). Screen reader users can't tell which filter is selected. The `role="group"` is correct, but the buttons themselves need either `aria-pressed="true|false"` or a radiogroup pattern. Conflicts with "Include ARIA labels where needed" in [app.instructions.md](../.github/instructions/app.instructions.md).
  - **Fix:** Add `aria-pressed` toggling alongside the `.active` class in the click handler.

### Low severity

- **L1 — Missing `default` branch in the sort `switch`.** If `activeSort` ever becomes an unknown value, the comparator returns `undefined` and `Array.sort` behavior is undefined per spec. Add `default: return 0;`.
- **L2 — `data-company` is the de-facto primary key.** If two deals ever shared a company name, edits would target the first match. Consider a stable `id` field; not urgent for the demo.
- **L3 — Decorative dots lack `aria-hidden="true"`.** Add to `.badge .dot`, `.filter-btn .dot`, and `.chevron`.
- **L4 — `value="${d.value}"` on the number input.** Works for integers; if `Number(target.value)` ever produces `NaN`, the fallback `|| 0` masks it silently. Acceptable for a prototype but worth a comment.
- **L5 — `confirm()` for Reset.** Functional and accessible, but a styled modal would feel more polished. Out of scope unless the PRD calls for it (it doesn't).
- **L6 — `<ul>` inside `<span class="field-value">`.** A `<ul>` inside a `<span>` is invalid HTML (block in inline). Browsers tolerate it, but a validator will flag it. Change the wrapper to `<div class="field-value">`.

## 4. Suggestions

Highest leverage, lowest effort:

1. **Add a 6-line `escapeHtml()` helper** and pipe every user-controlled string through it in `renderDeals()`. Resolves H1 for both functionality (quotes in stage names) and security (latent XSS). Single file, ~15-line diff.
2. **Bump touch-target padding** on `.filter-btn`, `.reset-btn`, and `.sort-control select` to ≥44 px tall. Resolves M1.
3. **Toggle `aria-pressed`** on filter buttons in the existing click handler. ~3 lines. Resolves M3.
4. **Add `default: return 0;`** to the sort `switch`. 1 line. Resolves L1.
5. **Replace `<span class="field-value">` wrapping `<ul>` with `<div>`.** 1 line. Resolves L6.

Nice-to-have, separate task:

- Promote the desktop-first CSS to mobile-first (M2) — touches a lot of selectors; warrants its own task in [specs/Tasks.md](../specs/Tasks.md) per the "one task at a time" rule.

## 5. Verdict

**Request Changes** — accept the prototype's functionality (R1–R12 all PASS), but resolve **H1** (escaping) and **M1** (touch targets) before treating this as demo-ready. Both are explicit violations of stated rules and both have small, contained fixes.

The remaining medium and low issues should be filed as new tasks in [specs/Tasks.md](../specs/Tasks.md) following the project's one-task-at-a-time workflow rather than batched into a single change.

---

# R14 Review — Filter-aware summary tiles

**Reviewer:** review-agent (Senior Developer)
**Date:** May 4, 2026
**Scope:** Task 14 changes to `renderSummary()`, the new `getFilteredDeals()` helper, refactored summary helpers, and the filter click handler.
**Reviewed against:**
- Requirements: [specs/PRD.md](../specs/PRD.md) R3 (extended), R14
- Frontend rules: [.github/instructions/app.instructions.md](../.github/instructions/app.instructions.md)

## R14.1 Summary

The R14 change is a small, focused refactor that gets the spec right. The summary helpers were cleanly parameterised with a `data` argument, a single `getFilteredDeals()` helper now drives the filtered view, and the summary tiles re-render on every filter change. The new scope label ("filtered: <Status>") is a nice usability touch — it makes the otherwise ambiguous "Total Deals = 2" understandable at a glance. The first tile was redesigned to lead with **Total Deals** (count) and the fourth was renamed **Critical** for consistency, both directly addressing the R3 extension. R14 acceptance criteria all pass behaviorally.

The main concerns are **a missed DRY opportunity** (the `renderDeals()` inline filter wasn't switched over to the new `getFilteredDeals()` helper) and **two accessibility gaps** carried into the change (no `aria-live` on the summary region; the existing `aria-pressed` gap on filter buttons becomes more impactful now that clicks affect more of the page).

## R14.2 What's Good

- **Clean parameterisation.** Refactoring `totalPipeline`, `countBy`, `valueBy`, `needsAttentionTotal` to take a `data` argument is exactly the right surgery — the helpers stay pure, and the same code now serves filtered and unfiltered totals.
- **`getFilteredDeals()` extracted.** Single source of truth for the filter rule. Makes the intent explicit and easy to maintain.
- **Scope label.** Putting `"all deals"` / `"filtered: <Status>"` in the Total Deals sub-line is a thoughtful affordance. Without it, a user staring at "Total Deals = 2" after clicking a filter would have no signal that the number is filtered.
- **Re-renders on filter change.** The click handler now calls `renderSummary()` and `renderDeals()` together — correct ordering, no flicker.
- **Helpful comment** above the helper block explaining *why* (`R3 + R14: helpers compute totals over whatever dataset is passed in`) — matches the "explain why, not what" rule from [app.instructions.md](../.github/instructions/app.instructions.md).
- **Initial-call ordering fixed.** Moving the bootstrap `renderSummary()` call below `let activeFilter = "all"` cleanly avoids the Temporal Dead Zone trap created by reading `activeFilter` inside the function.
- **No new XSS surface.** The new template-literal interpolations (`scopeLabel`, `data.length`, currency-formatted numbers) are all derived from constants, integers, and `Intl.NumberFormat` output — none come from user input. H1 from the prior review is unchanged in scope by this work.
- **No accidental scope creep.** The change is confined to summary rendering; no unrelated refactors snuck in.

## R14.3 Issues Found

### Medium severity

- **M4 — `renderDeals()` duplicates the filter rule that `getFilteredDeals()` now owns.**
  Inside `renderDeals()`, the first lines still read `let visible = activeFilter === "all" ? deals.slice() : deals.filter(d => d.status === activeFilter);` — the exact body of the new `getFilteredDeals()` helper. If the filter rule ever changes (e.g. multi-select filters, search box), this becomes a two-place edit and the summary and table can disagree.
  - **Fix (1 line):** Replace those two lines with `let visible = getFilteredDeals();`.

- **M5 — `#summary` is not announced when filters change.**
  Filter clicks now meaningfully change the content of the summary tiles, but the `#summary` element has no `aria-live` region and no `role="region"`/label, so a screen-reader user gets no feedback that the totals updated. The existing `#last-updated` element correctly uses `aria-live="polite"` — the same treatment applied to `#summary` (or to the Total Deals tile specifically) would close this gap.
  - **Fix (1 line):** Add `aria-live="polite"` and `aria-atomic="true"` to the `#summary` container, or wrap the scope-aware "Total Deals" sub-line in a small `aria-live` element to keep announcements terse.

### Low severity

- **L7 — Each `renderSummary()` call walks the dataset 7 times.**
  `getFilteredDeals()` returns a copy, then `totalPipeline(data)` reduces over it, `countBy(data, s)` filters it three times, `valueBy(data, s)` filters+reduces it three times, and `needsAttentionTotal(data)` calls `valueBy` twice more. With 8 deals this is invisible (microseconds), so it's strictly a code-smell rather than a real performance bug. If the dataset ever grew, a single `data.reduce` producing `{count, total, byStatus: {...}}` would replace all of them with one O(n) pass.
  - **Fix (optional):** Inline a single-pass aggregator inside `renderSummary()` and drop the four helpers. Don't do this in the R14 task — file it as its own.

- **L8 — Tile semantics shifted but the color treatment didn't.**
  The fourth tile was renamed Needs Attention → **Critical** and its metric switched from a dollar amount to a count. The dollar amount moved to the sub-line ("$X at-risk + critical"). The red `.tile.critical` accent now applies to a count, which is fine, but a returning user who was used to "the red tile is the dollar number that matters" may briefly misread it. Worth a short note in the user guide (already addressed in [docs/USER-GUIDE.md](USER-GUIDE.md) §3a).

## R14.4 Suggestions

In priority order, all single-line or near-single-line fixes:

1. Replace the inline filter in `renderDeals()` with `let visible = getFilteredDeals();` — closes M4 and removes the duplication risk introduced by this change.
2. Add `aria-live="polite" aria-atomic="true"` to the `#summary` `<section>` — closes M5 and is consistent with how `#last-updated` is already handled.
3. (Future task only) Collapse the four summary helpers into a single-pass aggregator if the dataset ever grows beyond a few hundred deals — addresses L7.

H1 (innerHTML escaping) and M1/M3 (touch targets, `aria-pressed`) from the prior review are still open and remain the highest-value cleanups overall, but they are not regressions introduced by R14.

## R14.5 Verdict

**Approve with follow-ups** — R14 meets its acceptance criteria, the refactor is small and clean, and the change introduces no regressions. M4 and M5 are worth fixing in a quick follow-up task (combined diff is ~3 lines) but neither blocks the feature. Recommend filing each as its own task in [specs/Tasks.md](../specs/Tasks.md) per the one-task-at-a-time rule.
