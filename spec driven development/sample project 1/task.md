# task.md: Rosie's Dog Walking Calculator — Task Tracker

## Phase B: HTML Scaffold & DOM (13 tasks)

### B-a: Scaffold HTML File (5 tasks)
- [ ] **B-a.1**: Create `index.html` with `<!DOCTYPE html>`, `<html lang="en">`, `<head>` (charset, viewport, title), and `<body>` [FR-8: offline after first load; AC-9: single HTML file with embedded CSS and JS]
- [ ] **B-a.2**: Add `<meta charset="UTF-8">` and `<meta name="viewport" content="width=device-width, initial-scale=1.0">` [FR-8]
- [ ] **B-a.3**: Add `<title>Rosie's Dog Walking Calculator</title>` [AC-10]
- [ ] **B-a.4**: Add embedded `<style>` block in `<head>` with basic reset and layout styles [AC-10: no frameworks, vanilla JS only]
- [ ] **B-a.5**: Add embedded `<script>` at the end of `<body>` (before `</body>`) [AC-9]

### B-b: DOM Structure Markup (5 tasks)
- [ ] **B-b.1**: Add dog count selector `<label>` group for 1, 2, 3+ dogs (use `<select>` or button group) with `id="dog-count"` [FR-1: dog count input; FR-8: minimum 1 dog]
- [ ] **B-b.2**: Add same-day booking `<label><input type="checkbox" id="same-day">` same-day booking [FR-3: same-day surcharge; FR-8: offline validation]
- [ ] **B-b.3**: Add holiday walk `<label><input type="checkbox" id="holiday">` holiday walk [FR-4: holiday double rate; FR-6: larger-wins conflict]
- [ ] **B-b.4**: Add output `<div>` with `id="output"` for breakdown and final total [FR-7: single config object display; AC-8: full breakdown shown]
- [ ] **B-b.5**: Add `<p id="error-msg" style="display:none">Please select at least 1 dog</p>` for validation [EC-4: zero dogs selected]

### B-c: Config Object & CSS Baseline (3 tasks)
- [ ] **B-c.1**: Inside `<script>`, add `const CONFIG = { baseRate: 5, sameDaySurcharge: 2, holidayMultiplier: 2, rounding: 2 };` [FR-2: $5 base rate; FR-9: configurable base rate; AC-7: owner edits one config object]
- [ ] **B-c.2**: Add basic CSS in `<style>`: body font-family, max-width container, centered layout, checkbox and select styling, output area formatting [AC-10: vanilla JS only, no frameworks]
- [ ] **B-c.3**: Ensure all CSS is embedded — no external `.css` file links [AC-9: single HTML file; constitution constraint]

### B-d: Verification (Human-Reviewed, 1 task)
- [ ] **B-d.1**: Open `index.html` in browser; verify page renders without errors [AC-10: no frameworks; AC-9: single file]
- [ ] **B-d.2**: Verify DOM elements are accessible: dog count changes, checkboxes toggle, output div present [FR-1 through FR-4]
- [ ] **B-d.3**: Confirm no external file requests (disconnect internet, refresh; should still load) [AC-8: offline after first load]
- [ ] **B-d.4**: Review task list with human — mark complete when human signs off that Phase B structure is correct before JS logic begins [go/no-go gate]

---

## Phase C: JS Calculation Logic (~8 tasks)

### C-a: `updateTotal()` Function & Calculation Order (5 tasks)
- [ ] **C-a.1**: Write `updateTotal()` function that reads `dogCount`, `same-day.checked`, `holiday.checked` [FR-1 through FR-8]
- [ ] **C-a.2**: Implement 5-step calculation order inside `updateTotal()`:
    1. `let total = dogCount * CONFIG.baseRate;`
    2. `let discount = dogCount > 1 ? CONFIG.baseRate * 0.5 * (dogCount - 1) : 0;`
    3. `let surcharge = sameDay ? CONFIG.sameDaySurcharge : 0;` (applied to full base, not discounted)
    4. Apply "larger wins" conflict resolution for holiday+same-day (holiday extra $5 > same-day $2 → holiday wins) [FR-6; EC-2; AC-6]
    5. `total = Math.round(total * 100) / 100;` (single rounding point) [EC-3; FR-7]
- [ ] **C-a.3**: Write logic to display breakdown in `#output`: base rate, discount amount, surcharge amount, holiday effect, final total [FR-7: full breakdown; AC-12]
- [ ] **C-a.4**: Wire each input: `dog-count.onchange = updateTotal;` `same-day.onclick = updateTotal;` `holiday.onclick = updateTotal;` [FR-6: real-time updates; AC-6]
- [ ] **C-a.5**: Add initial `updateTotal()` call on page load [AC-6: real-time from start]

### C-b: Human Testing & Sign-Off (2 tasks)
- [ ] **C-b.1**: Test all input combinations (1 dog no modifiers, 2 dogs, 3 dogs, same-day only, holiday only, both — should show holiday wins) against AC-1 through AC-12 [all acceptance criteria]
- [ ] **C-b.2**: Human review: confirm calculator matches spec, all pricing rules verified, sign off to proceed to Phase D [go/no-go gate]

---

## Phase D: CSS Styling & Final Integration (~5 tasks)

### D-a: CSS refinement (3 tasks)
- [ ] **D-a.1**: Refine `<style>` block for mobile-responsive layout (single column on phone, comfortable on desktop) [AC-10: single page; works on Rosie's likely phone use]
- [ ] **D-a.2**: Add visual distinction for error message `#error-msg` (red, visible) [EC-4]
- [ ] **D-a.3**: Add visual breakdown formatting: "Base rate: $X", "Multi-dog discount: $Y", "Same-day surcharge: $Z", "Holiday effect: $W", "Final total: $V" [AC-12: full breakdown shown]

### D-b: Final verification & sign-off (2 tasks)
- [ ] **D-b.1**: Test final `index.html`: open in browser, test all acceptance criteria (AC-1 through AC-12) [all acceptance criteria]
- [ ] **D-b.2**: Disconnect internet, refresh page; verify calculator still functions fully [AC-8: offline after first load]
- [ ] **D-b.3**: Change `CONFIG.baseRate` to 6, refresh; verify all totals update to new prices [AC-7: owner edits one config object]
- [ ] **D-b.4**: Human review and sign-off: calculator matches spec, all constraints met, ready for Rosie use [final go/no-go]

---

## Task Summary

| Phase | Tasks | Focus |
|---|---|---|
| **Phase B** | 13 tasks | HTML scaffold, DOM structure, config/CSS, human verification |
| **Phase C** | ~8 tasks | JS `updateTotal()` function, conflict resolution, event wiring, testing |
| **Phase D** | ~5 tasks | CSS refinement, final AC testing, offline verification, config edit verification |

**Total: 26+ checkable tasks**, each tagged to specific Functional Requirements (FR-1 through FR-11), Edge Cases (EC-1 through EC-6), and Acceptance Criteria (AC-1 through AC-12) from `spec.md`.

**Tracking system**: Each task has a `[ ]` checkbox. As tasks are completed, checkboxes are marked off (`[x]`). The human reviews each phase's task list before the next phase begins — no code is authored until the human signs off on the current phase's tasks.

**Go/No-Gate**: Phases B-d, C-b, and D-b.4 are explicit go/no-gate tasks. The human signs off that the previous phase's deliverables are correct before proceeding. This ensures the breakdown is reviewed, not authored, by the human at each transition point.