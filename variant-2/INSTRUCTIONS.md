# EWA POC — Variant 2: Warm & Friendly Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single-file, fully interactive 4-screen EWA offer flow (Warm & Friendly aesthetic — indigo/purple, very rounded, soft shadows) in vanilla HTML/CSS/JS.

**Architecture:** One `index.html` with embedded `<style>` and `<script>`. No build step, no external JS libraries. All 4 screens live in the DOM simultaneously; `.active` class controls visibility. Screen transitions use CSS opacity/transform. Logo referenced as `../media/logo.svg`. Offer data is hardcoded JS objects (POC — no API).

**Tech Stack:** HTML5, CSS3 (custom properties, SVG, keyframes), Vanilla JS ES6+, Google Fonts (Montserrat via `@import`)

## Global Constraints

- Mobile-first, max-width `480px` centered on desktop, full-bleed on mobile
- No external JS libraries or frameworks — vanilla only
- **No APR, interest rates, percentages, or loan language** anywhere on Screen 2, 3, or 4
- Pre-fill simulated via JS variables: `firstName: "Jason"`, `email: "jason@example.com"`
- Processing animation minimum **2.5 seconds** (2800ms before transition to Screen 4)
- All CTA buttons minimum **48px height** (touch target requirement)
- Input/select font-size minimum **16px** (prevents iOS zoom on focus)
- Logo: `../media/logo.svg` (relative path from `variant-2-warm-friendly/index.html`)
- Screen 1 uses brand green `#4a9c3f` for progress bar and Continue button (matches live adchain form)
- From Screen 2 onward, accent color switches to indigo `#6c47ff`
- Design tokens: `--accent: #6c47ff`, `--bg: #f8f7ff`, `--surface: #ffffff`, `--text: #1a1a2e`, `--radius: 20px`
- Offer data: Dave (BEST MATCH badge), Albert, Earnin — same data used in all 4 variants
- Age Range options: 18–24, 25–34, 35–44, 45–54, 55+
- State: full US 50-state dropdown, populated by JS array

---

## File Structure

```
ewa-poc/
├── media/
│   └── logo.svg                          ← already exists, used by all variants
├── variant-2-warm-friendly/
│   ├── INSTRUCTIONS.md                   ← this file
│   └── index.html                        ← single output file (create in Task 1)
└── ewa-poc-plan.md                       ← master POC plan (reference, do not modify)
```

---

### Task 1: File scaffold + Screen 1 (static layout + interactions)

**Files:**
- Create: `ewa-poc/variant-2-warm-friendly/index.html`

**Produces:** A working Screen 1. Selecting "No Bank Account" highlights that card and changes the button label to "Find EWA Options →". Clicking that button fades Screen 1 out and fades Screen 2 in (Screen 2 is a placeholder at this stage). Screens 3 and 4 are also placeholder divs.

---

- [ ] **Step 1: Create `index.html` with the full scaffold**

