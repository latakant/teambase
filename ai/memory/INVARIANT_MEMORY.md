# TEAMBASE — Hard Invariants (HALT on violation)

1. Org slug is immutable after creation
2. Seat check inside $transaction — no race condition on invites
3. Never check plan features directly — always check subscription status
4. Subscription cancellation: mark CANCELLED, never delete record
5. Downgrade blocked if current seat count exceeds new plan limit
