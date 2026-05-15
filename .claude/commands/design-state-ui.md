╔══════════════════════════════════════════════════════════════════════╗
║  CORTEX  /design-state-ui  |  v1.0  |  TIER: 2  |  BUDGET: LEAN   ║
╠═══════════════╦══════════════════════════════════════════════════════╣
║ DOMAIN        ║ design                                               ║
║ AUTHORITY     ║ BUILDER                                              ║
║ CAN           ║ - Design UI that reflects system state correctly    ║
║               ║ - Map state machine states → UI screens/components  ║
║               ║ - Define contextual action rules per state pair     ║
║               ║ - Identify edge-case UI (rejection loops, failures) ║
║ CANNOT        ║ - Write implementation code (→ /cert-feature)       ║
║               ║ - Override system state logic                       ║
║               ║ - Choose styling or visual tokens                   ║
║ REQUIRES      ║ - State machine definition from BRAIN.md or PRD     ║
║ PAIRED WITH   ║ /design-component · /cortex-design-handoff          ║
╚═══════════════╩══════════════════════════════════════════════════════╝

/design-state-ui v1.0 — Design UI that reflects system state correctly.
The core law: never design "pretty screens". Design state-aware screens.

---

## THE CORE LAW

```
❌ Wrong prompt: "Design a nice UI for the order screen"
   → Result: pretty screens, useless system

✅ Right prompt: "Design UI that reflects system state correctly"
   → Result: screens that enforce state, prevent confusion, guide user action
```

A UI is only correct if:
1. Every system state has a distinct visual representation
2. State transitions drive what the user can do (contextual actions)
3. Parallel state machines are shown SEPARATELY — never merged into one display

---

## INPUT PARSING

Parse from the user's message:
- `<entity>` — the domain entity with state (e.g. Order, Booking, Dispute)
- `<lifecycle-states>` — primary state machine (e.g. PLACED → ACCEPTED → IN_PROGRESS)
- `<recovery-states>` — secondary/parallel state machine (optional, e.g. NONE → DISPUTED → RESOLVED)
- `<actor>` — who sees this UI: `customer` | `vendor` | `ops` | `admin`
- `--screens` — if provided, scope output to specific screen names
- `--edge-cases` — if provided, also design rejection loops and failure states

---

## STEP 1 — State Map

Build the full state map for the entity:

```
STATE MAP — <entity>
──────────────────────────────────────────────────
PRIMARY (lifecycle):
  <state1> → <state2> → <state3> → ...
  Terminal states: <FINAL_STATE_A> · <FINAL_STATE_B>

SECONDARY (recovery / parallel):
  <state1> → <state2> → <state3> → ...
  Terminal states: <FINAL_STATE_A>

RULE: These two machines run in PARALLEL. Never merge into one display.
```

---

## STEP 2 — Screen Inventory

For each actor, list the screens needed:

```
SCREENS — <actor>
──────────────────────────────────────────────────
1. [Entity] List screen       → shows state badges, entry to detail
2. [Entity] Detail screen     → primary state + secondary state + timeline + actions
3. [Action] Confirmation screen → for user-gated state transitions
4. Resolution screen          → final state summary + outcome
```

---

## STEP 3 — State × Screen Matrix

For every screen, define what EACH STATE shows:

```
SCREEN: [Entity] Detail — <actor>
──────────────────────────────────────────────────
PRIMARY state display (top of screen, always visible):
  PLACED           → "Order received" · pending icon
  ACCEPTED         → "Confirmed by vendor" · check icon
  IN_PROGRESS      → "Work in progress" · spinner icon
  DELIVERED        → "Delivered" · success icon
  CLOSED           → "Completed" · neutral icon
  CANCELLED        → "Cancelled" · destructive styling

SECONDARY state display (separate section, below primary):
  NONE             → hidden — do not show recovery section
  DISPUTED         → "Issue raised — under review" · warning banner
  ASSIGNED         → "Assigned to ops agent" · info banner
  RESOLVED         → "Resolution proposed" · action banner
  CONFIRMATION_PENDING → "Awaiting your response" · high-priority banner + CTA
  RECOVERED        → "Issue resolved" · success note
  REDO_IN_PROGRESS → "Revision in progress" · info banner
```

---

## STEP 4 — Contextual Action Rules

Define which actions are available based on STATE COMBINATION (primary × secondary):

