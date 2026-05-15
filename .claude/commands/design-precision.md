```
╔══════════════════════════════════════════════════════════════════════╗
║  CERT  /design-precision  |  v1.0  |  TIER: 5  |  BUDGET: LEAN     ║
╠═══════════════╦══════════════════════════════════════════════════════╣
║ LAYER SCOPE   ║ L2 · L5                                              ║
║ AUTHORITY     ║ PLANNER                                              ║
║ CAN           ║ - Analyse a Layout Intent spec before execution      ║
║               ║ - Score conversion logic, confusion risk, friction  ║
║               ║ - Assign per-section confidence + overall verdict    ║
║               ║ - BLOCK execution if structural risk is critical     ║
║ CANNOT        ║ - Build any section or component (→ design-layout)  ║
║               ║ - Override audience or goal set in layout-intent     ║
║               ║ - Approve work with a BLOCK-level conversion risk    ║
║ REQUIRES      ║ - Layout Intent spec (from /design-layout-intent)   ║
║               ║ - adapters/design/rules.md loaded                   ║
║               ║ - adapters/design/patterns.md loaded                ║
║ OUTPUTS       ║ - Precision report · risk flags · execution verdict  ║
╚═══════════════╩══════════════════════════════════════════════════════╝
```

Pre-execution precision gate. Runs AFTER `/design-layout-intent` produces a Layout Intent
spec, BEFORE `/design-layout` begins building. Analyses the structural plan for conversion
logic, psychological flow, confusion risk, and structural friction — before any section is
written.

This is the Cortex **P** in U→D→P→E applied to design.

Do not skip this step. A wrong structure costs more to fix after execution than to reject here.

$ARGUMENTS

Parse from $ARGUMENTS:
- `spec` — the Layout Intent spec output from `/design-layout-intent` (paste inline)
- `goal` — conversion goal if not embedded in the spec (optional if spec is complete)

If spec is not provided: ask once. Do not proceed without it.

---

## STEP 1 — Extract the structural decisions

From the provided Layout Intent spec, extract:

```
Page goal      : [stated goal — what the user must achieve]
Audience       : [cold | warm | hot]
Section count  : [N sections in stated order]
Primary CTA    : [label + destination]
Scroll depth   : [short | medium | long]
```

If any of these is absent from the spec: flag the gap immediately.
A precision analysis cannot run on an incomplete spec — list missing fields and stop.

---

## STEP 2 — Conversion logic check

For each section in order, answer:

> "Does this section move the user closer to the primary CTA — or does it distract?"

Apply the audience-specific flow logic:

```
cold   → must follow: ESTABLISH → EXPLAIN → TRUST → ACT
         Any section out of this sequence increases drop-off risk.

warm   → must follow: VALIDATE → DETAIL → ACT
         Any section that re-pitches the product wastes warm users' time.

hot    → must follow: ACT → REASSURE → ACT
         Any section that delays the primary CTA is friction. Flag immediately.
```

For each section output:

```
[N] [section type]  →  ADVANCE | NEUTRAL | DISTRACT
    Reason: [one line — why it does or does not advance the goal]
```

DISTRACT = conversion risk. Flag it.
NEUTRAL alone is not a block, but two consecutive NEUTRALs should be flagged.

---

## STEP 3 — Confusion risk per section

For each section, identify what a first-time user could misunderstand:

```
[N] [section type]
    Risk         : [specific confusion — quote the section's stated purpose if ambiguous]
    Severity     : LOW | MEDIUM | HIGH
    Mitigation   : [one concrete structural change that eliminates the risk]
```

Severity guide:
- HIGH   = user stops, navigates away, or loses trust
- MEDIUM = user hesitates, re-reads, or questions the product
- LOW    = minor friction, unlikely to cause drop on its own

HIGH severity = flag and recommend fix before execution.
Accumulation rule: 3+ LOW = treat as MEDIUM. 2+ MEDIUM = treat as HIGH.

---

