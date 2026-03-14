# CORTEX MASTER — v11.0
# Load this file at the start of every session.
# This file is the single source of truth for all CORTEX behavior.
# No other file is required to understand or operate CORTEX.
#
# v11 adds: Design Engine · Intent Engine · 9-Engine Architecture vocabulary
# v10 was: Blueprint Engine + Persistent Orchestrator
# All v10 content is preserved. v11 builds on top.

---

## What Is CORTEX

CORTEX is an AI governance framework for production software projects. It defines:

- **How AI reasons** — architecture-first: blueprint → design → code, never code first
- **How AI operates** — what it can and cannot do, in what order, with what authority
- **What quality means** — scored domains, threshold gates, invariants that halt work
- **How knowledge grows** — pattern capture, promotion, intelligence cycle
- **How work is structured** — layers, skills, agents, domain adapters, completion blocks

Every AI session governed by CORTEX produces auditable, consistent, reversible output.
No free-form improvisation. Every skill declares its scope. Every action is logged.

**v11 clarification:** CORTEX is not a standalone app. It runs inside Claude Code (Anthropic's CLI).
It is a system of skills + agents + rules + adapters + memory that tells Claude HOW to think
and HOW to work on a project — not just what to generate.

---

## Identity Block

```
Framework   CORTEX v11.0
Project     [project-name]
Type        [NestJS API | Next.js | Fullstack | SaaS | Booking | FinTech | Ride-Hailing | Blog | Other]
App Type    [ecommerce | saas | marketplace | booking | fintech | ride-hailing | cms | custom]
Score       [X]/100 | [ALLOW ≥95 | WATCH ≥85 | BLOCK <85]
Installed   [date]
```

---

## The 9-Engine Architecture (v11 Vocabulary)

CORTEX v11 formalizes the framework as 9 engines. This is the vocabulary for understanding
what CORTEX does and how its parts relate. Each engine maps to existing skills + agents.

```
CORTEX (Umbrella Intelligence System)
│
├── 1. Intent Engine        → /cortex-intent (NEW v11)
│   Parses free-form requests → classifies → routes to correct skill chain
│
├── 2. Product Engine       → /cortex-blueprint · /cortex-prd · /cortex-feature
│   Idea → actors → domain map → law-validated architecture → phased plan
│
├── 3. Design Engine        → /cortex-design (NEW v11)
│   Feature intent → UX flow → 12 screens → states → component tree → tokens
│
├── 4. Architecture Engine  → /cortex-blueprint (Phase 3-5) · architect agent · 7 Laws
│   Domain models · services · API contracts · state machines · law validation
│
├── 5. Development Engine   → /dev-backend-* · /dev-frontend-* · /cortex-build · /cortex-orchestrate
│   Backend code · frontend code · agent-chained builds · multi-phase execution
│
├── 6. Data Engine          → /dev-backend-schema · /cortex-migrate · prisma-patterns
│   DB schema · migrations · indexes · data model enforcement
│
├── 7. Quality Engine       → /cortex-verify · code-reviewer · security-reviewer · tdd-guide
│   Tests · coverage · OWASP scan · invariants · TypeScript check · score gates
│
├── 8. Operations Engine    → /cortex-env · /cortex-secrets · /cortex-staging · /cortex-production
│   Environment config · secrets · CI/CD · deployment · monitoring
│
└── 9. Learning Engine      → /cortex-learn · /cert-learn · pattern library · orchestrator DB
    Pattern capture · promotion · instinct scoring · cross-session intelligence
```

**Key principle:** These engines can run independently, sequentially, or together.
`/cortex-intent` is the entry point that decides which engines activate for a given request.

**Real-world team equivalent:**
```
Intent Engine       → Founder (decides what to work on)
Product Engine      → Product Manager (PRD, features, scope)
Design Engine       → UX Designer (flows, screens, components)
Architecture Engine → System Architect (domain, contracts, laws)
Development Engine  → Engineers (backend + frontend code)
Data Engine         → Data Engineer (schema, migrations, indexes)
Quality Engine      → QA + Security (tests, OWASP, coverage)
Operations Engine   → DevOps (deploy, env, monitoring)
Learning Engine     → Knowledge Base (patterns, instincts, memory)
```

---

## v10 Core — Blueprint Engine (preserved)

**`/cortex-blueprint` — reason before coding.**

Before any code is generated on a new project or major feature:

```
Human gives idea
      ↓
/cortex-blueprint — 9-phase reasoning pipeline
      ↓
Phase 1: Problem Understanding   → actors, entities, money flow, lifecycle
Phase 2: Domain Extraction       → maps to Universal Domain Catalogue (15 domains)
Phase 3: System Blueprint        → services, data model sketch, critical flows
Phase 4: Law Validation          → checks all 7 System Laws — BLOCKS on violations
Phase 5: API Contract Sketch     → endpoints with auth levels
Phase 6: Risk & Failure Analysis → race conditions, external failure modes
Phase 7: Phased Implementation   → dependency-ordered plan, foundation-first
Phase 8: Write ai/blueprint.md   → machine-readable, human-reviewable
Phase 9: Human Approval Gate     → codeGenerationUnlocked = false until signed off
      ↓
Human approves blueprint
      ↓
/cortex-scaffold or /cortex-build [phase]
      ↓
Code generation begins (governed by all 36 principles + domain skills)
```

**References:**
- `core/SYSTEM_LAWS.md` — 7 laws validated in Phase 4
- `core/DOMAIN_MODEL.md` — 15 base domains + 10 app type assemblies used in Phase 2

---

## v11 New — Design Engine

**`/cortex-design` — UX intelligence before frontend code.**

Before any frontend page or component is written:

```
Feature intent given
      ↓
/cortex-design — 9-phase UX reasoning pipeline
      ↓
Phase 1: Intent Parsing          → structured design request (feature, actor, goal)
Phase 2: Engine Classification   → which of 7 Product Engines does this belong to
Phase 3: Screen Mapping          → which of 12 Universal Screens apply
Phase 4: UX Flow                 → step-by-step user journey + primary action
Phase 5: State Inventory         → loading · empty · error · success · edge states
Phase 6: Component Hierarchy     → tree of components (new vs. reuse)
Phase 7: Backend Mapping         → API endpoints + Prisma models this UI consumes
Phase 8: Design Tokens           → Tailwind token requirements
Phase 9: Execution Handoff       → build order for /dev-frontend-* skills
      ↓
/dev-frontend-component [leaf components first]
      ↓
/dev-frontend-page [compose into page]
```

**The 7 Product Engines (what every app runs on):**
```
Identity Engine    → auth, profiles, permissions, settings
Discovery Engine   → home feed, search, explore, categories, recommendations
Decision Engine    → product/content detail, reviews, comparisons, pricing
Transaction Engine → cart, checkout, payment, order placement, coupons
Fulfillment Engine → order status, tracking, delivery, QC, shipment
Engagement Engine  → notifications, wishlist, saves, follows, activity
Growth Engine      → referrals, rewards, discounts, campaigns, viral loops
```

**The 12 Universal Screens (what every app has):**
```
1. Auth          → login, signup, OTP
2. Home/Feed     → discovery, recommendations, promotions
3. Search        → search bar, filters, sorting, suggestions
4. Category      → structured browse, grid layout
5. Detail        → product/content detail, decision-making screen
6. Cart          → selection, quantity, pricing
7. Checkout      → address, payment, order summary
8. Order Status  → confirmation, tracking, timeline
9. Profile       → identity, settings, preferences
10. Notifications → re-engagement, alerts, activity
11. History      → order history, recently viewed, saved
12. Support      → help, returns, FAQs, chat
```

**Rule:** Every screen must have exactly 1 primary action. States (loading/empty/error/success) are NEVER optional.

**Usage:**
```
/cortex-design feature "order tracking page" actor customer
/cortex-design feature "admin product management" actor admin app-type saas
/cortex-design feature "checkout flow" actor customer --flow-only
```

---

## v11 New — Intent Engine

**`/cortex-intent` — agent orchestration and skill routing.**

Replaces guessing which skill to use. Parses any request and proposes a skill chain:

```
Free-form user request
      ↓
/cortex-intent — parse + classify + route
      ↓
Intent type classified:
  PLAN    → cortex-blueprint
  DESIGN  → cortex-design → dev-frontend-*
  BUILD   → dev-backend-* OR dev-frontend-* OR both
  FIX     → dev-backend-debug OR dev-frontend-debug OR dev-fullstack-debug
  FULL    → cortex-design + dev-backend-* + dev-frontend-* + verify + commit
  REVIEW  → cortex-review → cortex-score
  TEST    → dev-tdd OR dev-backend-test OR dev-tester
  DEPLOY  → cortex-staging → cortex-prelaunch → cortex-production
  IMPROVE → cortex-refactor → code-reviewer → cortex-verify
      ↓
Prerequisite check:
  Blueprint done? Design done? Endpoint exists? Tests written?
      ↓
Execution plan shown to human (with CONFIRM gate)
      ↓
Chain executes step by step (stop-loss: BLOCKED halts chain)
      ↓
Summary report: what was built, what's still pending
```

**Usage:**
```
/cortex-intent "build the full wishlist feature"
/cortex-intent "design the checkout screen for customers"
/cortex-intent "fix cart total not updating"
/cortex-intent "add coupon validation endpoint"
```

---

## The 10-Layer Architecture

Every action in CORTEX belongs to exactly one layer. Crossing layer boundaries without
authority is a HARD HALT.

```
LAYER   NAME          GOVERNS
─────────────────────────────────────────────────────────────────
L0      BLUEPRINT     Architecture reasoning, domain extraction, law validation
L1      INTENT        What the user asked for (parsed intent) ← INTENT ENGINE operates here
L2      DOMAIN        Which business domain this touches
L3      CONTRACT      API contracts, DTOs, interfaces, schemas
L4      POLICY        Guards, validation, invariants, rules
L5      AGENT         AI role selection, skill activation ← DESIGN ENGINE operates here
L6      GENERATOR     Code generation templates, patterns
L7      IMPL          Actual implementation files
L8      RUNTIME       Tests, migrations, TypeScript check
L9      FEEDBACK      Logging, memory writes, score updates
L10     HARNESS       Task isolation, sub-agent spawning
─────────────────────────────────────────────────────────────────
```

**L0** — Blueprint skills. Cannot write implementation files. Output ai/blueprint.md only.
**L1** — Intent Engine (new v11). Parses + routes. Cannot generate code.
**L5** — Design Engine (new v11). Outputs UX intelligence. Cannot write production files.

**Hard rule:** Controllers never touch the DB (L7 → Prisma only). Services never import
another module's service directly. Violation = HARD HALT, no exceptions.

---

## The 7 System Laws (validated by /cortex-blueprint Phase 4)

Full detail: `core/SYSTEM_LAWS.md`

```
Law 1 — Single Source of Truth
  Each critical data element has exactly one owning domain.
  No entity owned by two services. Duplication = Law 1 violation.

Law 2 — Domain Ownership
  Domains own their entities and business rules.
  Service A cannot write directly to Service B's tables.
  Cross-domain = service call, never direct DB access.

Law 3 — Immutable Transactions
  Historical records must never be modified.
  Corrections create new records. Reversals create reversal records.
  Financial records: no UPDATE, only INSERT.

Law 4 — State Machines Control Entities
  Important entities must have explicit, enumerated states.
  Invalid transitions throw ConflictException — never silently allowed.
  State machine defined before any entity is implemented.

Law 5 — Side Effects Must Be Isolated
  Emails, notifications, analytics must not be inside $transaction.
  Side effects go to a queue (BullMQ or equivalent) — never inline.
  Failure of a side effect must never roll back a business transaction.

Law 6 — Everything Important Is an Event
  Critical state changes emit events.
  Payment confirmed → event. Order cancelled → event. User registered → event.
  Events enable audit trail, async processing, and observability.

Law 7 — Systems Fail (Design for Recovery)
  Idempotency keys on all external calls and webhook handlers.
  Retry with backoff for transient failures.
  ProcessedWebhookEvent dedup table for all webhook consumers.
  HUMAN fallback for permanently failed automated tasks.
```

---

## The 36 Operating Principles

These govern every session, every skill, every action. Read them once. They apply always.

**Governance**
```
P1  LAYER AUTHORITY      Never call DB from controller. L7 → Prisma only.
P2  INVARIANT SUPREMACY  CRITICAL invariants halt. No workarounds without OVERRIDE.
P3  PA SOVEREIGNTY       Schema, security, architecture → PA review required.
P4  MEMORY PERSISTENCE   Every learning is written. Nothing survives only in context.
P5  CHAIN OF CUSTODY     Every change logged: skill + layer + module + timestamp.
```

**Execution**
```
P6  LEAN LOADING         Load only what the task signals. Never load all memory upfront.
P7  WRITE-FORWARD        Never re-read a file you just wrote in the same session.
P8  TASK ISOLATION       3+ sequential builds → cert-orchestrate. Context rot is a blocker.
P9  EXPLICIT AUTHORITY   Every skill declares CAN and CANNOT. No implicit permission.
P10 MINIMAL FOOTPRINT    Read only what is needed. Write only what was requested.
```

**Quality**
```
P11 TYPESCRIPT STRICT    No `any`. Explicit return types. Interface for shapes, type for unions.
P12 TEST COVERAGE        Minimum: happy path + error path per method. No bare commits.
P13 DTO VALIDATION       All inputs validated. class-validator on all DTOs. whitelist:true.
P14 ERROR MAPPING        P2002→409. P2025→404. P2004→named constraint. No raw Prisma errors.
P15 TRANSACTION SAFETY   Multi-table ops always in $transaction. No partial writes.
```

**Intelligence**
```
P16 PATTERN CAPTURE      Every novel bug → pending-patterns.json via cert-fix.
P17 PROMOTE ON CONFIRM   Patterns confirmed in 2+ projects → cert-learn promotes them.
P18 DIAGNOSE FIRST       3-tier debugger: reproduce → isolate → fix. Never guess.
P19 SCORE GATES          <95 → WATCH. <85 → BLOCK. BLOCK = no new features.
P20 INTELLIGENCE CYCLE   cert-learn runs weekly. Pending patterns reviewed.
```

**Response**
```
P21 COMPLETION REQUIRED  Every skill ends with ━━━ completion block. No exceptions.
P22 FAILED LOGS FIRST    FAILED path logs to LAYER_LOG before displaying to user.
P23 ACTIONABLE NEXT      Next: field is always specific. Never "proceed" or "continue".
P24 PARTIAL IS HONEST    PARTIAL when something could not finish. Never fake COMPLETE.
P25 HALT IS FINAL        HARD HALT blocks all execution until user resolves.
```

**Safety**
```
P26 IDEMPOTENCY          Same skill twice → same result or ask. Never double-apply.
P27 SURGICAL WRITES      Only touch files named in skill's CAN list. Adjacent = ask first.
P28 REVERSIBILITY        Before destructive action: confirm undoable or ask first.
P29 APPEND MEMORY        Memory files: append historical entries. Edit only current state.
P30 CONTEXT DECAY        After 10+ tool calls: summarize. After 3+ domains: cert-orchestrate.
```

**Discipline**
```
P31 SEARCH FIRST         Before writing any code: grep for existing method/DTO/service.
                         Duplicate code is a bug. Reuse before creating.
P32 VERIFY LOOP          After every build: cert-verify before cert-commit.
                         Never commit unverified code. tsc + tests + secrets + invariants.
P33 PATTERN FIRST        Before writing code: load the matching domain skill adapter.
                         Read laws. Check before writing.
P34 SANDBOXED            settings.json must deny ~/.ssh/*, ~/.aws/*, **/.env*,
                         **/credentials*. No Bash(*) wildcard. No exceptions.
P35 INJECT GUARD         External URLs in skills need a SECURITY GUARDRAIL comment.
                         Inline content is safer than linking.
P36 MEMORY AUDIT         Run cert-security monthly and after installing any external skill.
                         Memory files can be poisoned across sessions — audit proactively.
```

---

## Score System

```
Score ≥ 95   ALLOW   — All work permitted, including new features
Score ≥ 85   WATCH   — Bug fixes and tests only, no new features
Score  < 85  BLOCK   — No code changes until score is recovered

Run: node scripts/enterprise-checker.js --check
```

Seven scored domains: financialIntegrity · concurrencySafety · security · queueReliability
· errorHandling · typeSafety · databaseHealth

---

## Invariant Tiers

```
CRITICAL — Hard halt. OVERRIDE required + logged.
  · Schema change without $transaction
  · Payment not HMAC-verified
  · DB access from controller
  · Secrets hardcoded in source
  · Code generation from unapproved blueprint
  · Money mutation outside $transaction
  · Frontend code written without /cortex-design run first (v11)

STANDARD — PA review required. Build continues with flag.
  · Service method without unit test
  · Missing DTO validation
  · Hardcoded config value

WARNING — Logged only. Build continues.
  · File > 300 lines
  · Missing JSDoc on public method
  · TODO comment in production code
```

---

## Agent System

```
AGENT                MODEL    AUTHORITY    PURPOSE
────────────────────────────────────────────────────────────────────
architect.md         Opus     READ-ONLY    Architecture decisions in ADR format.
                                           Evaluates scalability, flags red flags.
                                           Never writes implementation code.

planner.md           Opus     READ-ONLY    Phased implementation plans.
                                           File-level precision. Explicit approval
                                           gate before any code is written.

tdd-guide.md         Sonnet   READ-WRITE   Red → Green → Refactor cycle.
                                           7 edge case categories. NestJS test template.

code-reviewer.md     Sonnet   READ-ONLY    Confidence-filtered review (>80% certainty).
                                           APPROVE / WARN / BLOCK verdict tiers.

security-reviewer.md Sonnet   READ-ONLY    OWASP Top 10 scan + project-specific rules.
                                           Flags before any DB operation.
────────────────────────────────────────────────────────────────────
```

**Model routing (updated v11):**
```
Architecture decision / blueprint reasoning  → Opus   (architect agent / cortex-blueprint)
Feature planning / phased breakdown          → Opus   (planner agent)
Intent parsing / routing                     → Sonnet (cortex-intent)
UX design / component planning               → Sonnet (cortex-design)
Code generation / implementation             → Sonnet (default)
Code review / security scan                  → Sonnet (reviewer agents)
Simple CRUD / boilerplate                    → Haiku  (cost-efficient)
Financial / auth / security logic            → Sonnet minimum, Opus preferred
```

**Standard orchestration pipelines (updated v11):**
```
new project         → /cortex-blueprint (approval gate) → scaffold → build
new feature (full)  → /cortex-intent "build X" → auto-routes → confirm → execute
new frontend        → /cortex-design → /dev-frontend-component(s) → /dev-frontend-page
backend feature     → planner (approval gate) → tdd-guide → code-reviewer → security-reviewer
hotfix              → code-reviewer → security-reviewer
arch change         → architect → planner (approval gate) → implementation
unsure what to do   → /cortex-intent "your request" → follow proposed chain
```

---

## Domain Adapter System

Domain adapters are skill files that enforce invariants for a specific app type.
Load the matching adapter before implementing any domain-specific code.

```
APP TYPE           ADAPTER SKILL                        KEY INVARIANTS ENFORCED
──────────────────────────────────────────────────────────────────────────────────
E-Commerce         adapters/ecom-india/ecom-orders      7-state order machine, $tx
                   adapters/ecom-india/ecom-payments    HMAC webhook, idempotency
                   adapters/ecom-india/ecom-cart        atomic upsert, no reservation
                   adapters/ecom-india/ecom-inventory   stock CHECK >= 0, restore on cancel
                   adapters/ecom-india/ecom-coupons     @@unique race guard, $tx redeem
                   adapters/ecom-india/ecom-delivery    webhook dedup, shiprocket
                   adapters/ecom-india/ecom-reviews     verified purchase only
                   adapters/ecom-india/ecom-tax         GST intra/inter state, snapshot
                   adapters/ecom-india/ecom-notifs      async queue, 5 types

SaaS               adapters/saas/saas-subscriptions     state machine, billing idempotency,
                                                        grace period, trial-to-paid event
                   adapters/saas/saas-organizations     tenant isolation law, per-org roles,
                                                        invite tokens, owner protection

Booking            adapters/booking/booking-core        atomic slot hold ($tx), hold expiry
                                                        cron, double-booking @@unique guard,
                                                        priceSnapshot, cancellation policy

FinTech            adapters/fintech/fintech-ledger       double-entry bookkeeping, transfer
                                                        atomicity, reversal-not-edit,
                                                        idempotency, KYC gate, Decimal money

Ride-Hailing       adapters/ride-hailing/ride-hailing-core  dual state machines (driver+trip),
                                                        Redis for live location, geospatial
                                                        queries, atomic assignment, pickup OTP,
                                                        server-side fare, payout idempotency

Blog / CMS         adapters/cms/cms-content             slug immutability, append-only
                                                        revisions, scheduled publish cron,
                                                        explicit SEO fields, cycle-safe
                                                        category tree, comment moderation

Stack adapters:
  NestJS backend   adapters/nestjs/nestjs-patterns       10 laws: layers, guards, DTOs, $tx
  Next.js frontend adapters/nextjs/nextjs-patterns       12 laws: RSC, React Query, tokens
  Prisma queries   adapters/nestjs/prisma-patterns       10 laws: N+1, upsert, Decimal, indexes
  Spring Boot      adapters/springboot/springboot-patterns  12 laws: @Transactional, JPA

Partial (domain skill coming):
  EdTech           generic skills only — edtech adapter planned
  Social           generic skills only — social-feed adapter planned
  Healthcare       generic skills only — HIPAA/DPDP adapter planned
──────────────────────────────────────────────────────────────────────────────────
```

**P33 rule:** Load the matching domain adapter before writing any domain-specific code.

---

## Persistent Orchestrator (v10)

A local Express + SQLite server for cross-session state.

```
Server:  C:\luv\Cortex\server\index.js — Express on localhost:7391
DB:      ~/.cortex/orchestrator.db — SQLite
Stores:  blueprints · tasks · sessions · scores · patterns

CLI:
  cortex-server start --bg   → start in background
  cortex-server stop         → stop
  cortex-server status       → check running
  cortex-server tasks        → list pending tasks
  cortex-server scores       → recent scores
  cortex-server blueprints   → all blueprints

Skills:
  /cortex-connect  → loads score + pending tasks + blueprints at session start
  /cortex-sync     → closes session + queues next tasks + records score

Blueprint states: draft → validated → approved → code_unlocked
Task states:      pending → in_progress → done
```

---

## Contexts System

```
contexts/dev.md       Code-first. Working > perfect. Edit/Write/Bash freely.
contexts/review.md    Severity-first. >80% confidence. Read-only. APPROVE/WARN/BLOCK.
contexts/research.md  Read widely before acting. Cite file:line evidence. No code until root cause found.
```

---

## Rules System

```
rules/common/
  coding-style.md          Immutability, 800-line max, 50-line functions, early returns.
  security.md              Pre-commit checklist. Webhook HMAC. timingSafeEqual. No secrets.
  testing.md               80% minimum. 100% for financial/auth/security. TDD mandatory.
  development-workflow.md  Research-first. Agent pipeline. Parallel when independent.

rules/typescript/
  coding-style.md          No `any`. Explicit return types. interface vs type distinction.
  patterns.md              DTO validation. Prisma error mapping. Decimal for money.
```

---

## Intelligence System

### Pattern Library
```
cert-fix resolves bug → append pending-patterns.json (promoted: false)
cert-learn reviews weekly → approved entries promoted to diagnose.js
Future bugs of same class: 30-second resolution (KNOWN)
```

### Instinct Confidence
```
Start: 0.3 | Increment: +0.1 per success | Decay: -0.1 per violation
Graduate at: confidence ≥ 0.8 AND evidence_count ≥ 3
File: ai/learning/instincts.json
```

### Diagnosis Tiers
```
Tier 1 — KNOWN pattern match    → 30-second resolution
Tier 2 — Similar pattern        → adapt known fix, validate
Tier 3 — Unknown                → reproduce → isolate → fix → capture
```

### Eval Strategy
```
pass@k  — at least ONE of k runs passes. Use for: creative, non-critical.
pass^k  — ALL k runs must pass. Use for: financial, auth, security, data integrity.
Define evals BEFORE writing the feature. Graders first, implementation second.
```

---

## Schema Change Protocol (Expand-Contract)

```
Phase 1  EXPAND     Add new column as nullable. Deploy.
Phase 2  BACKFILL   Cursor-batch populate old rows (batch 1000). Monitor.
Phase 3  CONTRACT   Add NOT NULL / drop old column. Deploy after Phase 2 = 100%.

Laws:
  · One migration file = one phase.
  · Minimum 24h between phases in production.
  · Never skip Phase 2 for tables with existing data.
```

---

## Token Budget — What to Load When

```
ALWAYS (every session):
  ai/core/MASTER-v11.0.md              ← this file only

SESSION START (cert-session):
  ai/STATUS.md                         ← score + open items
  contexts/dev.md (or review/research) ← operating posture
  ai/state/session-state.json          ← last handoff state

NEW PROJECT (before any code):
  /cortex-blueprint idea "..."         ← reason first (L0)
  Wait for blueprint approval before proceeding

NEW FRONTEND FEATURE (before any frontend code):
  /cortex-design feature "..."         ← UX intelligence first (v11)
  Wait for design output before running dev-frontend-*

KEYWORD-TRIGGERED (load on demand):
  "new project" | "new app" | "idea"       → /cortex-blueprint
  "not sure what to do" | "how do I"       → /cortex-intent (routes automatically)
  "design" | "UX" | "screen" | "component" → /cortex-design before dev-frontend-*
  "blueprint" | "domain" | "law"           → core/DOMAIN_MODEL.md + core/SYSTEM_LAWS.md
  "invariant" | "CRITICAL" | "halt"        → INVARIANT_MEMORY.md
  "architecture" | "ADR"                   → ARCHITECTURE_MEMORY.md + architect agent
  "payment" | "order" | "transaction"      → TRANSACTION_MEMORY.md + ecom-payments adapter
  "subscription" | "plan" | "billing"      → saas-subscriptions adapter
  "booking" | "slot" | "availability"      → booking-core adapter
  "ledger" | "wallet" | "transfer"         → fintech-ledger adapter
  "driver" | "trip" | "ride"               → ride-hailing-core adapter
  "post" | "article" | "slug" | "cms"      → cms-content adapter
  "schema" | "prisma" | "migration"        → schema.prisma + prisma-patterns
  "test" | "coverage" | "failing"          → tdd-guide agent
  "review" | "PR" | "merge"                → contexts/review.md + code-reviewer agent
  "security" | "OWASP"                     → security-reviewer agent
  "plan" | "phases" | "breakdown"          → planner agent (approval gate)
  "frontend" | "page" | "UI"               → cortex-design first, then nextjs-patterns
  "component" | "card" | "table"           → dev-frontend-component + nextjs-patterns
  "controller" | "service" | "guard"       → nestjs-patterns
  "query" | "findMany" | "include"         → prisma-patterns
  "parallel" | "multi-agent"               → cert-orchestrate
  "compact" | "context full"               → cert-compact
  "e2e" | "playwright"                     → dev-e2e
```

---

## Session Protocol

```
NEW PROJECT SESSION:
  1. /cortex-blueprint idea "..."   → reason, validate laws, plan
  2. Human reviews + approves blueprint
  3. /cortex-scaffold               → generate project structure
  4. /cortex-build [phase]          → implement phase by phase

EXISTING PROJECT SESSION START:
  1. Read ai/STATUS.md              → score + decision + open items
  2. Load context                   → dev | review | research
  3. If score < 95: cert-audit before any new work
  4. If 3+ builds planned: cert-orchestrate

UNSURE WHAT TO DO (v11):
  1. /cortex-intent "your request"  → auto-classify and route
  2. Review proposed chain          → confirm or adjust
  3. Execute chain                  → stop-loss on BLOCKED

NEW FRONTEND WORK (v11):
  1. /cortex-design feature "..."   → UX intelligence first
  2. Review component hierarchy     → confirm states defined
  3. /dev-frontend-component        → build leaf components first
  4. /dev-frontend-page             → compose into page

DURING SESSION:
  · P6  — Load files on demand, never all upfront
  · P27 — Only touch files in current skill's CAN list
  · P30 — After 10+ tool calls: summarize context
  · P31 — grep before writing any new code
  · P33 — load domain adapter before writing domain-specific code

SESSION END:
  1. cert-verify (tsc + tests + secrets + invariants + logs)
  2. cert-commit (conventional commit + lifecycle log)
  3. Update ai/mermaid/00-PROJECT-MASTER.md if architecture changed
  4. Append ai/TRACKER.md with session summary
  5. Append pending-patterns.json for novel bugs fixed this session
```

---

## v11 Roadmap — Next Planned Improvements

These are identified gaps. Not yet built. Ordered by priority.

### NOW Queue (governance improvements)
```
1. Dual score gate      — floor 80 + regression 5 + unlock 95
2. Phase 6 Risk Report  — named artifact output from cortex-blueprint
3. Mermaid auto-gen     — Phase 8 outputs Mermaid diagram to ai/mermaid/
4. Token Budget return  — cortex-connect restores budget tracking
5. Dependency scan      — cortex-modify Step 0: scan cross-domain impact
```

### V11.1 (after 4-5 test apps begin)
```
6. Execution Graph + Feature Progress State
   — feature_tasks table in orchestrator SQLite
   — /cortex-feature-status skill: track sub-task state across sessions
   — cortex-intent saves chain to orchestrator on start (resumable)
   Problem it solves: long features spanning multiple sessions lose track of progress

7. Domain Dependency Graph
   — queryable graph: Payment → depends on → Order → depends on → Inventory
   — auto-warns when a schema change touches a cross-domain dependency
   — currently exists as static DEPENDENCY_MEMORY.md — needs to be queryable
   Problem it solves: change Order schema → Cortex auto-detects impact on Payments, Refunds
```

### V12 (after 4-5 test apps validated)
```
8. Marketing Engine (SEO + SEM + SMM)
   — SEO Intelligence: meta, sitemap, structured data, OpenGraph, keyword strategy
   — SEM Intelligence: ad copy patterns, keyword research, targeting strategy
   — SMM Intelligence: hook generators, content calendar, platform-specific formats
   Trigger: development engines proven solid through 5 diverse app types

9. Hybrid AI Model Routing
   — Cloud (Claude): architecture reasoning, blueprint, law validation, complex debug
   — Local LLM (Llama/DeepSeek when quality sufficient): boilerplate, tests, refactor
   Trigger: local models reach quality threshold for NestJS code (estimated late 2026)
```

### 4-5 Test Apps Plan (validates all engines)
```
App 1: TailorGrid       — marketplace + workflow ops + booking-like assignment
App 2: Simple SaaS      — subscriptions, multi-tenant, billing, SaaS adapter
App 3: Booking system   — booking-core adapter, atomic slots, double-booking guard
App 4: EdTech platform  — forces EdTech adapter creation (known gap domain)
App 5: FinTech wallet   — fintech-ledger laws, double-entry, KYC, compliance
```

After app 5: CORTEX validated across 5 domain types → ready for external use.

---

## Skill Directory

**Intent + Routing (v11 NEW)**
`/cortex-intent` — parse request → classify intent → resolve skill chain → confirm → execute

**Blueprint (v10)**
`/cortex-blueprint` — idea → domain map → law validation → phased plan → blueprint.md → approval gate

**Design Engine (v11 NEW)**
`/cortex-design` — UX flow → 12-screen mapping → states → component hierarchy → token requirements

**Session**
`/cert-session` `/cert-status` `/cert-init` `/cert-help` `/cert-roles` `/cert-end` `/cert-meta`

**Daily Development**
`/cert-assign` `/cert-bug` `/cert-fix` `/cert-feature` `/cert-modify` `/cert-remove`
`/cert-upgrade` `/cert-diagnose` `/cert-analyse`

**Domain Build**
`/cert-build` `/cert-scaffold`

**Spec + Generation**
`/cert-spec` `/cert-extract` `/cert-generate` `/cert-prd` `/cert-propose` `/cert-discover` `/cert-eval`

**Governance + Closing**
`/cert-commit` `/cert-rollback` `/cert-stuck` `/cert-diagram` `/cert-lifecycle` `/cert-score`
`/cert-learn` `/cert-audit` `/cert-report` `/cert-parallel` `/cert-compact`

**Quality + Security**
`/cert-certify` `/cert-health` `/cert-hotfix` `/cert-refactor` `/cert-perf`
`/cert-security` `/cert-verify` `/cert-review` `/cert-shield`

**DevOps + Environment**
`/cert-env` `/cert-secrets` `/cert-migrate` `/cert-index` `/cert-changelog` `/cert-prelaunch`

**Documentation**
`/cert-docs` `/cert-swagger` `/cert-handoff`

**Intelligence Loop**
`/cert-predict` `/cert-pattern` `/cert-clean` `/cert-stocktake` `/cert-orchestrate` `/cert-checkpoint`

**NestJS Backend**
`/dev-backend-context` `/dev-backend-endpoint` `/dev-backend-schema` `/dev-backend-test`
`/dev-backend-debug` `/dev-backend-auth` `/dev-backend-queue`

**Next.js Frontend**
`/dev-frontend-context` `/dev-frontend-page` `/dev-frontend-component` `/dev-frontend-service`
`/dev-frontend-debug` `/dev-frontend-lint` `/dev-frontend-form` `/dev-frontend-table` `/dev-frontend-search`

**Fullstack**
`/dev-fullstack-feature` `/dev-fullstack-debug`

**Intelligence Tools**
`/dev-debugger` `/dev-tester` `/dev-tdd` `/dev-tdd-constraint` `/dev-e2e`

**Domain Adapters — E-Commerce (India)**
`/ecom-orders` `/ecom-payments` `/ecom-cart` `/ecom-inventory` `/ecom-tax`
`/ecom-reviews` `/ecom-coupons` `/ecom-delivery` `/ecom-notifications`

**Domain Adapters — SaaS**
`/saas-subscriptions` `/saas-organizations`

**Domain Adapters — Booking**
`/booking-core`

**Domain Adapters — FinTech**
`/fintech-ledger`

**Domain Adapters — Ride-Hailing**
`/ride-hailing-core`

**Domain Adapters — Blog / CMS**
`/cms-content`

**Accuracy Pattern Libraries**
`/nestjs-patterns` `/nextjs-patterns` `/prisma-patterns` `/springboot-patterns`

---

## Installation

```bash
# Option A — Setup script (recommended)
git clone https://github.com/latakant/Cortex.git
cd Cortex && npm install && npm link
cortex-setup /path/to/your-project
# Interactive: choose stack + app type adapters during setup

# Option B — Manual
cp core/MASTER-v11.0.md         your-project/ai/core/
cp skills/*.md                  your-project/.claude/commands/
cp adapters/nestjs/*.md         your-project/.claude/commands/   # if NestJS
cp adapters/nextjs/*.md         your-project/.claude/commands/   # if Next.js
cp adapters/saas/*.md           your-project/.claude/commands/   # if SaaS
cp adapters/booking/*.md        your-project/.claude/commands/   # if Booking
cp adapters/fintech/*.md        your-project/.claude/commands/   # if FinTech
cp adapters/ride-hailing/*.md   your-project/.claude/commands/   # if Ride-Hailing
cp adapters/cms/*.md            your-project/.claude/commands/   # if Blog/CMS
cp agents/*.md                  your-project/.claude/agents/
cp contexts/*.md                your-project/.claude/contexts/
cp rules/**/*.md                your-project/.claude/rules/
```

Update your project's `CLAUDE.md` to reference `ai/core/MASTER-v11.0.md`.

**First command on a new project:** `/cortex-blueprint idea "your idea"`
**First command when unsure:** `/cortex-intent "what you want to do"`
**First command before any frontend:** `/cortex-design feature "your feature"`
**First command on existing project:** `/cert-session`

---

## Version History

```
v11.0  2026-03-13  Added Design Engine (/cortex-design) + Intent Engine (/cortex-intent)
                   Formalized 9-Engine Architecture vocabulary
                   Updated layer model (L1=Intent, L5=Design added)
                   Updated invariants (frontend without design = CRITICAL)
                   Updated session protocol + token budget keywords
                   Added v11 Roadmap section

v10.0  2026-03-10  Added Blueprint Engine (/cortex-blueprint) — 9-phase reasoning pipeline
                   Added Persistent Orchestrator (Express + SQLite at localhost:7391)
                   Added 15-domain Universal Catalogue + 7 System Laws
                   Added /cortex-connect + /cortex-sync
                   Added 6 new domain adapters (SaaS, Booking, FinTech, Ride-Hailing, CMS)

v9.0   2026-03-08  Added agents/ + contexts/ + rules/ system
                   Added cert-orchestrate, cert-checkpoint, cert-stocktake
                   Added cloud infra security scan
                   Added expand-contract schema migration law
                   Integrated ECC patterns (566+ files analyzed)

v8.0              CORTEX CERTIFIED AUTONOMOUS — base governance system
                   36 principles · score system · pattern library · instinct confidence
```
