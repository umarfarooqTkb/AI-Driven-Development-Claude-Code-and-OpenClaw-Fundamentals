# plan.md: Rosie's Dog Walking Calculator — Technical Plan

## Phase A: Technical Architecture Decisions (Complete)

### 8 Key Decisions Referenced Below

| # | Decision | Key Choice |
|---|---|---|
| 1 | File structure | Single `index.html` |
| 2 | Config object | One `CONFIG` object at bottom of `<script>` |
| 3 | Calculation order | 5-step deterministic sequence (base → discount → same-day → holiday → round) |
| 4 | Holiday+same-day conflict | "Larger wins" resolved before math (holiday extra $5 > same-day $2) |
| 5 | DOM structure | Minimal: dog count selector, same-day checkbox, holiday checkbox, breakdown + total output div |
| 6 | Event handling | Real-time on every input change; no "Calculate" button |
| 7 | Rounding | `Math.round(total × 100) / 100` applied once at end |
| 8 | Input validation | At least 1 dog required; show error if 0 selected |

---

## Phase B: Task Breakdown (Checkable Task List)

### Phase B-a: Scaffold HTML File
- [ ] **B-a.1**: Create `index.html` with `<!DOCTYPE html>`, `<html lang="en">`, `<head>` (charset, viewport, title), and `<body>` [FR-8: offline after first load; AC-9: single HTML file with embedded CSS and JS]
- [ ] **B-a.2**: Add `<meta charset="UTF-8">` and `<meta name="viewport" content="width=device-width, initial-scale=1.0">` [FR-8]
- [ ] **B-a.3**: Add `<title>Rosie's Dog Walking Calculator</title>` [AC-10]
- [ ] **B-a.4**: Add embedded `<style>` block in `<head>` with basic reset and layout styles [AC-10: no frameworks, vanilla JS only]
- [ ] **B-a.5**: Add embedded `<script>` at the end of `<body>` (before `</body>`) [AC-9]

### Phase B-b: DOM Structure Markup
- [ ] **B-b.1**: Add dog count selector `<label>` group for 1, 2, 3+ dogs (use `<select>` or button group) with `id="dog-count"` [FR-1: dog count input; FR-8: minimum 1 dog]
- [ ] **B-b.2**: Add same-day booking `<label><input type="checkbox" id="same-day">` same-day booking [FR-3: same-day surcharge; FR-8: offline validation]
- [ ] **B-b.3**: Add holiday walk `<label><input type="checkbox" id="holiday">` holiday walk [FR-4: holiday double rate; FR-6: larger-wins conflict]
- [ ] **B-b.4**: Add output `<div>` with `id="-output"` for breakdown and final total [FR-7: single config object display; AC-8: full breakdown shown]
- [ ] **B-b.5**: Add `<p id="error-msg" style="display:none">Please select at least 1 dog</p>` for validation [EC-4: zero dogs selected]

### Phase B-c: Config Object & CSS Baseline
- [ ] **B-c.1**: Inside `<script>`, add `const CONFIG = { baseRate: 5, sameDaySurcharge: 2, holidayMultiplier: 2, rounding: 2 };` right before the `updateTotal` function [FR-2: $5 base rate; FR-9: configurable base rate; AC-7: owner edits one config object]
- [ ] **B-c.2**: Add basic CSS in `<style>`: body font-family, max-width container, centered layout, checkbox and select styling, output area formatting [AC-10: vanilla JS only, no frameworks]
- [ ] **B-c.3**: Ensure all CSS is embedded — no external `.css` file links [AC-9: single HTML file; constitution constraint]

### Phase B-d: Verification (Human Review)
- [ ] **B-d.1**: Open `index.html` in browser; verify page renders without errors [AC-10: no frameworks; AC-9: single file]
- [ ] **B-d.2**: Verify DOM elements are accessible: dog count changes, checkboxes toggle, output div present [FR-1 through FR-4]
- [ ] **B-d.3**: Confirm no external file requests (disconnect internet, refresh; should still load) [AC-8: offline after first load]
- [ ] **B-d.4**: Review task list with human — mark complete when human signs off that Phase B structure is correct before JS logic begins

---

## Phase C: JS Calculation Logic (Next)
- [ ] **C-a.1**: Write `updateTotal()` function that reads `dogCount`, `same-day.checked`, `holiday.checked` [FR-1 through FR-8]
- [ ] **C-a.2**: Implement 5-step calculation order inside `updateTotal()`:
    1. `let total = dogCount * CONFIG.baseRate;`
    2. `let discount = dogCount > 1 ? CONFIG.baseRate * 0.5 * (dogCount - 1) : 0;`
    3. `let surcharge = sameDay ? CONFIG.sameDaySurcharge : 0;` (applied to full base, not discounted)
    4. Apply "larger wins" conflict resolution for holiday+same-day (Decision 4 in Phase A) [FR-6; EC-2; AC-6]
    5. `total = Math.round(total * 100) / 100;` (single rounding point) [EC-3; FR-7]
- [ ] **C-a.3**: Write logic to display breakdown in `#-output`: base rate, discount amount, surcharge amount, holiday effect, final total [FR-7: full breakdown; AC-12]
- [ ] **C-a.4**: Wire each input: `dog-count.onchange = updateTotal;` `same-day.onclick = updateTotal;` `holiday.onclick = updateTotal;` [FR-6: real-time updates; AC-6]
- [ ] **C-a.4**: Add initial `updateTotal()` call on page load [AC-6: real-time from start]
- [ ] **C-d.1**: Human review: test all input combinations (1 dog no modifiers, 2 dogs, 3 dogs, same-day only, holiday only, both — should show holiday wins) against AC-1 through AC-12

---

## Phase D: CSS Styling & Final Integration (Next)
- [ ] **D-a.1**: Refine `<style>` block for mobile-responsive layout (single column on phone, comfortable on desktop) [AC-10: single page; works on Rosie's likely phone use]
- [ ] **D-a.2**: Add visual distinction for error message `#error-msg` (red, visible) [EC-4]
- [ ] **D-a.3**: Add visual breakdown formatting: "Base rate: $X", "Multi-dog discount: $Y", "Same-day surcharge: $Z", "Holiday effect: $W", "Final total: $V" [AC-12: full breakdown shown]
- [ ] **D-b.1**: Test final `index.html`: open in browser, test all acceptance criteria (AC-1 through AC-12) [all acceptance criteria]
- [ ] **D-b.2**: Disconnect internet, refresh page; verify calculator still functions fully [AC-8: offline after first load]
- [ ] **D-b.3**: Change `CONFIG.baseRate` to 6, refresh; verify all totals update to new prices [AC-7: owner edits one config object]
- [ ] **D-b.4**: Human review and sign-off: calculator matches spec, all constraints met, ready for Rosie use

---

## Summary of Task Tracking

Each phase breaks into concrete, checkable tasks tagged to functional requirements (FR-1 through FR-11) and acceptance criteria (AC-1 through AC-12). The human (you or Rosie) reviews each phase task list before the next phase begins — no code is authored until the human signs off on the current phase's tasks.

**Phase B total**: 13 tasks (B-a through B-d) covering HTML scaffold, DOM structure, config/CSS, and verification.

**Phase C total**: ~8 tasks covering JS calculation function, conflict resolution, event wiring, and human testing.

**Phase D total**: ~5 tasks covering CSS refinement, final testing against all ACs, offline verification, config edit verification.

**Overall**: 26+ checkable tasks, each tagged to specific FRs/ECs/ACs from the approved spec.md. The breakdown is ready for human review — not authored forward until Phase B tasks are signed off.