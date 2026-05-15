# /cert-design-system — Business-Adaptive UI/UX Design Engine
# skill: cert-design-system | domain: design | version: 2.0 | added: 2026-04-30
# Classifies the business first, then selects design mode, visual language, and build stack.
# QA gate · Hard Invariants · CORTEX pipeline. Business → UX logic → Design mode → Visuals → Stack.

---

## LOAD

Before executing:
- `adapters/design/vocabulary.md`
- `adapters/design/patterns.md`
- `adapters/design/rules.md`

---

## TRIGGER

Use when:
- Starting any new website, landing page, or product UI from scratch
- Existing design feels generic or "off" for the business type
- Choosing between animation vs static, minimal vs rich, trust vs emotion
- Any session where the wrong design mode was previously applied
- Bridging from product discovery → first UI decisions

Never skip this skill and go straight to `/design-section`.
`/design-section` outputs components. This skill decides what KIND of components.

---

## THE CORE PRINCIPLE

```
Most AI design systems fail because they do this:

  ❌ WRONG:  Visuals first → "make it look good"
             Result: Pretty garbage. Converts nobody.

  ✅ RIGHT:  Business → UX logic → Design mode → Visuals → Stack
             Result: Design that works for the specific business.

A luxury fashion brand site ≠ a B2B SaaS dashboard ≠ a logistics ops portal.
Same component, wrong context = trust destroyed, conversions lost.
```

---

## EXECUTION

### STEP 0 — Business Classification

The mandatory entry gate. Do not design anything without completing this step.

Collect and classify across 5 dimensions:

```
DIMENSION 1 — Industry & Category
  What industry? (e-commerce · SaaS · marketplace · operations · creative · healthcare · fintech · logistics)
  What product type? (tool · store · platform · portfolio · service · community)

DIMENSION 2 — Audience
  Who is the primary user? (founder · ops worker · consumer · enterprise buyer · tailor · pilot · student)
  Tech literacy: (high / medium / low)
  Device split: (mobile-first / desktop-first / equal)
  India-specific? → WhatsApp trust signals + vernacular tolerance + price sensitivity matter

DIMENSION 3 — Trust Requirement
  How much trust must the design earn before conversion?
    HIGH   → regulated, financial, medical, legal, enterprise B2B
    MEDIUM → SaaS tools, marketplaces, operational software
    LOW    → creative, gaming, experimental, entertainment

DIMENSION 4 — Emotional Positioning
  Where on the spectrum?
    Clinical  ←──────────────────────────────────→  Expressive
    (law/medical)  (SaaS/ops)  (fashion/gaming)  (luxury/creative)

DIMENSION 5 — Conversion Goal
  What is the single action the page must produce?
    (Sign up · Book demo · Place order · Download · WhatsApp enquiry · Subscribe · Explore)
```

Output: **Business Design Profile**
```
Industry:          [category]
Audience:          [who, device, literacy]
Trust level:       HIGH / MEDIUM / LOW
Emotional tone:    [position on Clinical → Expressive spectrum, 1–10]
Conversion goal:   [single action]
India context:     YES / NO
```

---

### STEP 1 — UX Blueprint

Given the Business Design Profile, define the information architecture:

```
SITE MAP (hierarchical)
  Home
  └─ [Section: primary value proposition]
  └─ [Section: social proof / trust signals]
  └─ [Section: how it works / product detail]
  └─ [Section: pricing / offer]
  └─ [Section: conversion / CTA]
  └─ [Section: FAQ / objection handling]
  └─ [Optional: secondary pages]

USER JOURNEY (primary path)
  Enters at: [traffic source — organic / referral / ad / WhatsApp]
  Lands on:  [page + what they see in first 3 seconds]
  Question they ask: "Is this for me? Can I trust it? What does it cost?"
  Obstacle:  [most likely reason they leave without converting]
  CTA path:  [the sequence of micro-commitments before the final action]

FUNNEL LOGIC
  Awareness screen:   [headline that answers "is this for me?"]
  Interest screen:    [proof that answers "can I trust this?"]
  Desire screen:      [value that answers "is it worth it?"]
  Action screen:      [CTA that answers "what do I do now?"]

TRUST SIGNAL REQUIREMENTS (by trust level)
  HIGH:   credentials · certifications · named team · case studies · security badges · SLA
  MEDIUM: customer count · testimonials · logos · free trial · money-back
  LOW:    social proof · community size · press mentions
```

---

### STEP 2 — Design Mode Selection

Choose ONE primary mode. Hybrid is almost always right for product businesses.

