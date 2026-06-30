# Task 5 Report — QA Pass

## Fixes Applied

### Fix 1 — `.ring-icon` missing `opacity: 1`
**What was wrong:** The `.ring-icon` CSS rule (line 359) lacked an explicit `opacity: 1` declaration. The JS in `initProcessingScreen()` fades the element to `opacity: 0` and then back to `opacity: 1` via inline styles. Without an initial CSS `opacity: 1`, the starting opacity is undefined across some browsers, which can cause the transition to behave unexpectedly.

**What was changed:** Added `opacity: 1;` to the `.ring-icon` rule, between `line-height: 1;` and `transition: opacity 0.25s ease;`.

**Note on deviation:** The actual file uses `rotate: 90deg` (CSS shorthand property) on a separate line rather than embedding the rotation in `transform: translate(-50%, -50%) rotate(90deg)` as shown in the brief. The brief's code example was illustrative — the fix (`opacity: 1`) was applied to the actual rule as-is without modifying the rotation approach.

---

### Fix 2 — Step-0 no-op flicker in `initProcessingScreen()`
**What was wrong:** `steps.forEach` used `step` as the only parameter (no index `i`). Step 0 (delay=0) faded the icon and message to opacity 0 then back, even though the initial HTML values already matched — causing a visible flicker on screen entry.

**What was changed:** Changed `steps.forEach(step => {` to `steps.forEach((step, i) => {` and added an early-return guard at `i === 0` that sets the content directly without triggering the fade cycle. The ring `strokeDashoffset` still advances normally for step 0 (the ring still begins animating immediately — only the text/icon fade is skipped).

---

### Fix 3 — Keyboard accessibility on `.acct-card` elements (Check 4 required fix)
**What was wrong:** The three `.acct-card` `<div>` elements used `onclick` handlers but had no `tabindex`, `role`, or `keydown` handling — making them inaccessible to keyboard-only users.

**What was changed:** Added the keyboard accessibility block at the end of the `<script>` block (before `</script>`):
- `tabindex="0"` — makes cards focusable
- `role="button"` — correct ARIA semantics for interactive divs
- `keydown` handler firing `card.click()` on Enter or Space

---

## QA Check Results

### Check 1 — Touch targets ≥ 48px ✅ Pass (3 of 4 pass; 1 above minimum)

| Element | CSS Rule | min-height | Result |
|---------|----------|-----------|--------|
| `#continueBtn` / `.btn-continue` | line 184 | `52px` | ✅ Pass |
| `.btn-accent` | line 303 | `52px` | ✅ Pass |
| `.acct-card` | line 161 | `90px` | ✅ Pass (well above 48px) |
| `.offer-cta` | line 487 | `52px` | ✅ Pass |

All touch targets meet or exceed the 48px minimum. No fixes required.

---

### Check 2 — iOS zoom prevention ✅ Pass

`.select-wrap select` (line 274) has `font-size: 16px`. iOS Safari only triggers auto-zoom on inputs with `font-size < 16px`. No fix required.

---

### Check 3 — APR / loan language scan ✅ Pass (no violations)

Full scan results:

| Line | Content | Screen | Assessment |
|------|---------|--------|-----------|
| 517 | `alt="Fast Loan Advance"` | Screen 1 logo | ✅ Exempt (logo alt text) |
| 555 | `alt="Fast Loan Advance"` | Screen 2 logo | ✅ Exempt (logo alt text) |
| 523 | "Lenders are significantly more likely..." | Screen 1 tip block | ✅ Acceptable — Screen 1 tip copy, not in Screen 2/3/4 body |
| 602 | "traditional loans" | Screen 2 legal text | ✅ Acceptable — used to contrast/differentiate EWA from loans in educational legal disclosure |
| 602 | "not your credit score" | Screen 2 legal text | ✅ Acceptable — negative/favorable claim ("NOT based on credit score") |
| 602 | "rather than interest" | Screen 2 legal text | ✅ Acceptable — "No interest ever" class of claim; states EWA charges subscriptions/tips not interest |
| 603 | "does not guarantee approval" | Screen 2 legal text | ✅ Acceptable — a disclaimer that EPCVIP makes no approval guarantee, not a loan approval promise |

**No APR, interest rates expressed as percentages, "apply for a loan," or financial rate "%" found anywhere in Screen 2/3/4 body copy.**

Albert's bullet "No interest ever" (line 672 in offer data) — confirmed acceptable per the brief's explicit exemption ("No interest ever" is a benefit claim, not a rate disclosure).