Create `ewa-poc/variant-2-warm-friendly/index.html` with the following complete content. This is the full starting file — every later task edits specific sections of this file.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Find EWA Options | Get Paid Before Payday</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700;800&display=swap');

    /* ========================
       DESIGN TOKENS
    ======================== */
    :root {
      --bg:           #f8f7ff;
      --surface:      #ffffff;
      --surface-2:    #f0effe;
      --accent:       #6c47ff;
      --accent-light: #ede9ff;
      --text:         #1a1a2e;
      --text-muted:   #6b7280;
      --border:       #e5e0ff;
      --success:      #22c55e;
      --brand-green:  #4a9c3f;
      --radius:       20px;
      --shadow:       0 4px 24px rgba(108,71,255,0.08);
      --font:         'Montserrat', system-ui, sans-serif;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; }

    body {
      font-family: var(--font);
      background: var(--bg);
      color: var(--text);
      min-height: 100vh;
      display: flex;
      justify-content: center;
      align-items: flex-start;
    }

    /* ========================
       FLOW WRAPPER
    ======================== */
    .flow-wrap {
      width: 100%;
      max-width: 480px;
      min-height: 100vh;
      background: var(--surface);
      position: relative;
      overflow: hidden;
      display: flex;
      flex-direction: column;
    }

    /* ========================
       PROGRESS BAR
    ======================== */
    .progress-bar {
      width: 100%;
      height: 6px;
      background: #e0e0e0;
      flex-shrink: 0;
    }
    .progress-fill {
      height: 100%;
      background: var(--brand-green);
      transition: width 0.5s ease, background 0.5s ease;
    }

    /* ========================
       SCREEN MANAGEMENT
    ======================== */
    .screen {
      display: none;
      flex-direction: column;
      flex: 1;
      padding: 24px 20px 32px;
    }
    .screen.active {
      display: flex;
      animation: fadeSlideUp 0.35s ease;
    }
    @keyframes fadeSlideUp {
      from { opacity: 0; transform: translateY(16px); }
      to   { opacity: 1; transform: translateY(0); }
    }

    /* ========================
       SHARED: LOGO
    ======================== */
    .logo-wrap {
      display: flex;
      justify-content: center;
      align-items: center;
      padding: 16px 0 12px;
    }
    .logo-wrap img {
      height: 38px;
      width: auto;
    }

    /* ========================
       SCREEN 1 STYLES
    ======================== */
    .s1-headline {
      font-size: 20px;
      font-weight: 700;
      color: var(--text);
      text-align: center;
      margin: 12px 0 16px;
      line-height: 1.3;
    }

    /* Helpful tip block */
    .tip-block {
      background: #fffbeb;
      border: 1px solid #fde68a;
      border-radius: 12px;
      padding: 12px 14px;
      margin-bottom: 20px;
    }
    .tip-block p {
      font-size: 14px;
      color: #92400e;
      font-style: italic;
      line-height: 1.5;
      margin-bottom: 6px;
    }
    .tip-toggle {
      font-size: 12px;
      font-weight: 600;
      color: #b45309;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      cursor: pointer;
    }

    /* Account card grid */
    .cards-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 10px;
      margin-bottom: 16px;
    }
    .no-bank-card {
      grid-column: 1 / -1; /* full width */
    }

    .acct-card {
      border: 2px solid var(--border);
      border-radius: 16px;
      padding: 18px 12px;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 8px;
      cursor: pointer;
      transition: border-color 0.2s, background 0.2s, transform 0.15s;
      background: var(--surface);
      min-height: 90px;
    }
    .acct-card:hover {
      border-color: var(--accent);
      background: var(--accent-light);
    }
    .acct-card.selected {
      border-color: var(--accent);
      background: var(--accent-light);
      transform: scale(0.985);
    }
    .acct-card .card-icon { font-size: 30px; line-height: 1; }
    .acct-card .card-label {
      font-size: 14px;
      font-weight: 600;
      color: var(--text);
      text-align: center;
    }

    /* Continue / CTA buttons */
    .btn-continue {
      width: 100%;
      padding: 16px;
      min-height: 52px;
      background: var(--brand-green);
      color: #fff;
      font-size: 17px;
      font-weight: 700;
      border: none;
      border-radius: 16px;
      cursor: pointer;
      margin-top: auto;
      transition: opacity 0.2s, transform 0.1s;
      font-family: var(--font);
    }
    .btn-continue:hover  { opacity: 0.9; }
    .btn-continue:active { transform: scale(0.98); }

    /* SSL footer */
    .ssl-footer {
      text-align: center;
      font-size: 12px;
      color: var(--text-muted);
      margin-top: 14px;
      padding-top: 12px;
      border-top: 1px solid #f0f0f0;
    }
    .ssl-footer a { color: var(--text-muted); text-decoration: underline; }
  </style>
</head>
<body>
  <div class="flow-wrap">

    <!-- PROGRESS BAR (always visible) -->
    <div class="progress-bar">
      <div class="progress-fill" id="progressFill" style="width: 75%"></div>
    </div>

    <!-- ========================
         SCREEN 1: Bank Account Question
    ======================== -->
    <div class="screen active" id="screen1">
      <div class="logo-wrap">
        <img src="../media/logo.svg" alt="Fast Loan Advance" />
      </div>

      <h1 class="s1-headline">What type of bank account do you have?</h1>

      <div class="tip-block">
        <p>"Lenders are significantly more likely to fund checking accounts than savings accounts."</p>
        <div class="tip-toggle">A HELPFUL TIP! ∧</div>
      </div>

      <div class="cards-grid">
        <div class="acct-card" id="card-checking" onclick="selectCard('checking')">
          <span class="card-icon">🏦</span>
          <span class="card-label">Checking</span>
        </div>
        <div class="acct-card" id="card-savings" onclick="selectCard('savings')">
          <span class="card-icon">🐷</span>
          <span class="card-label">Savings</span>
        </div>
        <div class="acct-card no-bank-card" id="card-nobank" onclick="selectCard('nobank')">
          <span class="card-icon">👛</span>
          <span class="card-label">No Bank Account</span>
        </div>
      </div>

      <button class="btn-continue" id="continueBtn" onclick="handleContinue()">Continue</button>

      <div class="ssl-footer">
        🔒 We use 256 bit SSL technology to encrypt your data. &nbsp;|&nbsp;
        <a href="#">Disclosures</a>
      </div>
    </div>

    <!-- SCREEN 2: EWA Landing (placeholder — filled in Task 2) -->
    <div class="screen" id="screen2">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 2 — Task 2</p>
    </div>

    <!-- SCREEN 3: Processing (placeholder — filled in Task 3) -->
    <div class="screen" id="screen3">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 3 — Task 3</p>
    </div>

    <!-- SCREEN 4: Offer Match (placeholder — filled in Task 4) -->
    <div class="screen" id="screen4">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 4 — Task 4</p>
    </div>

  </div><!-- /.flow-wrap -->

  <script>
    /* ========================
       SHARED DATA LAYER
    ======================== */
    const userData = {
      firstName: "Jason",
      email:     "jason@example.com"
    };

    const ewaOffers = [
      {
        id:       "dave",
        name:     "Dave",
        icon:     "💚",
        headline: "Get up to $500 before payday",
        bullets:  ["No credit check", "Funds in minutes", "Repays automatically"],
        rating:   "4.8",
        reviews:  "500K+",
        badge:    "YOUR BEST OPTION",
        cta:      "Get My Advance",
        url:      "#"
      },
      {
        id:       "albert",
        name:     "Albert",
        icon:     "💜",
        headline: "Instant cash when you need it",
        bullets:  ["Up to $250 advance", "No interest ever", "Free to try"],
        rating:   "4.7",
        reviews:  "200K+",
        badge:    null,
        cta:      "Try Albert Free",
        url:      "#"
      },
      {
        id:       "earnin",
        name:     "Earnin",
        icon:     "🟠",
        headline: "Access money you've already earned",
        bullets:  ["Up to $100/day", "Lightning Speed delivery", "You choose what to pay"],
        rating:   "4.6",
        reviews:  "300K+",
        badge:    null,
        cta:      "Get Started",
        url:      "#"
      }
    ];

    const processingMessages = [
      "Looking for offers near you...",
      "No bank account? That's fine.",
      "Scanning EWA providers...",
      "Found some great options!"
    ];

    /* ========================
       SCREEN TRANSITIONS
    ======================== */
    const progressMap = { screen1: 75, screen2: 85, screen3: 92, screen4: 100 };

    function transitionToScreen(targetId) {
      const current = document.querySelector('.screen.active');
      // Shrink + fade out
      current.style.cssText = 'display:flex; opacity:0; transform:scale(0.97) translateY(-8px); transition:opacity 0.25s ease,transform 0.25s ease;';
      setTimeout(() => {
        current.classList.remove('active');
        current.style.cssText = '';
        // Show next screen
        const target = document.getElementById(targetId);
        target.classList.add('active');
        // Progress bar
        const fill = document.getElementById('progressFill');
        fill.style.width = progressMap[targetId] + '%';
        // Switch progress color to accent from Screen 2 onward
        if (targetId !== 'screen1') {
          fill.style.background = 'var(--accent)';
        }
      }, 280);
    }

    /* ========================
       SCREEN 1 LOGIC
    ======================== */
    let selectedCard = null;

    function selectCard(type) {
      document.querySelectorAll('.acct-card').forEach(c => c.classList.remove('selected'));
      document.getElementById('card-' + type).classList.add('selected');
      selectedCard = type;
      const btn = document.getElementById('continueBtn');
      btn.textContent = type === 'nobank' ? 'Find EWA Options →' : 'Continue';
    }

    function handleContinue() {
      if (!selectedCard) return;
      if (selectedCard === 'nobank') {
        transitionToScreen('screen2');
      } else {
        alert('Normal adchain continues here. This POC demonstrates the "No Bank Account" branch only.');
      }
    }

    /* Stub functions filled in later tasks */
    function handleScreen2Submit() {}
    function initProcessingScreen() {}
    function buildOfferScreen() {}
  </script>
