# spec.md: Rosie's Dog Walking Calculator

## Goal
Provide Rosie's Dog Walking with a single-page instant quote calculator that both customers and Rosie can use. Customers get immediate pricing without texting, and Rosie uses the same page to quickly respond to texts without hand-working calculations. The calculator must reflect Rosie's specific pricing rules: 2nd dog 50% off (not free), same-day booking surcharge, holiday walks double, and no stacking discounts. All pricing derives from one config object the owner can edit without code changes.

### Clarified pricing rules (from client interview):
- **Base rate:** $5.00 per dog
- **Same-day surcharge:** $2.00 (applied to the FULL base rate, not the discounted multi-dog rate)
- **50% off:** Applies to EVERY dog after the first (2nd, 3rd, 4th all get 50% off)
- **No stacking:** When holiday AND same-day both selected, the LARGER of the two mechanisms wins (not both compounded)
- **Full breakdown shown:** The calculator displays base rate, discount amounts, surcharge amounts, and the final total so Rosie can double-check before quoting
- **Minimum 1 dog:** Zero or negative dog selection is invalid

## User Scenarios

### Customer using the calculator directly
- Customer visits the single-page calculator on mobile or desktop
- Customer selects number of dogs (1 or more)
- Customer toggles "Same-day booking" if applicable
- Customer toggles "Holiday walk" if applicable
- Customer sees the total estimated cost update in real time, with a full breakdown (base rate, discount, surcharge, holiday multiplier effect)
- Customer contacts Rosie or books through whatever channel they prefer

### Rosie using the same page when someone texts
- Rosie receives a text inquiry (e.g., "How much for 2 dogs same-day?")
- Rosie opens the calculator page on her phone or computer
- Rosie selects the number of dogs, same-day if applicable, holiday if applicable
- Rosie reads the total shown and the full breakdown, gives the quote to the customer over text
- Rosie can refresh the page later or edit the config object to update prices

## Functional Requirements

**FR-1: Dog count input**
- The calculator must have a dog count selector allowing the user to choose 1, 2, or 3+ dogs
- A build ignoring this requirement would show no dog count selector, making it impossible to differentiate pricing by number of dogs

**FR-2: $5.00 base rate per dog**
- The base rate per dog is $5.00 (configurable via config object)
- A build with a different base rate would produce incorrect quotes matching Rosie's business

**FR-3: 2nd dog 50% off — applies to ALL dogs after the first**
- When 2 or more dogs are selected, every dog after the first receives 50% off (2nd dog at 50%, 3rd dog at 50%, etc.)
- A build applying 50% off only to the 2nd dog, or not applying it to 3rd+ dogs, would produce incorrect totals for 3+ dog walks

**FR-4: Same-day booking surcharge of $2.00**
- The calculator must have a same-day toggle/switch
- When same-day is selected, a $2.00 surcharge must be added to the total (applied to the full base rate, not the discounted rate)
- A build ignoring this requirement would not add the surcharge, producing incorrect quotes for same-day requests

**FR-5: Holiday walk doubles the base rate**
- The calculator must have a holiday toggle/switch
- When holiday is selected, the base rate is doubled (2x)
- A build missing this requirement would charge the normal rate for holiday walks

**FR-6: No stacking — larger wins when holiday AND same-day both selected**
- When both holiday and same-day are selected, only the larger mechanism applies (not both compounded)
- Holiday: 2x the base rate. Same-day: $2.00 surcharge. Since 2x $5 = $10 > $2, holiday wins in the "larger wins" rule
- A build applying both would overcharge the customer

**FR-7: Real-time total update**
- The displayed total must update instantly whenever any input (dog count, same-day, holiday) changes
- A build without this would require a "Calculate" button press, producing a static result that doesn't reflect live input changes

**FR-8: Full breakdown display**
- The calculator must show a full breakdown: base rate per dog, 2nd-and-later dog discount amount, same-day surcharge amount (if applicable), holiday multiplier effect (if applicable), and final total
- A build showing only the final price without breakdown would prevent Rosie from double-checking the math before quoting

**FR-9: Single config object for pricing**
- All base rates, surcharges, and multipliers must live in one config object the owner can edit directly without code changes
- A build hardcoding prices would prevent Rosie from updating rates without a developer

**FR-10: Offline after first load**
- The calculator must work offline after the first page load; no backend or external API calls required
- A build requiring connectivity would fail in offline situations

**FR-11: At least one dog must be selected**
- The calculator must validate that at least 1 dog is selected; 0 dogs is invalid input
- A build not handling this would show NaN or incorrect pricing for zero dogs

## Edge Cases & Rules

**EC-1: 3+ dogs pricing**
- 1st dog: full $5.00 base rate
- 2nd dog and all subsequent dogs: $2.50 each (50% off $5.00)
- Example: 3 dogs = $5.00 + $2.50 + $2.50 = $10.00 (before any surcharges/multipliers)

