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