## STEP 4 — Structural friction audit

This is NOT a copy audit (that is /design-review R5). This checks structural decisions only.

Answer YES or NO to each gate question:

```
Gate 1 — CTA depth
Does the user scroll past 3+ sections before reaching the primary CTA?
YES → friction. First CTA placement is too deep for this audience.

Gate 2 — Premature commitment
Does any section ask the user to decide before establishing value?
YES → reorder required. Value must precede ask.

Gate 3 — Surplus sections
Is there a section whose removal would not hurt the conversion goal?
YES → surplus. Flag for removal or demotion to a secondary page.

Gate 4 — Audience × scroll depth mismatch
Does the scroll depth match the audience awareness level?
cold + short  → undersells (user not ready to act)
hot  + long   → oversells (user already ready, now frustrated)
Either = YES → structural mismatch.

Gate 5 — CTA label specificity
Is the primary CTA label outcome-specific?
"Submit" / "Get Started" / "Learn More" with no outcome context = YES → copy risk.
(Flag even though copy is not this skill's domain — it is a conversion risk.)
```

For each YES: output a specific structural recommendation.
All NO = no friction flags.

---

## STEP 5 — Confidence score

```
CONVERSION LOGIC   [0–25]   section order drives toward CTA (Step 2)
CONFUSION RISK     [0–25]   no HIGH-severity confusion remaining (Step 3)
FRICTION           [0–25]   all 5 gate questions answered NO (Step 4)
STRUCTURAL FIT     [0–25]   audience × scroll depth × section count aligned
──────────────────────────────────────────────────────
TOTAL              [0–100]
```

Scoring guide per dimension:
- 25    = fully satisfied, no issues
- 18–24 = minor concern, flag but do not block
- 10–17 = clear gap, recommend fix before proceeding
- 0–9   = critical — BLOCK

Total thresholds:
- 90–100 → PROCEED
- 70–89  → PROCEED WITH NOTES (fix flagged items in /design-layout step)
- 50–69  → REVISE SPEC (return to /design-layout-intent with specific fixes listed)
- < 50   → BLOCK (do not build — structural risk is too high to fix in execution)

---

## STEP 6 — Output the Precision Report

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DESIGN PRECISION REPORT — [page type] — [goal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

AUDIENCE      [cold | warm | hot]
SECTIONS      [N sections in order]
PRIMARY CTA   [label + destination]

CONVERSION LOGIC
─────────────────────────────────────────────
[Per-section ADVANCE / NEUTRAL / DISTRACT — Step 2]

CONFUSION RISKS
─────────────────────────────────────────────
[Per-section risk table — Step 3]
[Omit LOW-severity entries unless accumulation rule triggered]

FRICTION FLAGS
─────────────────────────────────────────────
[List every YES gate + structural recommendation]
[If all NO: "No structural friction detected."]

PRECISION SCORE
─────────────────────────────────────────────
Conversion logic   [N]/25
Confusion risk     [N]/25
Friction           [N]/25
Structural fit     [N]/25
────────────────
TOTAL              [N]/100

VERDICT   [PROCEED | PROCEED WITH NOTES | REVISE SPEC | BLOCK]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If BLOCK or REVISE SPEC: list the exact required changes. Do not suggest /design-layout.
If PROCEED WITH NOTES: list flagged items for `/design-layout` to address inline.

---

## Completion block

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
CORTEX  /design-precision               COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Page       {page type} — {goal summary}
Audience   {awareness level}
Score      {N}/100  ·  Verdict: {PROCEED | NOTES | REVISE | BLOCK}
Risks      {N HIGH · N MEDIUM · N LOW}
Friction   {N flags}
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
{if PROCEED}  Next → /design-layout page="{page}" product="{product}" sections="{list}"
{if NOTES}    Fix noted items inline during /design-layout
{if REVISE}   Return to /design-layout-intent — changes required: {list}
{if BLOCK}    Do not proceed. Revise spec and re-run /design-precision.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
