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