```
MODE A — Static Trust Mode
  Best for:   Law · medical · enterprise B2B · logistics · government · finance
  Emotional tone score: 1–3
  Characteristics:
    - Zero decorative animation
    - Heavy typography hierarchy
    - High contrast, authoritative palette
    - Dense information, scannable layout
    - Conversion through clarity, not emotion
  Motion rule:  NONE — animation harms trust in this category
  Stack:        Next.js + Tailwind + shadcn/ui (no animation libraries)

MODE B — Dynamic Premium Mode
  Best for:   Fashion · luxury · creative agencies · premium consumer products
  Emotional tone score: 7–10
  Characteristics:
    - Scroll-triggered reveals
    - Polished micro-interactions
    - Generous whitespace, editorial layout
    - Emotion-led typography (large, expressive)
    - Conversion through desire, not logic
  Motion rule:  Framer Motion — scroll reveals + hover states + page transitions
  Stack:        Next.js + Tailwind + Framer Motion

MODE C — Immersive Experience Mode
  Best for:   Gaming · AI tools · innovation brands · experimental products
  Emotional tone score: 8–10 (but rational product)
  Characteristics:
    - Interactive storytelling
    - 3D elements or particle systems
    - Non-linear navigation acceptable
    - Motion IS the product experience
  Motion rule:  GSAP + Three.js — full scene orchestration
  Stack:        Next.js + Tailwind + GSAP + Three.js (or React Three Fiber)
  ⚠ Warning:   Only choose this if immersion IS the product value, not decoration.

MODE D — Hybrid Mode [RECOMMENDED for most product businesses]
  Best for:   SaaS · ops tools · marketplaces · e-commerce · QSR · fintech
  Emotional tone score: 4–7
  Characteristics:
    - Static, fast-loading conversion pages (pricing, signup, docs)
    - Animated hero section and key feature reveals
    - Motion used for guidance, not decoration
    - Stable at scale — fast + engaging without overwhelming
  Motion rule:  Framer Motion for hero + feature sections only.
               All conversion pages (pricing, checkout, forms): NO animation.
  Stack:        Next.js + Tailwind + Framer Motion (selective)
```

**Mode Selection Matrix:**
```
                    HIGH TRUST    MEDIUM TRUST    LOW TRUST
Emotional 1–3       → Mode A       → Mode A        → Mode A
Emotional 4–6       → Mode D       → Mode D        → Mode D
Emotional 7–9       → Mode D       → Mode B        → Mode B
Emotional 10        → Mode D       → Mode C        → Mode C
```

---

### STEP 3 — Visual Language

Output a complete visual specification — the design token layer.

```
TYPOGRAPHY
  Primary font:     [serif / sans / display — specific recommendation]
  Heading scale:    [H1: Xpx / H2: Xpx / H3: Xpx]
  Body:             [size, line height, weight]
  Personality:      [geometric precision / humanist warmth / editorial authority / technical clarity]
  India note:       Ensure chosen font renders correctly for numbers in ₹ formatting

COLOUR PSYCHOLOGY
  Primary:          [hex + emotion it carries: trust / energy / calm / authority / warmth]
  Secondary:        [hex + role: accent / CTA / highlight]
  Neutral:          [background + surface + border range]
  Semantic:         success #_ · warning #_ · error #_
  Rule:             Maximum 3 brand colours on any single page. More = noise.

SPACING SYSTEM
  Base unit:        4px or 8px
  Density mode:
    Tight   → ops tools, data dashboards (4px base)
    Balanced → SaaS, marketplaces (8px base)
    Airy    → luxury, editorial, premium (12px+ base)

COMPONENT LANGUAGE
  Corner radius:    none (corporate) · 4px (professional) · 8px (friendly) · 16px+ (soft/consumer)
  Shadow style:     none · flat · soft · elevated
  Border weight:    hairline (premium) · 1px (neutral) · 2px (bold)
  Icon style:       outline (minimal) · filled (bold) · duotone (premium)

IMAGERY DIRECTION
  Photo style:      [authentic/candid · product-led · abstract · illustrative · none]
  Treatment:        [full colour · monochrome · branded tint · duotone]
  India note:       Use images of Indian faces/contexts if targeting India B2C.
                    Stock photos of Western users destroy authenticity.
```

---

### STEP 4 — Motion & Interaction Spec

Only after mode is selected. If Mode A: output "NO ANIMATION — skip this step."

```
MOTION LEVEL
  None      → Mode A only
  Subtle    → Hover states · focus rings · loading spinners (all modes)
  Selective → Hero entrance · feature reveals · scroll-triggered sections (Mode D)
  Rich      → Page transitions · staggered lists · parallax (Mode B)
  Immersive → Scene-based · interactive · continuous (Mode C)

ANIMATION TRIGGERS (for selective/rich/immersive)
  On load:    [hero entrance — fade up, duration 600ms, ease out-cubic]
  On scroll:  [section reveals — stagger 100ms per element, threshold 20%]
  On hover:   [cards — lift 2px, shadow increase, duration 150ms]
  On click:   [buttons — scale 0.97, duration 80ms]

PURPOSE DECLARATION (required for each animation defined above)
  State the WHY for each trigger:
    guides attention · signals state change · communicates brand premium · aids comprehension
  "It looks nice" is not a valid purpose. If no purpose → remove the animation.

PERFORMANCE RULE
  Every animation must pass:
    □ Runs on GPU (transform/opacity only — no layout-triggering properties)
    □ Disabled automatically for prefers-reduced-motion
    □ Does not block First Contentful Paint
    □ Conversion pages (checkout, forms, pricing, signup): NO animation — stability = trust
```

