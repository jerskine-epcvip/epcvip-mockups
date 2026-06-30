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

