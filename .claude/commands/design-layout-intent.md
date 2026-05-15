```
╔══════════════════════════════════════════════════════════════════════╗
║  CERT  /design-layout-intent  |  v1.0  |  TIER: 5  |  BUDGET: LEAN ║
╠═══════════════╦══════════════════════════════════════════════════════╣
║ LAYER SCOPE   ║ L2 · L5                                              ║
║ AUTHORITY     ║ PLANNER                                              ║
║ CAN           ║ - Produce a Layout Intent spec from a page goal      ║
║               ║ - Determine structure, hierarchy, density, flow      ║
║               ║ - Define section order before any visual work        ║
║               ║ - Flag structural problems before execution begins   ║
║ CANNOT        ║ - Generate section content (use /design-section)     ║
║               ║ - Produce visual output or code                      ║
║               ║ - Override user-stated constraints                   ║
║ REQUIRES      ║ - adapters/design/layout-intent.md loaded            ║
║               ║ - adapters/design/rules.md loaded                    ║
║ OUTPUTS       ║ - Layout Intent spec (canonical format)              ║
║               ║ - Feeds into: /design-section · /design-layout       ║
╚═══════════════╩══════════════════════════════════════════════════════╝
```

Produce a Layout Intent spec — the structural plan for a page before visual
execution begins. Use when building any page with 3+ sections or when the
brief is underspecified and structure decisions need to be made explicit.

$ARGUMENTS

Parse from $ARGUMENTS:
- `page` — page type: `landing` | `product` | `checkout` | `dashboard` | `about` | `pricing` | `onboarding` | `other`
- `goal` — what the user must accomplish or feel (one sentence, outcome-focused)
- `audience` — who arrives on this page and their awareness level: `cold` | `warm` | `hot`
- `constraints` — hard constraints (design system, tech, client rules) — optional

If any required field is missing: ask one focused question before proceeding.
Do not assume. A wrong page goal produces a wrong layout.

---

## STEP 1 — Validate the goal

Read the `goal` argument. Apply this test:

> "Does this goal describe what the USER achieves or feels — or does it describe
>  what the PAGE contains?"

If goal describes page contents ("shows our features and pricing"):
→ Reframe to user outcome before continuing. State the reframe clearly.

Correct: "User understands what the product does and starts a free trial"
Wrong:   "Page shows features, pricing, and a CTA at the bottom"

---

## STEP 2 — Determine user state

From `audience` and `goal`, determine the full user state on arrival:

**Awareness:**
- `cold` — never heard of product. Must establish what it is before anything else.
- `warm` — knows it exists, evaluating. Can skip basic explanation, needs proof.
- `hot`  — ready to act. Page must reduce friction, not sell harder.

**Knowledge:** What does this user already know? (Infer from page type + audience)
**Intent:** What did they come here to do? (One verb phrase)

If awareness is `cold`: the first section MUST be a hero that answers
"what is this and why should I care?" before any features or proof.

If awareness is `hot`: minimize copy. Surface the primary CTA above fold.
Every section adds friction. Remove anything that doesn't accelerate the decision.

---

## STEP 3 — Build the structure

### 3a — Choose container model

From `page` type and `goal`:

```
landing + cold/warm    → contained hero + contained sections (most common)
landing + hot          → full-width hero (high drama, immediate CTA)
product page           → split sections preferred (visual + text alternating)
dashboard              → sidebar+main
checkout/form          → contained form only — no other sections
about                  → contained + split for team/story sections
pricing                → contained grid for plan cards
```

### 3b — Choose flow type

```
linear     → default for marketing, about, product pages
branching  → only if audience segments have meaningfully different needs
             (e.g., pricing page with "Individual / Team / Enterprise" toggle)
modal      → not applicable at page level (modal is a component-level decision)
```

### 3c — Determine scroll depth

```
cold audience + product page  → medium (4–6 sections) — needs education
warm audience + landing page  → short-medium (3–5 sections) — needs proof
hot audience + any page       → short (1–3 sections) — needs action
dashboard / app interior      → depth driven by content, not conversion logic
```

