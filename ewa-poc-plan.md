# EWA POC — Implementation Plan
## 4 Mockup Variants: Bank Account Question → No Bank Account → EWA Offer Flow

**Date:** June 2026  
**Project:** EPCVIP EWA Adchain Integration  
**Goal:** 4 visually distinct, fully interactive proof-of-concept pages demonstrating the "No Bank Account" branch → EWA offer flow  
**Audience:** Internal review (Eric, Kyle, team) — pick one direction to build production

---

## The Flow (Same Across All 4 Variants)

Every mockup implements this exact sequence. Only the visual design, tone, and animation style differ.

```
SCREEN 1 — Bank Account Question
  Matches live Fast Loan Advance adchain form (logo, progress bar, tip block, SSL footer)
  Three icon cards: [Checking] [Savings] [No Bank Account]
  → Selecting "No Bank Account" highlights card + changes Continue button label
  → Clicking updated Continue button triggers transition to Screen 2

SCREEN 2 — EWA Landing / Info Collection
  Logo of originating site (top)
  Headline + subtext (EWA pitch, no bank account framing)
  Pre-filled form: First Name, Email (from prior form)
  Two new fields: Age Range (dropdown), State (dropdown)
  CTA button: "Show Me EWA Offers"
  Legal disclosure block (2 short paragraphs, below CTA)
  → Clicking CTA triggers processing screen

SCREEN 3 — Processing Animation
  Animated sequence, 2–15 seconds
  Status messages cycling through
  Some visual "working" element (varies by variant)

SCREEN 4 — Offer Match Screen
  3 EWA offer cards
  Top card flagged as "Best Match" / "Your Best Option"
  Each card: app icon, name, headline, 2–3 bullets, star rating, CTA
  No APR, no percentages, no loan language
```

---

## What's Consistent Across All 4 Mockups

### Screen 1 — Bank Account Question (Must Match Existing Form)
The live adchain form has been reviewed (Fast Loan Advance screenshot). Screen 1 must feel like a native extension of that form — not a jarring new design. Rules that apply to ALL variants:

- **Logo:** Fast Loan Advance logo at top, same position as the live form (centered, top of page)
- **Progress bar:** Green progress bar below the logo, same style as the live form (full-width, green fill, gray track) — frozen at ~75% to imply the user is near the end of the form
- **Headline:** Match the existing form's headline typography — large, centered, dark text. Use: "What type of bank account do you have?"
- **Helpful tip block:** Replicate the italic quote card from the live form: *"Lenders are significantly more likely to fund checking accounts than savings accounts."* with the "A HELPFUL TIP! ∧" toggle label below it
- **Card buttons:** Large tap-friendly cards with icon + label, same card style as the live Checking / Savings buttons. The third card "No Bank Account" matches the same card size and shape — it should not look smaller, different, or like an afterthought
- **"No Bank Account" card icon:** Use a no-bank / wallet icon (e.g. a wallet, or a crossed-out bank symbol) to match the visual language of the checkbook and piggy bank icons already there
- **Continue button:** Keep the green "Continue" button at the bottom — but for Checking and Savings it works as before. "No Bank Account" card selection highlights the card and changes the Continue button to "Find EWA Options →" or similar, before triggering the transition
- **Footer:** Replicate the "We use 256 bit SSL technology to encrypt your data." line and Disclosures link
- **Layout:** White background, mobile-first, max-width ~480px centered on desktop

The accent/brand green from the live form (`#4a9c3f` approx) is used on Screen 1 for the progress bar and Continue button. From Screen 2 onward, each variant introduces its own accent color.

### Shared Rules — All Screens
- Pre-fill simulation: name and email fields arrive pre-populated (use JS variables to simulate)
- Age Range options: 18–24, 25–34, 35–44, 45–54, 55+
- State: full US state dropdown
- 3 offer cards, same offer data, different visual treatment
- "Best Match" badge on card 1 in all variants
- No APR, no rate language anywhere on offer cards or Screen 2
- Mobile-first layout (max-width: 480px centered, or full-bleed mobile)
- Processing screen minimum 2.5 seconds, can run up to configurable max

