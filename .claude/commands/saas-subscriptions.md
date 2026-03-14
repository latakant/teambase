# saas-subscriptions — Subscription Lifecycle Standards
> Load this before working on: plans, subscriptions, billing cycles, upgrades, downgrades, cancellations, trials.
> Applies to: BACKEND_DEV, SENIOR_FULLSTACK, PRINCIPAL_ARCHITECT
> App type: SaaS / Subscription

---

## The Subscription State Machine — Memorise This

```
TRIALING → ACTIVE → PAST_DUE → CANCELLED
              ↓          ↓
           CANCELLED  CANCELLED
              ↓
           PAUSED (voluntary)
              ↓
           ACTIVE (resumed)
```

### Legal Transitions Table

| From | To | Who | Condition |
|------|----|-----|-----------|
| — | TRIALING | System | Account created with trial |
| TRIALING | ACTIVE | System | Trial ends + payment method on file |
| TRIALING | CANCELLED | System / User | Trial ends, no payment method |
| ACTIVE | PAST_DUE | System | Payment failed on renewal |
| ACTIVE | PAUSED | User | Voluntary pause (if plan allows) |
| ACTIVE | CANCELLED | User / System | Cancellation requested / non-payment |
| PAST_DUE | ACTIVE | System | Retry payment succeeded |
| PAST_DUE | CANCELLED | System | Max retries exhausted |
| PAUSED | ACTIVE | User | Resume requested |
| CANCELLED | ACTIVE | User | Re-subscribe (new subscription record) |

**Terminal state:** CANCELLED — do not modify. Re-subscribe = new Subscription record. (Law 3)

---

## Core Rules

### 1. Plan change is always Expand-Contract, never overwrite

```typescript
// WRONG — modifying active subscription price
await prisma.subscription.update({ where: { id }, data: { planId, pricePerPeriod } })

// CORRECT — end current, create new
await prisma.$transaction([
  prisma.subscription.update({
    where: { id: currentSubId },
    data: { status: 'CANCELLED', cancelledAt: new Date(), cancelReason: 'UPGRADE' }
  }),
  prisma.subscription.create({
    data: {
      userId, planId: newPlanId, status: 'ACTIVE',
      startedAt: new Date(), pricePerPeriod: newPlan.price,
      previousSubscriptionId: currentSubId  // audit trail
    }
  })
])
```

Historical subscription records are immutable. Upgrades/downgrades create new records. (Law 3)

### 2. Access control is derived from subscription status, checked server-side

```typescript
// Every protected route checks this — never trust client claims
async function requireActiveSubscription(userId: string, feature: string): Promise<void> {
  const sub = await prisma.subscription.findFirst({
    where: { userId, status: { in: ['ACTIVE', 'TRIALING'] } },
    include: { plan: { include: { features: true } } }
  })
  if (!sub) throw new ForbiddenException('No active subscription')
  const hasFeature = sub.plan.features.some(f => f.key === feature)
  if (!hasFeature) throw new ForbiddenException(`Feature '${feature}' not in your plan`)
}
```

Never evaluate access from a boolean flag on the user record — always derive from live subscription.

### 3. Billing is idempotent — same period, same invoice

```typescript
// Idempotency key = subscriptionId + billing period start
const idempotencyKey = `${subscriptionId}-${periodStart.toISOString().slice(0, 10)}`
const existing = await prisma.invoice.findUnique({ where: { idempotencyKey } })
if (existing) return existing  // already billed this period — return existing
```

Never bill twice for the same period. Cron-based billing must check for existing invoice before creating.

### 4. Payment failure → grace period → retry → cancel (never immediate cancel)

```typescript
// Payment failed flow
// Step 1: Mark PAST_DUE (not CANCELLED) + log
// Step 2: Queue retry email to customer (Communication domain — Law 5)
// Step 3: Retry payment after 3 days (cron)
// Step 4: Retry again after 7 days
// Step 5: After max retries → CANCELLED + final notification
```

Immediate cancellation on first payment failure destroys customer trust. Grace period is a business rule, not optional.

### 5. Trial-to-paid conversion is a critical event

```typescript
// Must emit event — multiple domains care about this (Law 6)
// Payments: charge the card
// Communication: send "welcome to paid" email (async — Law 5)
// Analytics: record conversion (async — Law 5)
// NOT inline — queue all side effects
await billingQueue.add('trial-converted', { subscriptionId, userId, planId })
```

---

## Plan / Pricing Rules

### Feature gating per plan tier

```typescript
// Plan features stored as key-value, not as columns
model PlanFeature {
  id       String @id
  planId   String
  key      String  // e.g. "max_users", "api_access", "export_csv"
  value    String  // e.g. "50", "true", "false"

  @@unique([planId, key])
}
```

Never add boolean columns to Plan for each feature — use PlanFeature records. Plans change often.

### Seat-based pricing