---

## STEP 4 — Define hierarchy

Identify exactly ONE primary element — the single message or action that dominates.

```
Cold landing:   Primary = value proposition headline
Warm landing:   Primary = social proof or key differentiator
Hot landing:    Primary = CTA button (the action IS the message)
Product page:   Primary = product name + core benefit
Dashboard:      Primary = the most time-sensitive data or action
```

Secondary elements (1–3): support or contextualize the primary.
Tertiary: specs, fine print, supporting detail. Never competes visually.

---

## STEP 5 — Order the sections

Use the section order logic from `adapters/design/patterns.md`.
For each section, determine:

- **Above fold?** Only 1 section is above fold on most pages. Occasionally 2 if the hero is very short.
- **Container:** full-width | contained | split | grid (from vocabulary)
- **Density:** sparse | balanced | dense (match to audience awareness + content volume)
- **Purpose:** one sentence — what this section must achieve for the user

**Section order rules by audience:**

```
cold    → hero → features → proof → cta
          (establish → explain → trust → act)

warm    → proof → features → cta
          (validate → detail → act — skip the pitch, they know you)

hot     → cta → minimal proof → cta (repeated)
          (act → reassure → act again — every section removes friction)
```

If `constraints` includes an existing section order: validate it against
the rules above. Flag any section that reduces conversion without clear reason.

---

## STEP 6 — Mobile decisions

For each section, state one specific mobile decision:

```
split sections → state which column stacks on top
grid sections  → state column count change (3-col → 1-col)
dense sections → state whether content collapses (accordion) or truncates
hero sections  → state whether visual hides or moves below text
nav            → state mobile nav pattern (hamburger | bottom bar | none)
```

"Make it responsive" is not a mobile decision. Be specific.

---

## STEP 7 — Output the Layout Intent spec

Use the canonical format from `adapters/design/layout-intent.md` exactly.
No deviation from the format structure. Fill every field.

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
LAYOUT INTENT: [page name from $ARGUMENTS or inferred]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PAGE GOAL
─────────────────────────────────────────────
[Validated goal — reframed if needed in Step 1]

USER STATE ON ARRIVAL
─────────────────────────────────────────────
Awareness     [cold | warm | hot]
Knowledge     [what they already know]
Intent        [what they came to do — one verb phrase]

STRUCTURE
─────────────────────────────────────────────
Container     [choice from Step 3a]
Flow type     [linear | branching | modal]
Scroll depth  [short | medium | long]
Primary CTA   [action + outcome] → [destination]

HIERARCHY
─────────────────────────────────────────────
Primary    [the one dominant element — from Step 4]
Secondary  [supporting elements — 1 to 3]
Tertiary   [contextual detail]

SECTIONS (in order)
─────────────────────────────────────────────
[Built in Step 5 — one block per section]

CONTENT BALANCE
─────────────────────────────────────────────
Visual weight   [image-heavy | text-heavy | balanced]
Social proof    [yes | no]
Trust signals   [yes | no]

MOBILE DECISIONS
─────────────────────────────────────────────
[Specific decisions from Step 6 — one line per section that changes]

CONSTRAINTS
─────────────────────────────────────────────
[From $ARGUMENTS constraints field | NONE]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## STEP 8 — Rule check

Before completing, verify against `adapters/design/rules.md`:
- [ ] R1: Would a new user know immediately what to do on this page?
- [ ] R2: Does each section have exactly one purpose?
- [ ] R5: Is there any section that adds friction without adding value?

If any check fails: fix the layout intent spec before outputting.

---

## Completion block

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CORTEX  /design-layout-intent              COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Page          {page type} — {goal summary}
Audience      {awareness level}
Sections      {N} sections · scroll: {depth}
Hierarchy     {primary element stated}
Rules         {N/3 passed | list any failures}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Next → /design-precision spec="{paste this Layout Intent spec}"
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
