# TEAMBASE — Principal Architect Context Brief
# CORTEX v11.0 · SaaS / Multi-tenant Platform
# Last verified: 2026-03-14

## 0. SNAPSHOT

B2B SaaS platform · Multi-tenant · Organizations + Members + Plans + Billing
Stack: NestJS 10 · Prisma · TypeScript · BullMQ · Next.js 15 · React 19 · Tailwind 4
Status: Pre-build · Score: N/A → ALLOW · Cortex governed

---

## 1. ACTORS

```
Owner       → creates org · manages billing · transfers ownership
Admin       → invites members · manages roles · changes settings
Member      → uses product features · limited by plan
BillingAdmin→ manages payment methods · views invoices
```

---

## 2. CORE MODULES

```
auth            JWT + magic-link invite
organizations   slug · owner · settings · soft-delete
members         invite flow · role (owner/admin/member) · seat limits
plans           FREE / PRO / ENTERPRISE · feature flags per plan
subscriptions   Razorpay recurring billing · webhook lifecycle
billing         invoice history · upgrade/downgrade · proration
usage           per-org counters · enforce plan limits at action time
notifications   invite · payment-failed · plan-expiry · seat-limit
```

---

## 3. CRITICAL BUSINESS RULES

**Org lifecycle:** ACTIVE → SUSPENDED (payment failed) → CANCELLED
- One owner per org — transfer requires explicit handoff flow
- Seat limit enforced at invite time inside `$transaction` — never trust count
- Subscription status drives feature access — check sub status, never plan directly
- Downgrade: seat count must fit new plan limit before allowed

**Plans:**
- FREE: 3 seats · basic features
- PRO: 25 seats · all features · Razorpay recurring
- ENTERPRISE: unlimited · custom pricing · manual activation

**Razorpay:** subscription webhooks → `$transaction(Subscription=ACTIVE + Org=ACTIVE)`
Idempotency via ProcessedWebhookEvent. Failed payment → SUSPENDED not CANCELLED.

---

## 4. CODING STANDARDS

Follow `ai/core/SYSTEM_LAWS.md` — all 7 laws apply.
TypeScript: no `any` · explicit return types · interface for shapes
NestJS: Controller → Service → Prisma · DTOs with class-validator
Multi-table ops: always `$transaction` · Errors: P2002→409 · P2025→404

---

## 5. INVARIANTS (HARD HALTS)

- Org slug is immutable after creation
- Seat check inside `$transaction` — no race condition on invites
- Never check plan features directly — always check subscription status
- Subscription cancellation: mark CANCELLED, never delete record

---

## 6. GOVERNANCE

CORTEX v11.0 governs all AI work.
Every session: `/cert-session` · Every commit: `/cert-commit`
Score: N/A → ALLOW (run `/cert-verify` after first code)
Domain adapters: `saas-subscriptions` · `saas-organizations`