### Screen 2 — EWA Legal Disclosure Block (Required on All Variants)
Below the "Show Me EWA Offers" CTA button on Screen 2, every variant must include a legal/informational text block. This is not fine print — it's a readable, honest paragraph that tells the user what EWA is before they proceed. Keep it 2 short paragraphs, plain language, no all-caps.

**Legal text to use (can be lightly edited per variant's tone):**

> **What is Early Wage Access?**
> Early Wage Access (EWA) services allow you to access a portion of your earned wages before your regular payday. Unlike traditional loans, EWA advances are typically based on hours you've already worked — not your credit score. Most EWA services charge a small optional tip or subscription fee rather than interest.
>
> By clicking "Show Me EWA Offers," you may be matched with one or more third-party EWA providers. These are independent services and EPCVIP does not guarantee approval, funding amounts, or terms. Availability may vary by state and employer. Please review each provider's terms before signing up.

**Styling:** Smaller font (13–14px), muted text color, separated from the CTA by a thin divider or adequate whitespace. Should not feel alarming — informational, calm, and trustworthy.

---

## Variant 1 — "Sharp & Modern" (Venmo / Wise Energy)

**Aesthetic:** White background, electric blue accent, sharp typography, bold use of negative space — feels like a modern payments app, not a bank  
**Tone:** Confident, direct, no fluff — "this is the smart move"  
**Stack:** Single-file HTML + CSS + Vanilla JS  
**Signature element:** The processing screen uses a typewriter-style text feed — dark text on white, lines print one by one with a blinking cursor, then a bold "✓ 3 offers found" stamps in with a brief scale pop before transitioning out

**Design tokens:**
```
--bg: #ffffff
--surface: #f4f6ff
--surface-2: #eaedff
--accent: #2563eb       /* electric blue */
--accent-light: #dbeafe
--accent-dark: #1d4ed8
--text: #0f172a
--text-muted: #64748b
--border: #e2e8f0
--success: #16a34a
--font-display: 'Inter', system-ui
--font-mono: 'JetBrains Mono', 'Courier New', monospace
--radius: 10px
--shadow: 0 2px 8px rgba(37,99,235,0.10)
```

**Screen 1 — Bank Account Question:**
- Follows the shared Screen 1 spec exactly (Fast Loan Advance logo, green progress bar, helpful tip block, SSL footer)
- Three icon cards in a row: Checking / Savings / No Bank Account — all same size and card style as the live form
- "No Bank Account" card: wallet/no-bank icon, same card weight as the others; selecting it highlights the card border in blue and changes the Continue button label to "Find EWA Options →"
- On click of the updated Continue button: card layout slides up and out (translateY + fade), Screen 2 slides up from below with a 300ms offset

**Screen 2 — EWA Landing:**
- Logo at top, full color on white, breathing room above and below
- Headline in Inter bold, near-black, large ("No bank account? We've got options.")
- Subtext: one calm sentence about EWA being built for exactly this situation
- Pre-filled fields have a blue left-border accent + a small "✓ Pre-filled" label in muted text
- Age + state as clean dropdowns with blue focus ring
- CTA: full-width blue button, white text, bold

**Screen 3 — Processing (Typewriter Feed):**
```
Analyzing your profile...
Checking EWA eligibility...
Scanning 47 providers...
Filtering: no bank account required...
Matching offers to your state...
Ranking by approval speed...
```
Dark text on white. Each line types out character by character (monospace font, blinking cursor). After last line, "✓ 3 offers found for you" stamps in with a quick scale(0.9→1) pop, then fades to offer screen.

**Screen 4 — Offer Cards:**
- White cards with blue left-border accent on best match card
- "BEST MATCH" badge: blue filled pill, top-left of card
- Clean card layout: icon + name + rating row, headline, bullets, CTA
- CTA buttons: solid blue
- Cards animate in with staggered fade+slide-up (100ms apart)

**Build estimate:** 1 Claude Code session, ~2–3 hours  
**File output:** `variant-1-sharp-modern/index.html`

---

## Variant 2 — "Warm & Friendly" (Albert / Chime Energy)

**Aesthetic:** White/light gray background, indigo/purple accent, rounded everything, soft shadows  
**Tone:** Approachable, reassuring, "we've got you" — feels like a wellness or budgeting app  
**Stack:** Single-file HTML + CSS + Vanilla JS  
**Signature element:** The processing screen uses a pulsing circular progress ring with a friendly icon in the center, and the status messages appear below in a sans-serif with a soft fade transition

**Design tokens:**
```
--bg: #f8f7ff
--surface: #ffffff
--surface-2: #f0effe
--accent: #6c47ff       /* indigo/purple */
--accent-light: #ede9ff
--text: #1a1a2e
--text-muted: #6b7280
--border: #e5e0ff
--success: #22c55e
--font: 'Inter', system-ui
--radius: 20px          /* very rounded */
--shadow: 0 4px 24px rgba(108,71,255,0.08)
```

**Screen 1 — Bank Account Question:**
- Follows the shared Screen 1 spec exactly (Fast Loan Advance logo, green progress bar, helpful tip block, SSL footer)
- Three icon cards matching the live form's card style — on mobile, laid out as 2-across top row (Checking + Savings) with "No Bank Account" full-width below, so it reads as the new third option naturally
- "No Bank Account" card: wallet icon; selecting it highlights border in indigo/purple and swaps Continue button to "Find EWA Options →"
- On click: card shrinks and fades (scale 1→0.95 + opacity 0), new screen fades in with slight upward drift

**Screen 2 — EWA Landing:**
- Friendly illustration or icon cluster at top (CSS-drawn or emoji-based, no external deps)
- Logo above headline
- Headline: conversational ("No bank? No problem.")
- Pre-filled fields have a soft purple tint and a checkmark icon indicating they're confirmed
- Age and state dropdowns use custom styled selects (no browser default)
- CTA: full-width purple button with white text and a subtle inner glow

**Screen 3 — Processing (Pulsing Ring):**
- Large SVG circle progress ring, indigo stroke, animates from 0–100%
- Center of ring: friendly icon that changes (🔍 → ⚡ → 🎯)
- Below ring: status messages fade in/out
  - "Looking for offers near you..."
  - "No bank account? That's fine."
  - "Found some great options!"

**Screen 4 — Offer Cards:**
- White cards, purple left-border accent on best match card
- "YOUR BEST OPTION" badge in indigo pill
- Soft card entrance: cards slide up with staggered 120ms delay
- CTA buttons: indigo, full width, rounded

**Build estimate:** 1 Claude Code session, ~2–3 hours  
**File output:** `variant-2-warm-friendly/index.html`

---

## Variant 3 — "Bold & Modern" (Dave / Current Energy)

**Aesthetic:** White background with electric orange/coral accent, bold typography, confident and energetic — warmth without darkness  
**Tone:** Direct, punchy, action-oriented — "Stop waiting, get your money"  
**Stack:** React + Tailwind CSS (single JSX file, deployable as artifact or Vite project)  
**Signature element:** The transition from Screen 1 to Screen 2 uses a full-screen "wipe" effect — an orange bar sweeps across the screen left to right, then the new content reveals behind it. The processing screen uses a large animated dollar amount counting up.

**Design tokens (Tailwind custom theme):**
```
colors:
  bg: '#ffffff'
  surface: '#fff8f4'    /* very faint warm tint */
  surface2: '#fff2ea'
  accent: '#ff6b2b'     /* bold orange */
  accentAlt: '#ff9f57'
  text: '#1a1a1a'
  muted: '#6b7280'
  border: '#ffe0cc'
  gold: '#f59e0b'
```

**Screen 1 — Bank Account Question:**
- Follows the shared Screen 1 spec exactly (Fast Loan Advance logo, green progress bar, helpful tip block, SSL footer)
- Three icon cards in a row matching the live form's style; "No Bank Account" card same size, selecting it highlights in orange and changes Continue to "Find EWA Options →"
- On click of Continue: orange wipe transition — a full-width orange bar sweeps left to right across the screen (CSS clip-path), then Screen 2 content reveals behind it on white

**Screen 2 — EWA Landing:**
- Bold statement headline ("Your bank won't help. We will.")
- Logo top-left, small and clean on white
- Pre-filled name/email display as read-only "confirmed" chips, not editable inputs — feels more like an app handshake
- Age + state as large tap-friendly dropdown rows (mobile optimized)
- CTA: full-width orange button, white text, uppercase bold, slight 3D press effect on click

**Screen 3 — Processing (Dollar Counter):**
- Large number counting up from $0 → $1,500 with fast easing
- Below it: "Searching for your best advance..."
- Background: white with a very subtle warm radial glow (rgba of accent) behind the number
- Small animated arc or spinning ring around the number in orange

**Screen 4 — Offer Cards:**
- Horizontal scroll on mobile (cards side by side, swipeable) OR stacked
- Best match card: full orange top border + warm surface tint, "⭐ BEST FOR YOU" badge
- White card backgrounds, orange CTA buttons
- Clean white page with subtle warm-tinted card borders

**Build estimate:** 1–2 Claude Code sessions, ~3–4 hours (React adds complexity)  
**File output:** `variant-3-bold-modern/` (Vite React project) or single JSX artifact

---

## Variant 4 — "Clean & Trustworthy" (Brigit / SoFi Energy)

**Aesthetic:** Clean white with a teal/green accent, professional but not bank-corporate, lots of white space  
**Tone:** Calm, trustworthy, "we've vetted these for you" — feels like a financial advisor  
**Stack:** Single-file HTML + CSS + Vanilla JS  
**Signature element:** The offer cards on Screen 4 use a "deal reveal" animation — each card flips face-down to face-up (CSS 3D card flip), staggered, like being dealt a hand. The best match card flips first and lands slightly elevated.

**Design tokens:**
```
--bg: #ffffff
--surface: #f9fafb
--surface-2: #f0fdf4
--accent: #0d9488       /* teal */
--accent-light: #99f6e4
--text: #111827
--text-muted: #6b7280
--border: #e5e7eb
--success: #10b981
--font: 'Inter', system-ui
--radius: 10px
--shadow: 0 1px 3px rgba(0,0,0,0.1), 0 4px 12px rgba(0,0,0,0.05)
```

**Screen 1 — Bank Account Question:**
- Follows the shared Screen 1 spec exactly (Fast Loan Advance logo, green progress bar, helpful tip block, SSL footer)
- Three icon cards in a row matching the live form; "No Bank Account" card same weight, selecting it adds a teal border highlight and updates Continue to "Find EWA Options →"
- On click: the green progress bar at the top animates to 100%, then a brief teal page-load bar sweeps across (like a browser navigation indicator), then content crossfades to Screen 2

**Screen 2 — EWA Landing:**
- Logo prominently placed, given breathing room
- Headline: reassuring ("You have options — even without a bank account.")
- Pre-filled fields: shown in a clean "review" format with an edit icon — feels like the app already knows you
- Age + state as clean dropdown fields, teal focus ring
- Small trust block below form: "🔒 Your information is safe. We never share your data."
- CTA: teal filled button, white text

**Screen 3 — Processing (Pulse Dots + Steps):**
- Three dots pulsing in sequence (teal, staggered)
- Beneath that, a vertical step list that checks off as processing progresses:
  ```
  ✓ Profile verified
  ✓ Checking EWA providers
  ○ Matching to your state...  ← currently active, animated spinner
  ○ Ranking by best fit
  ```
- Clean, structured, feels like a real backend doing real work

**Screen 4 — Offer Cards (Card Flip Reveal):**
- Cards start face-down (gray back)
- Flip one by one with 400ms stagger (CSS perspective + rotateY)
- Best match flips first, arrives with teal border and "✓ Best Match for You" badge
- Card face: clean, lots of white space, teal CTA button
- Footer: "Offers updated daily · Results based on your profile"

**Build estimate:** 1 Claude Code session, ~2–3 hours  
**File output:** `variant-4-clean-trustworthy/index.html`

---

## Shared Data Layer (Use in All 4)

All variants pull from the same simulated data so swapping or comparing is clean.

```javascript
// Simulated pre-fill from prior adchain form
const userData = {
  firstName: "Jason",
  email: "jason@example.com"
};

// EWA Offers (same data, different card design per variant)
const ewaOffers = [
  {
    id: "dave",
    name: "Dave",
    icon: "💚",
    headline: "Get up to $500 before payday",
    bullets: ["No credit check", "Funds in minutes", "Repays automatically"],
    rating: "4.8",
    reviews: "500K+",
    badge: "BEST MATCH",
    cta: "Get My Advance",
    url: "#"
  },
  {
    id: "albert",
    name: "Albert",
    icon: "💜",
    headline: "Instant cash when you need it",
    bullets: ["Up to $250 advance", "No interest ever", "Free to try"],
    rating: "4.7",
    reviews: "200K+",
    badge: null,
    cta: "Try Albert Free",
    url: "#"
  },
  {
    id: "earnin",
    name: "Earnin",
    icon: "🟠",
    headline: "Access money you've already earned",
    bullets: ["Up to $100/day", "Lightning Speed delivery", "You choose what to pay"],
    rating: "4.6",
    reviews: "300K+",
    badge: null,
    cta: "Get Started",
    url: "#"
  }
];

// Processing messages (same across variants, display style differs)
const processingMessages = [
  "Analyzing your profile...",
  "Scanning EWA providers...",
  "No bank account? No problem.",
  "Checking availability in your state...",
  "Ranking by fastest approval...",
  "3 offers found for you!"
];
```

---

## Build Order Recommendation

Build in this order for fastest feedback loop:

1. **Variant 2 (Warm & Friendly)** — simplest animations, best for establishing the flow logic
2. **Variant 1 (Sharp & Modern)** — same vanilla stack, just reskin + typewriter animation
3. **Variant 4 (Clean & Trustworthy)** — introduces the card flip, slightly more complex CSS
4. **Variant 3 (Bold & Modern)** — React, do last once flow is proven in vanilla

Each vanilla variant is a single `index.html` file. Variant 3 is a Vite project or React artifact.

---

## Agents & Skills Needed

### Already Built
- ✅ `ewa-specialist` agent
- ✅ `ewa-user-flow.md`
- ✅ `ewa-frontend-poc.md`
- ✅ `ewa-offer-anatomy.md`
- ✅ `ewa-offer-routing.md`
- ✅ Fullstack Dev agent
- ✅ QA agent

### Recommended Additions
- `ewa-mockup-variants.md` *(this plan, as a skill)* — so the build agent knows all 4 variants and doesn't need re-briefing
- Consider a `INSTRUCTIONS.md` per variant for clean Claude Code handoffs

### Claude Code Handoff Structure (per variant)
```
variant-N-[name]/
├── INSTRUCTIONS.md    ← what to build, design tokens, animation spec
├── index.html         ← output file (vanilla) or
├── src/App.jsx        ← output (React variant 3)
└── README.md          ← what was built, how to run
```

---

## Review Criteria

When presenting to Eric/Kyle, evaluate each variant on:

| Criteria | What to look for |
|---|---|
| Does it feel like an app, not a loan form? | No APR language, card-based offers, smooth transitions |
| Does the "No Bank Account" moment feel intentional? | Not a dead end — feels like a feature |
| Processing screen believability | Does it feel like real work is happening? |
| Offer cards: trust vs. urgency balance | User should feel found, not sold |
| Mobile feel | Does it pass the thumb-reach test? |

---

## Next Step

Say **"build variant 1"**, **"build variant 2"**, etc. and the Fullstack Dev agent + EWA Specialist will execute against this plan.

Or say **"build all 4 as artifacts"** and we'll generate all four as interactive Claude artifacts you can preview in-browser right now.