```
ACTION RULES — <actor>
──────────────────────────────────────────────────
RULE FORMAT:
  IF primary=<X> AND secondary=<Y> → show [Action A, Action B] · hide [Action C]

Examples:
  IF lifecycle=IN_PROGRESS AND recovery=NONE
    → show: [Cancel Order, Contact Vendor]
    → hide: [Accept Resolution, Reject Resolution]

  IF lifecycle=IN_PROGRESS AND recovery=DISPUTED
    → show: [nothing — awaiting ops review]
    → disable: all vendor actions
    → display: "Issue under review — actions paused"

  IF lifecycle=DELIVERED AND recovery=CONFIRMATION_PENDING
    → show: [Accept Resolution, Reject Resolution]
    → block navigation away until user responds

  IF lifecycle=CLOSED AND recovery=RECOVERED
    → show: [Rate Experience]
    → hide: all dispute actions
```

---

## STEP 5 — Recovery Timeline Component

If a secondary state machine exists, define the timeline display:

```
RECOVERY TIMELINE — spec
──────────────────────────────────────────────────
Position: below primary state · above action buttons
Show only when: secondary state ≠ NONE

Steps (ordered):
  1. Issue raised           → filled when: DISPUTED or later
  2. Under review           → filled when: ASSIGNED or later
  3. Resolution proposed    → filled when: RESOLVED or later
  4. Awaiting confirmation  → filled when: CONFIRMATION_PENDING
  5. Resolved               → filled when: RECOVERED

Current step indicator: animated pulse on active step
Past steps: static filled icon
Future steps: empty / muted

Edge: if REDO_IN_PROGRESS → step 3 resets to empty, step 2 shows "In revision"
```

---

## STEP 6 — Edge Case UI (if --edge-cases)

Design the failure and rejection states explicitly:

```
EDGE CASES — <entity>
──────────────────────────────────────────────────
Case: User rejects resolution (1st or 2nd time)
  State: DISPUTED (returned from CONFIRMATION_PENDING)
  UI:    "You rejected the proposed resolution. Ops will review again."
         Show: rejection count badge ("2 of 3 rejections used")

Case: User rejects resolution 3rd time (final rejection)
  State: ESCALATED (or system-defined terminal)
  UI:    "Maximum rejections reached — escalated to senior review."
         Hide all action buttons

Case: Redo fails / vendor cannot complete
  State: REDO_FAILED → auto-transitions to REFUND
  UI:    "Revision could not be completed. Refund issued automatically."
         Show: refund amount + expected arrival date

Case: SLA breach (no vendor response in time)
  State: SLA_BREACHED → AUTO_REFUND triggered
  UI:    "No response received. Refund processed automatically."
```

---

## STEP 7 — Reusable State Component Spec

Define the shape of a state-driven component the developer will build:

```typescript
// COMPONENT SPEC: StateAwareEntity
// Props contract — do NOT implement here, pass to /cert-feature

interface StateAwareEntityProps {
  lifecycleState: LifecycleState;    // primary state machine
  recoveryState: RecoveryState;      // secondary state machine (NONE if no case)
  actor: 'customer' | 'vendor' | 'ops';
  onAction: (action: ActionType) => void;
}

// Rendering rules:
// 1. lifecycleState → always visible at top
// 2. recoveryState === NONE → hide recovery section
// 3. recoveryState !== NONE → show timeline + banner
// 4. Available actions derived from (lifecycleState × recoveryState × actor)
```

---

## STEP 8 — Handoff Summary

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
STATE UI DESIGN — <entity> — <actor>
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Screens:        [N] screens mapped
State pairs:    [N] primary × [N] secondary = [N] combinations
Action rules:   [N] rules defined
Timeline steps: [N] steps (if secondary state exists)
Edge cases:     [N] failure/rejection states (if --edge-cases)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Anti-patterns caught:
  ❌ Merged state machines → FIXED: displayed separately
  ❌ Generic actions → FIXED: contextual per state pair
  ❌ Missing edge cases → FIXED: rejection loop + SLA breach designed
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Ready for: /design-component (per screen) · /cortex-design-handoff
Or build:  /cert-feature (pass StateAwareEntityProps as spec)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## COMPLETION BLOCK

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SKILL:      design-state-ui
STATUS:     COMPLETE
ENTITY:     <entity>
ACTOR:      <actor>
SCREENS:    [N] defined
ACTIONS:    [N] rules
EDGE CASES: [N] | --edge-cases flag
NEXT:       /design-component | /cortex-design-handoff | /cert-feature
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```
