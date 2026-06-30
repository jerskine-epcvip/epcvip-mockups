# Task 5 Brief — QA Pass: Mobile, Accessibility, Final Polish

**File to modify:** `/Users/jason/Development/claude_proj/ewa-poc/variant-2-warm-friendly/index.html`

**Report file:** `/Users/jason/Development/claude_proj/ewa-poc/.superpowers/sdd/task-5-report.md`

---

## Context

This is the final task in a 5-task SDD build of a single-file EWA POC flow (4 screens: Bank Account Question → EWA Landing → Processing Animation → Offer Match Cards). Tasks 1–4 are complete. The file is fully built. Task 5 is a QA + polish pass — find and fix real issues in the code.

You cannot open a browser. Do a code-level audit instead: read the full file, check CSS values, check JS logic, and apply the known fixes listed below. Do not simulate browser behavior — if a check requires a browser and you cannot verify it from the code, say so in the report.

---

## Known Issues to Fix (carry-forwards from prior task reviews)

### Fix 1 — `.ring-icon` missing explicit `opacity: 1`

The `.ring-icon` CSS rule controls the processing ring's center icon (Screen 3). The `initProcessingScreen()` JS fades it to `opacity: 0` then back up via inline style, but if the CSS rule does not declare `opacity: 1` as the starting state, the transition may be undefined across browsers.

**Find this rule:**
```css
.ring-icon {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%) rotate(90deg);
  font-size: 40px;
  line-height: 1;
  transition: opacity 0.25s ease;
}
```

**Add `opacity: 1;` to it so it reads:**
```css
.ring-icon {
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%) rotate(90deg);
  font-size: 40px;
  line-height: 1;
  opacity: 1;
  transition: opacity 0.25s ease;
}
```

### Fix 2 — Step-0 no-op flicker in `initProcessingScreen()`

The `initProcessingScreen()` function runs 4 steps. Step 0 (delay=0) sets the icon to 🔍 and the message to `processingMessages[0]` — but these are already the initial values rendered in the HTML. This causes a brief flicker: the element fades to 0 and then fades back to what it already showed.

**Current step loop (approximately):**
```js
steps.forEach((step, i) => {
  setTimeout(() => {
    icon.style.opacity = '0';
    msg.style.opacity  = '0';
    setTimeout(() => {
      icon.textContent = step.icon;
      msg.textContent  = step.msg;
      icon.style.opacity = '1';
      msg.style.opacity  = '1';
    }, 130);
  }, step.delay);
});
```

**Fix:** Skip the fade-out/fade-in cycle for step 0 (delay === 0) since it's a no-op:
```js
steps.forEach((step, i) => {
  setTimeout(() => {
    if (i === 0) {
      // Step 0: values already match the HTML default — skip the flicker cycle
      icon.textContent = step.icon;
      msg.textContent  = step.msg;
      return;
    }
    icon.style.opacity = '0';
    msg.style.opacity  = '0';
    setTimeout(() => {
      icon.textContent = step.icon;
      msg.textContent  = step.msg;
      icon.style.opacity = '1';
      msg.style.opacity  = '1';
    }, 130);
  }, step.delay);
});
```

Find the actual implementation in the file and apply the equivalent fix. The exact code may look slightly different — adapt accordingly.

---

## QA Checks to Perform

### Check 1 — Touch targets ≥ 48px

Verify in CSS:
- `#continueBtn` / `.btn-continue`: min-height ≥ 48px
- `.btn-accent`: min-height ≥ 48px
- `.acct-card`: min-height ≥ 48px (currently 90px — confirm)
- `.offer-cta`: min-height ≥ 48px (currently 52px — confirm)

Report findings. Fix any that are under 48px.

### Check 2 — iOS zoom prevention

Confirm `.select-wrap select` has `font-size: 16px` (or larger). If smaller, change it.

### Check 3 — APR / loan language scan

Search the full file for these strings. Flag any in Screen 2, 3, or 4 body copy (logo alt text "Fast Loan Advance" in Screen 1 is exempt):
- "APR"
- "interest rate" (note: Albert's bullet "No interest ever" is acceptable — it's a benefit claim, not a rate disclosure — do NOT remove it)
- "%" used as a financial rate (star ratings showing "4.8" are fine; "%25 APR" would not be)
- "apply" (as in "apply for a loan")
- "approval" (as in "loan approval")
- "credit score"
- "loan" (in Screen 2/3/4 body copy — not in page title or logo alt)

Report each finding with line number. Fix real violations.

### Check 4 — Accessibility: keyboard + ARIA on account cards

The `.acct-card` elements use `onclick` but are `<div>` elements, which are not keyboard-accessible by default. Add the following to the file's `<script>` block (after all function definitions, before the closing `</script>`):

```js
// Keyboard accessibility for account cards
document.querySelectorAll('.acct-card').forEach(card => {
  card.setAttribute('tabindex', '0');
  card.setAttribute('role', 'button');
  card.addEventListener('keydown', e => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      card.click();
    }
  });
});
```

### Check 5 — Heading hierarchy

Verify:
- Screen 1: `<h1>` exists with "What type of bank account do you have?"
- Screen 2: `<h2>` exists with "No bank? No problem."
- Screen 4: `<h2>` exists with "Your matched offers"
- Logo `<img>` has `alt="Fast Loan Advance"`

Report findings. Fix any missing.

### Check 6 — No horizontal overflow at 320px

Review the CSS for any rules that could cause horizontal overflow at 320px:
- Fixed widths wider than 320px
- No `overflow-x: hidden` on the outer container (`.flow-wrap` or `body`)
- The SVG ring: `viewBox="0 0 180 180"` with `width: 180px` — on 320px viewport with 32px side padding (16px each), available width is 288px, so 180px ring is fine.
- The `.cards-grid` two-column layout: check the card min-width. If the grid could squeeze below 120px per card on 320px screens (320 - 32px padding = 288px / 2 cols = 144px), it should be fine. Confirm.

If you find a real overflow risk, fix it with `max-width: 100%` or `min-width: 0`.

### Check 7 — Progress bar state on initial load

Confirm:
- `#progressFill` starts at `width: 75%` with green `background: #4a9c3f` (or via CSS)
- `transitionToScreen()` switches to `var(--accent)` color from screen2 onward

---

## Output Requirements

Write your full report to: `/Users/jason/Development/claude_proj/ewa-poc/.superpowers/sdd/task-5-report.md`

Report format:
```markdown
# Task 5 Report — QA Pass

## Fixes Applied
[List each fix: what was wrong, what was changed, line reference]

## QA Check Results
[Each check: ✅ Pass / ⚠️ Fixed / ❌ Fail (with line reference and what was done)]

## Deviations
[Any deviations from this brief, or "None"]

## Self-Review
[Any issues you noticed beyond the spec that were or were not fixed]
```

Return only: `DONE` or `DONE_WITH_CONCERNS: [brief description]`