No violations found. No fixes required.

---

### Check 4 — Accessibility: keyboard + ARIA on account cards ⚠️ Fixed

`.acct-card` elements were non-interactive `<div>` elements with `onclick` but no keyboard support. Fixed by adding the keyboard accessibility block at the end of the `<script>` section (see Fix 3 above).

---

### Check 5 — Heading hierarchy ✅ Pass

| Check | Element | Line | Result |
|-------|---------|------|--------|
| Screen 1 `<h1>` | `<h1 class="s1-headline">What type of bank account do you have?</h1>` | 520 | ✅ Present |
| Screen 2 `<h2>` | `<h2 class="s2-headline">No bank? No problem.</h2>` | 559 | ✅ Present |
| Screen 4 `<h2>` | `<h2 class="s4-headline">Your matched offers</h2>` | 637 | ✅ Present |
| Logo `alt` | `alt="Fast Loan Advance"` | 517, 555 | ✅ Present (both instances) |

No fixes required.

---

### Check 6 — No horizontal overflow at 320px ✅ Pass

- `.flow-wrap` uses `width: 100%; max-width: 480px; overflow: hidden` — no fixed width wider than the viewport.
- No fixed-width containers found that would exceed 320px. The grep for fixed widths (other than 100%, 480px max-width, and 180px ring) returned no hits.
- SVG ring: `width: 180px` on a 320px viewport with 20px side padding (per `.screen` padding: `24px 20px`) = 280px available. 180px fits comfortably.
- `.cards-grid` two-column layout: 280px available ÷ 2 cols − 10px gap = ~135px per card. The `.acct-card` has no `min-width` set, so it will naturally shrink to fill the grid cell. No overflow risk.
- `overflow: hidden` on `.flow-wrap` (line 50) is a clip-guard, not a symptom — it clips any accidental overflow but doesn't cause it.
- No `overflow-x` issues detected in code review.

No fixes required.

---

### Check 7 — Progress bar state on initial load ✅ Pass

- `#progressFill` inline style: `style="width: 75%"` (line 509) — starts at 75% on load. ✅
- `.progress-fill` CSS (line 66): `background: var(--brand-green)` = `#4a9c3f`. ✅
- `transitionToScreen()` (lines 721–723): `if (targetId !== 'screen1') { fill.style.background = 'var(--accent)'; }` — switches to accent purple from Screen 2 onward. ✅

No fixes required.

---

## Deviations

1. **Fix 1 — rotation approach preserved:** The brief's illustrative CSS for `.ring-icon` showed `transform: translate(-50%, -50%) rotate(90deg)` (rotation embedded in transform). The actual file uses separate `transform: translate(-50%, -50%)` and `rotate: 90deg` (CSS shorthand property). Both achieve the same visual result. Only `opacity: 1` was added; the rotation approach was not changed, as altering it could break the visual layout.

2. **Fix 2 — ring still advances at step 0:** The brief's fix example only showed skipping the icon/message fade, but did not address `strokeDashoffset`. In the actual code, the ring advance call (`ring.style.strokeDashoffset = ...`) runs before the `i === 0` guard, so the ring still begins animating to 20% immediately on screen entry. This is intentional and correct behavior — keeping it.

---

## Self-Review

1. **`offer-cta` vertical centering:** `.offer-cta` has `min-height: 52px` and `line-height: 1.3` but no `display: flex; align-items: center`. For single-line CTAs this is fine (padding handles it), but if a CTA text wraps, the content may not be vertically centered. Low risk given the short CTA strings in the data. Left as-is — not a spec item.

2. **Screen 3 has no heading:** Screen 3 (processing animation) has no `<h2>` or ARIA `aria-live` region for the status message. The `#s3Msg` updates dynamically but is not announced to screen readers. This is a real accessibility gap — but not listed in the brief's checks and fixing it would require adding `aria-live="polite"` to `#s3Msg`. Left as-is per brief scope; flagged for future pass.

3. **`tip-block` is non-interactive:** The `.tip-toggle` div has `cursor: pointer` but no `onclick`, `tabindex`, or keyboard handler. Appears to be placeholder UI with no implementation. Left as-is — not in scope.

4. **Logo SVG path:** Both logo `<img>` tags use `src="../media/logo.svg"` which is a relative path assuming the media folder exists at that path. Cannot verify without filesystem access — noted as a deploy concern.