---

### STEP 5 — Build Stack Recommendation

```
FRAMEWORK:      Next.js 15 (App Router) — default for all product businesses
                Exception: purely static marketing site → Astro

STYLING:        Tailwind CSS v4 — utility-first, zero runtime, India-deployable

COMPONENTS:     shadcn/ui — copy-paste, no runtime dependency, full control
                Exception: Mode B/C — build custom, shadcn defaults feel generic

ANIMATION:
  Mode A:       Nothing
  Mode D:       Framer Motion (selective — wrap only animated components)
  Mode B:       Framer Motion (layout + page transitions)
  Mode C:       GSAP + @gsap/react (scene orchestration) + Three.js if 3D

STATE:          React Query (server state) + Zustand (UI state only if needed)

FONTS:          next/font (self-hosted, no CLS, no Google Fonts latency)

IMAGES:         next/image (WebP auto-conversion, lazy load, CDN-ready)

HOSTING:        Vercel (default) · Railway (if same instance as API) · Cloudflare Pages (edge)
```

---

### STEP 6 — Conversion + QA Review

Mandatory gate before hand-off to `/design-section`. All checks must PASS.

```
UX CHECKS
  □ Is there one clear, prominent CTA on every conversion page?
  □ Is layout mobile-first? (80%+ India traffic is mobile)
  □ Is the navigation friction-free? (≤2 clicks to primary action)
  □ LCP target < 2.5s? (no blocking render, images optimised, no animation on critical path)

BUSINESS CHECKS
  □ Does the visual design match the Business Design Profile from Step 0?
  □ Is the trust level calibration correct?
      (not too clinical for emotional products · not too playful for regulated ones)
  □ Is the conversion path obvious without explanation?
  □ India context applied if applicable?
      (₹ formatting · local face imagery · WhatsApp CTA option · regional language tolerance)

VISUAL CHECKS
  □ Are design tokens consistent across all sections? (no rogue colours or fonts)
  □ Does contrast meet WCAG AA? (4.5:1 for body text · 3:1 for large text / UI components)
  □ Is the design under-designed enough? (complexity kills conversion — when in doubt, remove)

Output: QA PASS ✅  or  QA FAIL ❌ [list each failed item]
A QA FAIL blocks hand-off — resolve every failed item, then re-run checks.
```

---

### FINAL OUTPUT CONTRACT

```
FINAL OUTPUT CONTRACT
──────────────────────────────────────────────────────
Business Design Profile:  [Step 0 — 5 dimensions filled]
UX Blueprint:             [Step 1 — site map + journey + funnel + trust signals]
Design Mode:              [A / B / C / D + matrix reasoning]
Visual Language Spec:     [typography + palette + spacing + components]
Motion Spec:              [level + triggers + purpose declarations, or "NO ANIMATION"]
Build Stack:              [framework + libraries + hosting with reasoning]
QA Review:                [PASS ✅ / FAIL ❌ — failed items listed if any]
First 3 Components:       [in conversion-impact order, handed to /design-section]
Hand-off to:              /design-section [visual spec attached as context]
──────────────────────────────────────────────────────
```

---

### BUSINESS PROFILES — Quick Reference

```
TAILORGRID (Indian tailoring SaaS)
  Profile:    Operations · B2B · Medium trust · Tone 5 · Conversion: Sign up
  Mode:       D — Hybrid
  Typography: Clean geometric sans (Inter or Plus Jakarta Sans)
  Palette:    Deep navy (authority) + amber accent (craft/warmth) + light grey surface
  Density:    Balanced (8px base)
  Motion:     Subtle — hero entrance + feature section reveals only
  Stack:      Next.js + Tailwind + shadcn/ui + Framer Motion (selective)

EXENA INDIA (EV electronics e-commerce)
  Profile:    E-commerce · B2C India · Medium-high trust · Tone 6 · Conversion: Purchase
  Mode:       D — Hybrid
  Typography: Technical sans + mono accents for spec display
  Palette:    Electric green (innovation/EV) + near-black + white
  Density:    Balanced
  Motion:     Selective — product hero + spec reveals. Checkout: NO animation.
  Stack:      Next.js + Tailwind + shadcn/ui + Framer Motion (selective)

EVFLEET (EV fleet ops platform)
  Profile:    Operations platform · B2B · High trust · Tone 3 · Conversion: Demo/onboard
  Mode:       A — Static Trust (with minimal Mode D for marketing site only)
  Typography: Professional sans, dense, high contrast
  Palette:    Dark teal (fleet/tech) + white + slate grey
  Density:    Tight (ops dashboard) · Balanced (marketing pages)
  Motion:     None on app · Subtle on marketing site only
  Stack:      Next.js + Tailwind + shadcn/ui (no animation libraries in app)

YOLK N' SIP (QSR food ordering)
  Profile:    Consumer · B2C · Low-medium trust · Tone 7 · Conversion: Order now
  Mode:       D — Hybrid
  Typography: Friendly rounded sans (Nunito or DM Sans)
  Palette:    Warm yellow (appetite/energy) + rich black + cream white
  Density:    Airy (consumer comfort)
  Motion:     Selective — menu entrance + add-to-order confirmation. Token page: static.
  Stack:      Next.js + Tailwind + Framer Motion (menu/order flow only)
```

