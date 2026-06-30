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