**EC-2: Same-day AND holiday selected — larger wins**
- When both holiday and same-day are selected, only the larger mechanism applies
- Holiday: 2x base rate = $10.00 for 1 dog. Same-day: $2.00 surcharge.
- Holiday wins ($10 > $2), so the customer pays the holiday doubled rate, not the same-day surcharge on top
- A build applying both would overcharge ($12 instead of $10 for 1 dog)

**EC-3: Rounding to 2 decimal places**
- The final total must be rounded to 2 decimal places (currency format)
- A build without explicit rounding would display floating-point results (e.g., $45.00000001) that appear incorrect

**EC-4: Zero or negative dogs selected**
- The calculator must reject 0 or negative dog selection; at least 1 dog must be chosen
- A build not handling this would show NaN or incorrect pricing

**EC-5: Same-day definition**
- "Same-day" means booking for a walk today (the calculator must have a clear mechanism; a build without this would not correctly identify same-day requests)

**EC-6: Holiday date range**
- The holiday multiplier must apply only within specified holiday dates (to be defined in config)
- A build applying the holiday multiplier year-round would incorrectly double prices outside holiday seasons

## Out-of-Scope

- **OOS-1: Backend, database, or user accounts**
  No user accounts, logins, database storage, or backend. The calculator is a standalone single-page tool.

- **OOS-2: Booking integration or payment collection**
  No booking flow, payment processing, or calendar integration. The calculator provides estimates only.

- **OOS-3: SMS/MMS integration**
  No text message sending or receiving. Rosie uses the calculator page alongside her existing text conversations.

- **OOS-4: Custom scope beyond dogs/same-day/holiday**
  No add-ons beyond: number of dogs, same-day booking, holiday walks. No distance, duration, or other variables.

- **OOS-5: Admin dashboard or config management UI**
  The config object is edited by directly modifying the HTML/JS file, not via a separate admin interface.

- **OOS-6: Multiple dog walkers or team pricing**
  Pricing applies to Rosie's rates only. No variations for different walkers.

## Acceptance Criteria

**AC-1: 1 dog, no same-day, no holiday → total = $5.00**
- Given 1 dog, no same-day, no holiday → total = $5.00 (base rate)
- A build ignoring this would show incorrect total for 1 dog

**AC-2: 2 dogs, no same-day, no holiday → total = $7.50**
- Given 2 dogs, no same-day, no holiday → total = $5.00 + $2.50 = $7.50 (1st full, 2nd 50% off)
- A build ignoring this would charge 2× $5.00 = $10.00 or $5.00 + free

**AC-3: 3 dogs, no same-day, no holiday → total = $10.00**
- Given 3 dogs, no same-day, no holiday → total = $5.00 + $2.50 + $2.50 = $10.00 (1st full, 2nd and 3rd at 50% off)
- A build ignoring this would charge 3× $5.00 = $15.00 or incorrect tiering

**AC-4: 1 dog, same-day yes → total = $7.00**
- Given 1 dog, same-day yes → total = $5.00 base + $2.00 surcharge = $7.00
- A build ignoring this would not add the surcharge ($5.00 instead of $7.00)

**AC-5: 1 dog, holiday yes → total = $10.00**
- Given 1 dog, holiday yes → total = $5.00 × 2 = $10.00 (doubled rate)
- A build ignoring this would charge the normal rate ($5.00 instead of $10.00)

**AC-6: 1 dog, same-day yes + holiday yes → total = $10.00 (holiday wins)**
- Given 1 dog, same-day yes + holiday yes → total = $10.00 (holiday doubling is larger than $2.00 surcharge, so larger wins — only holiday applies)
- A build applying both would overcharge $12.00

**AC-7: Real-time total updates as inputs change**
- Changing dog count, same-day toggle, or holiday toggle updates the total instantly without a button press
- A build requiring a "Calculate" button press fails this criterion

**AC-8: Owner can edit prices by changing one config object**
- Rosie can open the HTML file and change the base rate, surcharge amount, or holiday multiplier in the config object and save; the new prices appear immediately on refresh
- A build hardcoding prices fails this criterion

**AC-9: Page works offline after first load**
- After the initial page load, the calculator functions without internet connectivity
- A build requiring external API calls fails this criterion

**AC-10: Page is a single HTML file with embedded CSS and JS**
- No separate CSS or JS files; all code is embedded inline
- A build using external file references fails this criterion

**AC-11: No frameworks (vanilla JS only)**
- No React, Vue, jQuery, or other frameworks
- A build using a framework fails this criterion

**AC-12: Full breakdown shown includes base rate, discount, surcharge, and final total**
- The calculator displays the breakdown so Rosie can double-check before quoting
- A build without a breakdown prevents Rosie from verifying the math