```typescript
// Seat count checked at invite time, not just at billing
async function inviteTeamMember(orgId: string, email: string): Promise<void> {
  const sub = await getActiveSubscription(orgId)
  const seats = sub.plan.features.find(f => f.key === 'max_seats')?.value
  const currentMembers = await prisma.orgMember.count({ where: { orgId } })
  if (currentMembers >= parseInt(seats ?? '1')) {
    throw new ConflictException('Seat limit reached — upgrade your plan')
  }
  // proceed with invite
}
```

### Usage-based pricing

For metered features (API calls, storage, messages sent):
```typescript
// Record usage events — never update a running counter
await prisma.usageEvent.create({
  data: { subscriptionId, feature: 'api_calls', quantity: 1, recordedAt: new Date() }
})
// Aggregate at billing time — sum of events for the period
// UsageEvent is append-only (Law 3) — never modify
```

---

## Data Model

```prisma
model Plan {
  id              String        @id @default(cuid())
  name            String        @unique  // "Starter" | "Pro" | "Enterprise"
  slug            String        @unique  // "starter" | "pro" | "enterprise"
  priceMonthly    Decimal       @db.Decimal(10,2)  // 0 = free
  priceYearly     Decimal       @db.Decimal(10,2)
  trialDays       Int           @default(0)
  isActive        Boolean       @default(true)  // soft-disable old plans
  features        PlanFeature[]
  subscriptions   Subscription[]
  createdAt       DateTime      @default(now())
  updatedAt       DateTime      @updatedAt
}

model PlanFeature {
  id      String @id @default(cuid())
  planId  String
  key     String
  value   String
  plan    Plan   @relation(fields: [planId], references: [id])

  @@unique([planId, key])
  @@index([planId])
}

model Subscription {
  id                     String             @id @default(cuid())
  userId                 String
  organizationId         String?            // null for individual, set for org
  planId                 String
  status                 SubscriptionStatus
  billingCycle           BillingCycle       // MONTHLY | YEARLY
  pricePerPeriod         Decimal            @db.Decimal(10,2)  // snapshot — Law 3
  trialEndsAt            DateTime?
  currentPeriodStart     DateTime
  currentPeriodEnd       DateTime
  cancelledAt            DateTime?
  cancelReason           String?
  previousSubscriptionId String?            // upgrade/downgrade audit trail
  createdAt              DateTime           @default(now())
  updatedAt              DateTime           @updatedAt

  invoices               Invoice[]
  user                   User               @relation(fields: [userId], references: [id])
  plan                   Plan               @relation(fields: [planId], references: [id])

  @@index([userId])
  @@index([organizationId])
  @@index([status])
}

model Invoice {
  id              String        @id @default(cuid())
  subscriptionId  String
  idempotencyKey  String        @unique  // subscriptionId + periodStart
  amount          Decimal       @db.Decimal(10,2)
  currency        String        @default("USD")
  status          InvoiceStatus // DRAFT | OPEN | PAID | VOID | UNCOLLECTIBLE
  periodStart     DateTime
  periodEnd       DateTime
  paidAt          DateTime?
  lineItems       Json          // snapshot of what was billed — immutable (Law 3)
  createdAt       DateTime      @default(now())
  // no updatedAt — invoices are immutable (Law 3)

  @@index([subscriptionId])
}

model UsageEvent {
  id             String   @id @default(cuid())
  subscriptionId String
  feature        String
  quantity       Decimal  @db.Decimal(10,4)
  recordedAt     DateTime @default(now())
  // append-only — no updates (Law 3)

  @@index([subscriptionId, feature, recordedAt])
}

enum SubscriptionStatus { TRIALING ACTIVE PAST_DUE PAUSED CANCELLED }
enum BillingCycle       { MONTHLY YEARLY }
enum InvoiceStatus      { DRAFT OPEN PAID VOID UNCOLLECTIBLE }
```

---

## Common Mistakes

| Mistake | Correct pattern |
|---------|----------------|
| Cancel immediately on payment failure | Grace period → retries → then cancel |
| Store access level as boolean on User | Derive from live Subscription.status |
| Mutate Subscription on plan change | Close current, create new (immutable — Law 3) |
| Bill without idempotency check | Always check Invoice.idempotencyKey first |
| Gate features client-side | Always check server-side via requireActiveSubscription() |
| Add feature flag columns to Plan | Use PlanFeature key-value records |
| Hard delete cancelled subscriptions | Soft-delete only — keep for revenue analytics |
| Update usage counter in place | Append UsageEvent records, aggregate at bill time |

---

## Webhook Events to Handle (Stripe / payment provider)

| Event | Action |
|-------|--------|
| `invoice.payment_succeeded` | Mark subscription ACTIVE, record Invoice as PAID |
| `invoice.payment_failed` | Mark PAST_DUE, queue retry email |
| `customer.subscription.deleted` | Mark CANCELLED + cancelledAt |
| `customer.subscription.trial_will_end` | Queue warning email (3 days before) |

Always: verify webhook signature before processing. Record in ProcessedWebhookEvent for dedup. (Law 7)