---

### OUTPUT FORMAT

Deliver in this sequence:
1. **Business Design Profile** (Step 0 — 5 dimensions filled)
2. **UX Blueprint** (site map + user journey + funnel logic + trust signals)
3. **Design Mode** (which mode + why, with the matrix reasoning)
4. **Visual Language Spec** (typography + colour + spacing + components)
5. **Motion Spec** (level + triggers + purpose declarations, or "NO ANIMATION")
6. **Build Stack** (framework + libraries + hosting with reasoning)
7. **QA Review** (Step 6 — PASS ✅ or FAIL ❌ with items; resolve FAIL before continuing)
8. **FINAL OUTPUT CONTRACT** (filled)

Then hand off to `/design-section` with the Visual Language Spec as context.

---

## HARD INVARIANTS — NEVER BREAK THESE

```
NEVER classify and design in the same step.
  Step 0 must be complete and output reviewed before any visual decision is made.
  Skipping classification is the root cause of 90% of bad AI-generated designs.

NEVER use motion without explicit purpose.
  Every animation in Step 4 must declare WHY it exists:
    guides attention · signals state change · communicates brand premium · aids comprehension
  "It looks nice" is not a purpose. If no clear purpose → remove the animation.

NEVER apply Mode C (Immersive) to high-trust products.
  Law · finance · medical · enterprise B2B cannot afford immersive.
  Immersion signals "experience product" — trust products need clarity and authority, not spectacle.

NEVER optimise aesthetics before the conversion path is defined.
  UX Blueprint (Step 1) locks first. Visual Language (Step 3) comes after Steps 0–1 are complete.
  A beautiful page with a broken funnel is a beautiful failure.

NEVER skip Step 6 QA Review for production-bound designs.
  If this skill is being used for a real deployment (not exploration or ideation),
  a QA FAIL must be resolved before the session ends.
  Shipping a design that fails its own QA is a governance violation.
```

---

## CORTEX INTEGRATION — Recommended Design Pipeline

```
/cortex-intake          → defines product, target user, jobs to be done, business model
/cert-blueprint         → architecture + stack decisions + module map
/cert-design-system     → THIS SKILL — classifies business · selects design mode
                          defines visual language · runs QA gate · delivers OUTPUT CONTRACT
/design-section         → component-by-component execution (uses visual spec from this skill)
/cert-verify            → final output audit before deployment
```

This skill sits at the centre of the design pipeline.
It receives business context from `/cortex-intake` and produces the visual spec that `/design-section` consumes.
Do not run `/design-section` without first completing `/cert-design-system`.

---

## SUBSKILL EVOLUTION ROADMAP

These planned subskills activate when a project's design domain justifies deeper specialisation.
Until they exist, `cert-design-system` covers all modes with its built-in mode system.

```
luxury-design-mode        (Mode B deep-dive)
  Trigger:  fashion · premium consumer · editorial brands
  Adds:     scroll storytelling rules · editorial typography pairings · premium restraint
            checklist · "less is always more" invariants for luxury brands

saas-conversion-mode      (Mode D SaaS-specific)
  Trigger:  SaaS tools · B2B products · marketplaces
  Adds:     pricing page conversion patterns · trial flow design · onboarding UX rules
            · feature announcement templates · churn-reducing trust patterns

ops-dashboard-mode        (Mode A variant)
  Trigger:  ops platforms · data dashboards · fleet management · admin tools
  Adds:     data density rules · table design system · status/badge system
            · strict no-animation invariants · information hierarchy for power users

creative-immersive-mode   (Mode C deep-dive)
  Trigger:  gaming · AI tools · innovation brands · portfolio sites
  Adds:     GSAP scene orchestration patterns · Three.js / React Three Fiber setup
            · WebGL performance rules · non-linear navigation patterns
            · motion storytelling framework
```