</body>
</html>
```

- [ ] **Step 2: Open in browser and verify Screen 1**

Open `ewa-poc/variant-2-warm-friendly/index.html` in Chrome (drag to browser or `open index.html` in terminal).

Visual checks:
- Logo centered at top (green Fast Loan Advance logo renders from `../media/logo.svg`)
- Green progress bar at ~75% width
- Headline "What type of bank account do you have?" — bold, centered, dark
- Yellow tip block with italic quote and "A HELPFUL TIP! ∧"
- **Checking + Savings side by side** (2-col), **No Bank Account full-width below** — all same card height and weight
- Cards have emoji icon + label
- Green "Continue" button at bottom (≥ 48px height)
- "🔒 We use 256 bit SSL..." footer

Interaction checks:
- Click "Checking" → card gets purple border + light purple bg, button stays "Continue"
- Click "Savings" → same highlight behavior
- Click "No Bank Account" → card highlights, button label changes to "Find EWA Options →"
- Click "Find EWA Options →" → Screen 1 shrinks/fades, Screen 2 placeholder appears ("Screen 2 — Task 2")
- Click "Continue" after selecting Checking/Savings → alert appears
- Progress bar color switches to indigo after transition

---

### Task 2: Screen 2 — EWA Landing

**Files:**
- Modify: `ewa-poc/variant-2-warm-friendly/index.html`
  - Add CSS block for Screen 2 styles inside `<style>` (before `</style>`)
  - Replace Screen 2 placeholder div content
  - Replace stub `handleScreen2Submit()` function

**Produces:** A fully interactive Screen 2. Pre-filled name/email shown with purple tint + "✓ Pre-filled" checkmark. Age Range and State dropdowns. Form validation highlights empty fields. "Show Me EWA Offers →" button transitions to Screen 3 placeholder. Legal disclosure block at bottom.

---

- [ ] **Step 1: Add Screen 2 CSS — paste before `</style>` in the `<head>`**

```css
    /* ========================
       SCREEN 2 STYLES
    ======================== */
    .s2-icon-cluster {
      font-size: 36px;
      text-align: center;
      letter-spacing: 6px;
      margin-bottom: 10px;
    }
    .s2-headline {
      font-size: 26px;
      font-weight: 800;
      text-align: center;
      margin-bottom: 8px;
      color: var(--text);
      line-height: 1.2;
    }
    .s2-sub {
      font-size: 15px;
      color: var(--text-muted);
      text-align: center;
      margin-bottom: 24px;
      line-height: 1.5;
    }

    /* Field label above inputs */
    .field-label {
      font-size: 12px;
      font-weight: 600;
      color: var(--text-muted);
      text-transform: uppercase;
      letter-spacing: 0.05em;
      margin-bottom: 5px;
    }

    /* Pre-filled row (read-only, purple tint) */
    .field-prefill {
      background: var(--accent-light);
      border: 2px solid var(--accent);
      border-radius: 12px;
      padding: 13px 16px;
      font-size: 16px;
      color: var(--text);
      display: flex;
      align-items: center;
      justify-content: space-between;
      margin-bottom: 14px;
    }
    .prefill-check {
      font-size: 12px;
      font-weight: 700;
      color: var(--accent);
      white-space: nowrap;
      margin-left: 8px;
    }

    /* Custom select wrapper */
    .select-wrap {
      position: relative;
      margin-bottom: 14px;
    }
    .select-wrap select {
      width: 100%;
      padding: 14px 40px 14px 16px;
      font-size: 16px;
      font-family: var(--font);
      border: 2px solid var(--border);
      border-radius: 12px;
      background: var(--surface);
      color: var(--text);
      appearance: none;
      -webkit-appearance: none;
      outline: none;
      cursor: pointer;
      transition: border-color 0.2s;
    }
    .select-wrap select:focus { border-color: var(--accent); }
    .select-wrap select.error  { border-color: #ef4444; }
    .select-wrap::after {
      content: '▾';
      position: absolute;
      right: 16px;
      top: 50%;
      transform: translateY(-50%);
      color: var(--accent);
      pointer-events: none;
      font-size: 16px;
    }

    /* Accent CTA button (indigo) */
    .btn-accent {
      width: 100%;
      padding: 18px;
      min-height: 52px;
      background: var(--accent);
      color: #fff;
      font-size: 17px;
      font-weight: 700;
      border: none;
      border-radius: 16px;
      cursor: pointer;
      font-family: var(--font);
      margin-top: 6px;
      box-shadow: 0 4px 20px rgba(108,71,255,0.25), inset 0 1px 0 rgba(255,255,255,0.15);
      transition: opacity 0.2s, transform 0.1s;
    }
    .btn-accent:hover  { opacity: 0.92; }
    .btn-accent:active { transform: scale(0.98); }

    /* Legal disclosure */
    .legal-divider {
      height: 1px;
      background: var(--border);
      margin: 20px 0 16px;
    }
    .legal-text {
      font-size: 13px;
      color: var(--text-muted);
      line-height: 1.6;
    }
    .legal-text strong { color: var(--text); font-weight: 600; }
    .legal-text p { margin-bottom: 10px; }
    .legal-text p:last-child { margin-bottom: 0; }
```

- [ ] **Step 2: Replace the Screen 2 placeholder div content**

Find this exact block in `index.html`:
```html
    <!-- SCREEN 2: EWA Landing (placeholder — filled in Task 2) -->
    <div class="screen" id="screen2">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 2 — Task 2</p>
    </div>
```

Replace with:
```html
    <!-- ========================
         SCREEN 2: EWA Landing
    ======================== -->
    <div class="screen" id="screen2">
      <div class="logo-wrap">
        <img src="../media/logo.svg" alt="Fast Loan Advance" />
      </div>

      <div class="s2-icon-cluster">💰 ⚡ 💸</div>
      <h2 class="s2-headline">No bank? No problem.</h2>
      <p class="s2-sub">EWA apps are built for exactly this — fast access to your earned pay, no bank account required.</p>

      <!-- Pre-filled: First Name -->
      <div class="field-label">First Name</div>
      <div class="field-prefill">
        <span id="displayName"></span>
        <span class="prefill-check">✓ Pre-filled</span>
      </div>

      <!-- Pre-filled: Email -->
      <div class="field-label">Email</div>
      <div class="field-prefill">
        <span id="displayEmail"></span>
        <span class="prefill-check">✓ Pre-filled</span>
      </div>

      <!-- Age Range -->
      <div class="field-label">Age Range</div>
      <div class="select-wrap">
        <select id="ageRange">
          <option value="">Select age range...</option>
          <option value="18-24">18–24</option>
          <option value="25-34">25–34</option>
          <option value="35-44">35–44</option>
          <option value="45-54">45–54</option>
          <option value="55+">55+</option>
        </select>
      </div>

      <!-- State -->
      <div class="field-label">State</div>
      <div class="select-wrap">
        <select id="stateSelect">
          <option value="">Select state...</option>
        </select>
      </div>

      <button class="btn-accent" onclick="handleScreen2Submit()">Show Me EWA Offers →</button>

      <div class="legal-divider"></div>
      <div class="legal-text">
        <p><strong>What is Early Wage Access?</strong><br>
        Early Wage Access (EWA) services allow you to access a portion of your earned wages before your regular payday. Unlike traditional loans, EWA advances are typically based on hours you've already worked — not your credit score. Most EWA services charge a small optional tip or subscription fee rather than interest.</p>
        <p>By clicking "Show Me EWA Offers," you may be matched with one or more third-party EWA providers. These are independent services and EPCVIP does not guarantee approval, funding amounts, or terms. Availability may vary by state and employer. Please review each provider's terms before signing up.</p>
      </div>
    </div>
```

- [ ] **Step 3: Replace the `handleScreen2Submit` stub in `<script>`**

Find:
```js
    function handleScreen2Submit() {}
```

Replace with:
```js
    function handleScreen2Submit() {
      const age   = document.getElementById('ageRange').value;
      const state = document.getElementById('stateSelect').value;
      let valid = true;

      // Validate — highlight missing dropdowns in red
      const ageEl   = document.getElementById('ageRange');
      const stateEl = document.getElementById('stateSelect');
      ageEl.classList.remove('error');
      stateEl.classList.remove('error');

      if (!age)   { ageEl.classList.add('error');   valid = false; }
      if (!state) { stateEl.classList.add('error'); valid = false; }
      if (!valid) return;

      transitionToScreen('screen3');
      initProcessingScreen();
    }

    /* Populate Screen 2 on page load */
    (function initScreen2() {
      // Pre-fill display
      document.getElementById('displayName').textContent  = userData.firstName;
      document.getElementById('displayEmail').textContent = userData.email;

      // Populate states dropdown
      const US_STATES = [
        'Alabama','Alaska','Arizona','Arkansas','California','Colorado','Connecticut',
        'Delaware','Florida','Georgia','Hawaii','Idaho','Illinois','Indiana','Iowa',
        'Kansas','Kentucky','Louisiana','Maine','Maryland','Massachusetts','Michigan',
        'Minnesota','Mississippi','Missouri','Montana','Nebraska','Nevada',
        'New Hampshire','New Jersey','New Mexico','New York','North Carolina',
        'North Dakota','Ohio','Oklahoma','Oregon','Pennsylvania','Rhode Island',
        'South Carolina','South Dakota','Tennessee','Texas','Utah','Vermont',
        'Virginia','Washington','West Virginia','Wisconsin','Wyoming'
      ];
      const sel = document.getElementById('stateSelect');
      US_STATES.forEach(s => {
        const opt = document.createElement('option');
        opt.value = s;
        opt.textContent = s;
        sel.appendChild(opt);
      });
    })();
```

- [ ] **Step 4: Open in browser and verify Screen 2**

Navigate to Screen 2 by clicking "No Bank Account" then "Find EWA Options →" on Screen 1.

Visual checks:
- Logo at top, same as Screen 1
- 3 emoji icons in a row (💰 ⚡ 💸)
- Bold headline "No bank? No problem."
- Muted subtitle paragraph
- First Name pre-filled with "Jason" — purple-tinted box, "✓ Pre-filled" in purple on the right
- Email pre-filled with "jason@example.com" — same purple-tinted style
- Age Range dropdown with chevron indicator (custom styled, no browser default arrow)
- State dropdown with 50 states populated
- Indigo CTA button with inner glow effect (≥ 52px height)
- Thin divider line
- Legal disclosure text in small muted font, 2 paragraphs

Interaction checks:
- Click "Show Me EWA Offers →" without selecting Age and State → both dropdowns get red border, no transition
- Select an Age and a State → click button → transitions to Screen 3 placeholder
- Progress bar advances to ~92% in indigo color

---

### Task 3: Screen 3 — Processing Animation (SVG Ring)

**Files:**
- Modify: `ewa-poc/variant-2-warm-friendly/index.html`
  - Add CSS for Screen 3 inside `<style>` (before `</style>`)
  - Replace Screen 3 placeholder div content
  - Replace stub `initProcessingScreen()` function

**Produces:** A 2.8-second animated sequence. An SVG circle ring fills from 0→100% in indigo. The center emoji cycles through 🔍 → ⚡ → 🎯 with a fade transition. Status messages fade in and out below the ring. After 2.8 seconds the experience automatically transitions to Screen 4 (placeholder at this stage).

**SVG math:** Ring uses `cx=90 cy=90 r=76` in a `180×180` viewBox. Circumference = 2π×76 = **477.5**. `stroke-dashoffset` starts at `477.5` (fully hidden) and animates to `0` (fully shown).

---

- [ ] **Step 1: Add Screen 3 CSS — paste before `</style>`**

```css
    /* ========================
       SCREEN 3 STYLES
    ======================== */
    .s3-wrap {
      flex: 1;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      gap: 28px;
      text-align: center;
      padding: 20px;
    }

    .ring-container {
      position: relative;
      width: 180px;
      height: 180px;
    }
    /* SVG is rotated -90deg so the fill starts at 12 o'clock */
    .ring-container svg {
      width: 180px;
      height: 180px;
      transform: rotate(-90deg);
    }
    .ring-icon {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 48px;
      line-height: 1;
      transition: opacity 0.25s ease;
      /* Counter-rotate so emoji is upright (SVG is rotated -90deg) */
      rotate: 90deg;
    }

    .s3-msg {
      font-size: 18px;
      font-weight: 600;
      color: var(--text);
      transition: opacity 0.25s ease;
      min-height: 28px;
    }
```

- [ ] **Step 2: Replace the Screen 3 placeholder div content**

Find:
```html
    <!-- SCREEN 3: Processing (placeholder — filled in Task 3) -->
    <div class="screen" id="screen3">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 3 — Task 3</p>
    </div>
```

Replace with:
```html
    <!-- ========================
         SCREEN 3: Processing Animation
    ======================== -->
    <div class="screen" id="screen3">
      <div class="s3-wrap">
        <div class="ring-container">
          <svg viewBox="0 0 180 180" xmlns="http://www.w3.org/2000/svg">
            <!-- Track (background ring) -->
            <circle cx="90" cy="90" r="76" fill="none"
              stroke="#e5e0ff" stroke-width="10" />
            <!-- Animated progress ring -->
            <circle cx="90" cy="90" r="76" fill="none"
              stroke="#6c47ff" stroke-width="10"
              stroke-linecap="round"
              stroke-dasharray="477.5"
              stroke-dashoffset="477.5"
              id="svgRing"
              style="transition: stroke-dashoffset 0.6s ease;" />
          </svg>
          <div class="ring-icon" id="ringIcon">🔍</div>
        </div>
        <div class="s3-msg" id="s3Msg">Looking for offers near you...</div>
      </div>
    </div>
```

- [ ] **Step 3: Replace the `initProcessingScreen` stub in `<script>`**

Find:
```js
    function initProcessingScreen() {}
```

Replace with:
```js
    function initProcessingScreen() {
      const circumference = 477.5;
      const ring   = document.getElementById('svgRing');
      const icon   = document.getElementById('ringIcon');
      const msgEl  = document.getElementById('s3Msg');

      // Reset ring to 0%
      ring.style.strokeDashoffset = circumference;

      const steps = [
        { pct: 20,  msg: processingMessages[0], icon: '🔍', delay: 0    },
        { pct: 52,  msg: processingMessages[1], icon: '⚡', delay: 800  },
        { pct: 80,  msg: processingMessages[2], icon: '⚡', delay: 1500 },
        { pct: 100, msg: processingMessages[3], icon: '🎯', delay: 2200 },
      ];

      steps.forEach(step => {
        setTimeout(() => {
          // Advance the ring
          ring.style.strokeDashoffset = circumference * (1 - step.pct / 100);

          // Fade out icon → swap → fade in
          icon.style.opacity = '0';
          setTimeout(() => {
            icon.textContent  = step.icon;
            icon.style.opacity = '1';
          }, 130);

          // Fade out message → swap → fade in
          msgEl.style.opacity = '0';
          setTimeout(() => {
            msgEl.textContent  = step.msg;
            msgEl.style.opacity = '1';
          }, 130);
        }, step.delay);
      });

      // Transition to Screen 4 after minimum 2.8 seconds
      setTimeout(() => {
        buildOfferScreen();
        transitionToScreen('screen4');
      }, 2800);
    }
```

- [ ] **Step 4: Open in browser and verify Screen 3**

Flow through: Screen 1 → "No Bank Account" → "Find EWA Options →" → Screen 2 → select Age + State → "Show Me EWA Offers →" → Screen 3.

Visual checks:
- Centered layout, ring + message visible
- SVG circle ring: light purple track ring, indigo fill ring starts at 12 o'clock
- Center emoji 🔍 is upright (not sideways — the counter-rotation CSS handles this)
- Message "Looking for offers near you..." displayed below ring

Animation checks:
- Ring fills smoothly from 0% → 20% → 52% → 80% → 100%
- Emoji fades: 🔍 → ⚡ (at 800ms) → stays ⚡ (at 1500ms) → 🎯 (at 2200ms)
- Messages fade out and in as they change
- After ~2.8 seconds, transitions automatically to Screen 4 placeholder

---

### Task 4: Screen 4 — Offer Match Cards

**Files:**
- Modify: `ewa-poc/variant-2-warm-friendly/index.html`
  - Add CSS for Screen 4 inside `<style>` (before `</style>`)
  - Replace Screen 4 placeholder div content
  - Replace stub `buildOfferScreen()` function

**Produces:** Three EWA offer cards rendered from the `ewaOffers` data array. Dave card gets the "YOUR BEST OPTION" badge and a purple left-border accent. Cards animate in with a staggered 120ms slide-up. CTA buttons are indigo. Footer trust bar at bottom.

**Note on innerHTML safety:** `buildOfferScreen()` uses template literals to render `ewaOffers` data via `innerHTML`. This is acceptable for this POC because `ewaOffers` is hardcoded in the JS — it is not user-supplied data and carries no XSS risk here.

---

- [ ] **Step 1: Add Screen 4 CSS — paste before `</style>`**

```css
    /* ========================
       SCREEN 4 STYLES
    ======================== */
    .s4-header {
      margin-bottom: 18px;
      flex-shrink: 0;
    }
    .s4-headline {
      font-size: 22px;
      font-weight: 800;
      color: var(--text);
      margin-bottom: 3px;
    }
    .s4-sub {
      font-size: 14px;
      color: var(--text-muted);
    }

    /* Scrollable offers area */
    .offers-list {
      flex: 1;
      overflow-y: auto;
      -webkit-overflow-scrolling: touch;
    }

    /* Offer card */
    .offer-card {
      background: var(--surface);
      border: 2px solid var(--border);
      border-radius: var(--radius);
      padding: 20px;
      margin-bottom: 12px;
      /* Start hidden for stagger animation */
      opacity: 0;
      transform: translateY(20px);
      transition: opacity 0.4s ease, transform 0.4s ease;
    }
    .offer-card.best-match {
      border-color: var(--accent);
      border-left: 5px solid var(--accent);
      box-shadow: var(--shadow);
    }
    .offer-card.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* Badge */
    .offer-badge {
      display: inline-block;
      background: var(--accent);
      color: #fff;
      font-size: 11px;
      font-weight: 700;
      letter-spacing: 0.06em;
      padding: 4px 12px;
      border-radius: 99px;
      margin-bottom: 14px;
    }

    /* App identity row */
    .offer-top {
      display: flex;
      align-items: center;
      gap: 12px;
      margin-bottom: 12px;
    }
    .offer-icon     { font-size: 38px; line-height: 1; }
    .offer-name     { font-size: 16px; font-weight: 700; }
    .offer-rating   { font-size: 13px; color: var(--text-muted); margin-top: 2px; }
    .offer-headline {
      font-size: 19px;
      font-weight: 700;
      color: var(--text);
      margin-bottom: 10px;
      line-height: 1.3;
    }

    /* Bullets */
    .offer-bullets  { margin-bottom: 16px; }
    .offer-bullet {
      font-size: 14px;
      color: var(--text-muted);
      margin-bottom: 5px;
      display: flex;
      align-items: center;
      gap: 7px;
    }
    .offer-bullet::before {
      content: '✓';
      color: var(--success);
      font-weight: 700;
      flex-shrink: 0;
    }

    /* CTA link */
    .offer-cta {
      display: block;
      text-align: center;
      background: var(--accent);
      color: #fff;
      padding: 15px;
      border-radius: 14px;
      font-weight: 700;
      font-size: 16px;
      text-decoration: none;
      transition: opacity 0.2s;
      min-height: 52px;
      line-height: 1.3;
    }
    .offer-cta:hover { opacity: 0.9; }

    /* Trust footer */
    .s4-trust {
      flex-shrink: 0;
      text-align: center;
      font-size: 12px;
      color: var(--text-muted);
      margin-top: 8px;
      padding-top: 14px;
      border-top: 1px solid var(--border);
    }
```

- [ ] **Step 2: Replace the Screen 4 placeholder div content**

Find:
```html
    <!-- SCREEN 4: Offer Match (placeholder — filled in Task 4) -->
    <div class="screen" id="screen4">
      <p style="color:var(--text-muted); margin:auto; text-align:center;">Screen 4 — Task 4</p>
    </div>
```

Replace with:
```html
    <!-- ========================
         SCREEN 4: Offer Match
    ======================== -->
    <div class="screen" id="screen4">
      <div class="s4-header">
        <h2 class="s4-headline">Your matched offers</h2>
        <p class="s4-sub">Based on your profile — updated today</p>
      </div>
      <div class="offers-list" id="offersList"></div>
      <div class="s4-trust">
        🔒 256-bit SSL · We never share your data without consent
      </div>
    </div>
```

- [ ] **Step 3: Replace the `buildOfferScreen` stub in `<script>`**

Find:
```js
    function buildOfferScreen() {}
```

Replace with:
```js
    function buildOfferScreen() {
      const list = document.getElementById('offersList');

      list.innerHTML = ewaOffers.map(o => `
        <div class="offer-card ${o.badge ? 'best-match' : ''}" id="offer-${o.id}">
          ${o.badge ? `<div class="offer-badge">${o.badge}</div>` : ''}
          <div class="offer-top">
            <span class="offer-icon">${o.icon}</span>
            <div>
              <div class="offer-name">${o.name}</div>
              <div class="offer-rating">★ ${o.rating} · ${o.reviews} reviews</div>
            </div>
          </div>
          <div class="offer-headline">${o.headline}</div>
          <div class="offer-bullets">
            ${o.bullets.map(b => `<div class="offer-bullet">${b}</div>`).join('')}
          </div>
          <a href="${o.url}" class="offer-cta">${o.cta} →</a>
        </div>
      `).join('');

      // Staggered entrance — each card appears 120ms after the previous
      ewaOffers.forEach((o, i) => {
        setTimeout(() => {
          const card = document.getElementById('offer-' + o.id);
          if (card) card.classList.add('visible');
        }, 100 + i * 120);
      });
    }
```

- [ ] **Step 4: Run the complete flow and verify Screen 4**

Flow through all 4 screens end-to-end.

Visual checks:
- "Your matched offers" headline + "Based on your profile — updated today" subtitle
- Three offer cards render in order: Dave, Albert, Earnin
- Dave card has purple left border (5px) + subtle shadow + "YOUR BEST OPTION" indigo pill badge
- Albert and Earnin cards have lighter border, no badge
- Each card: emoji icon, app name, star rating + review count, bold headline, 3 bullet points with green ✓, indigo CTA button
- No APR language, no percentages, no interest rates anywhere
- Cards slide up and fade in with ~120ms stagger (Dave first, then Albert, then Earnin)
- Trust bar at bottom: "🔒 256-bit SSL · We never share your data without consent"

---

### Task 5: QA Pass — Mobile, Accessibility, Final Polish

**Files:**
- Modify: `ewa-poc/variant-2-warm-friendly/index.html` (targeted fixes only)

**Produces:** A QA-verified build ready for internal review. All screens checked at 375px and 320px. Touch targets confirmed. Accessibility basics confirmed. No regression from prior tasks.

---

- [ ] **Step 1: Test at 375px viewport (iPhone SE / standard mobile)**

In Chrome DevTools → Toggle Device Toolbar → set width to 375px.

Check each screen:
- **Screen 1:** No horizontal scroll. All 3 cards visible and correctly sized. "Find EWA Options →" button not clipped. SSL footer visible.
- **Screen 2:** Pre-filled rows readable. Both dropdowns full-width. CTA button visible without scrolling (may need to scroll to legal, that's fine — CTA must be visible).
- **Screen 3:** Ring and message visible without scroll. Ring fits within 375px width.
- **Screen 4:** Offer cards full-width. CTA buttons not clipped. Trust bar visible.

- [ ] **Step 2: Test at 320px viewport (smallest common mobile)**

Set DevTools width to 320px. Repeat checks from Step 1. Common issues to watch for:
- Card grid breaking (Checking/Savings overlapping)
- Offer card text overflowing card boundary
- Ring container overflowing

Fix any overflow or clipping before moving on.

- [ ] **Step 3: Verify touch target sizes**

In DevTools, inspect the following elements and confirm their height is ≥ 48px:
- `#continueBtn` (Continue / Find EWA Options button)
- `.btn-accent` (Show Me EWA Offers button)
- `.acct-card` (all 3 account cards — min-height: 90px already set)
- `.offer-cta` (all 3 offer CTAs)

If any button is under 48px, add `min-height: 52px` to its CSS rule.

- [ ] **Step 4: Verify no iOS zoom-on-focus**

All `<select>` elements must have `font-size: 16px` or larger. Confirm the `.select-wrap select` rule reads `font-size: 16px`. If it's smaller, iOS will auto-zoom when the user taps a dropdown.

- [ ] **Step 5: Check for APR / loan language**

Search the entire `index.html` file for these strings (Cmd+F in editor):
- "APR"
- "interest rate"
- "%" (check each — star ratings are fine, financial rates are not)
- "apply"
- "approval"
- "credit score"
- "loan" (only the logo alt text and page title may reference it — not offer cards or Screen 2/3/4 body copy)

Fix any findings.

- [ ] **Step 6: Basic accessibility spot-check**

- The `<h1>` on Screen 1 reads "What type of bank account do you have?" ✓
- The `<h2>` on Screen 2 reads "No bank? No problem." ✓
- The `<h2>` on Screen 4 reads "Your matched offers" ✓
- `<img>` logo has `alt="Fast Loan Advance"` ✓
- All `.acct-card` elements are clickable — confirm they have `cursor: pointer` and respond to keyboard `Enter` key. If not, add `role="button" tabindex="0"` and a `keydown` handler:

```js
// Add after the onclick attributes are wired, inside the initScreen2 IIFE or in a new init call
document.querySelectorAll('.acct-card').forEach(card => {
  card.addEventListener('keydown', e => {
    if (e.key === 'Enter' || e.key === ' ') {
      e.preventDefault();
      card.click();
    }
  });
  card.setAttribute('tabindex', '0');
  card.setAttribute('role', 'button');
});
```

- [ ] **Step 7: Final full-flow smoke test**

Run through the complete flow 3 times:
1. Select "Checking" → alert appears (correct — POC only covers No Bank Account branch)
2. Select "No Bank Account" → "Find EWA Options →" → Screen 2 → skip dropdowns → validation error shows (red borders) → select Age + State → "Show Me EWA Offers →" → processing animation runs for ~2.8s → offer cards appear with stagger
3. Check that "Get My Advance →" (Dave), "Try Albert Free →" (Albert), and "Get Started →" (Earnin) all link to `#` without JS errors

---

## Self-Review Checklist

After implementation, verify against `ewa-poc-plan.md` Variant 2 spec:

| Requirement from spec | Covered by task |
|---|---|
| Screen 1 matches live form (logo, green progress bar, tip block, SSL footer) | Task 1 |
| Three icon cards — 2-across top + No Bank Account full-width below | Task 1 |
| "No Bank Account" highlights in indigo, Continue → "Find EWA Options →" | Task 1 |
| Screen 1→2 transition: shrink + fade (scale 1→0.97 + opacity) | Task 1 (transitionToScreen) |
| Screen 2: emoji icon cluster, "No bank? No problem." headline | Task 2 |
| Pre-filled name/email with purple tint + checkmark | Task 2 |
| Age Range + State custom-styled dropdowns | Task 2 |
| Legal disclosure block (2 paragraphs, exact copy) | Task 2 |
| CTA: full-width indigo button with inner glow | Task 2 |
| Screen 3: SVG circle ring, indigo stroke | Task 3 |
| Center icon cycles 🔍 → ⚡ → 🎯 with fade | Task 3 |
| Status messages fade in/out below ring | Task 3 |
| Minimum 2.5s processing time (2800ms) | Task 3 |
| Screen 4: white cards, purple left-border on best match | Task 4 |
| "YOUR BEST OPTION" badge in indigo pill | Task 4 |
| Staggered 120ms card entrance animation | Task 4 |
| No APR, no rates, no loan language on offer cards | Tasks 2–4 + QA |
| Mobile-first, max-width 480px | Task 1 (base layout) |
| Touch targets ≥ 48px | Task 5 |
| Design tokens: accent #6c47ff, radius 20px, bg #f8f7ff | Task 1 |
| Progress bar: green on Screen 1, switches to indigo Screen 2+ | Task 1 (transitionToScreen) |
