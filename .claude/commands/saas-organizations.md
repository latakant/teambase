# saas-organizations — Multi-Tenancy Standards
> Load this before working on: organizations, teams, members, invitations, roles, workspace isolation.
> Applies to: BACKEND_DEV, SENIOR_FULLSTACK, PRINCIPAL_ARCHITECT
> App type: SaaS / Subscription

---

## The Core Law: Tenant Isolation

**Every query on tenant-owned data MUST include organizationId.**
Missing organizationId on a WHERE clause = data leakage between tenants.

```typescript
// WRONG — missing tenant scope
const projects = await prisma.project.findMany({ where: { name: { contains: query } } })

// CORRECT — scoped to tenant
const projects = await prisma.project.findMany({
  where: { organizationId, name: { contains: query } }
})
```

This is not optional. Every service method that touches org-owned data receives `organizationId` as a parameter. Never infer it from context alone.

---

## Member Role State Machine

```
INVITED → ACTIVE → SUSPENDED → REMOVED
```

| From | To | Who | Condition |
|------|----|-----|-----------|
| — | INVITED | Admin | invite sent |
| INVITED | ACTIVE | Invitee | accepts invite link |
| INVITED | REMOVED | Admin / system | invite expired or cancelled |
| ACTIVE | SUSPENDED | Admin | temporary access removal |
| ACTIVE | REMOVED | User (self) OR Admin | leave or kicked |
| SUSPENDED | ACTIVE | Admin | restored |
| SUSPENDED | REMOVED | Admin | permanent removal |

**Terminal state:** REMOVED — member record kept for audit. Re-invite = new OrgMember record.

---

## Core Rules

### 1. Organization owns all its data — no cross-org access

```typescript
// Guard in every controller/service for org-scoped resources
async function assertOrgAccess(userId: string, organizationId: string): Promise<OrgMember> {
  const member = await prisma.orgMember.findUnique({
    where: { organizationId_userId: { organizationId, userId } },
  })
  if (!member || member.status !== 'ACTIVE') {
    throw new ForbiddenException('Not a member of this organization')
  }
  return member
}
```

### 2. Org roles are scoped — never platform-wide

```
OWNER > ADMIN > MEMBER > VIEWER (or custom)
```

Roles exist per-organization, not globally. A user can be ADMIN in org A and VIEWER in org B.

```typescript
// Check org-scoped role, never global role
async function requireOrgRole(userId: string, organizationId: string, minRole: OrgRole): Promise<void> {
  const member = await assertOrgAccess(userId, organizationId)
  const roleHierarchy = ['VIEWER', 'MEMBER', 'ADMIN', 'OWNER']
  if (roleHierarchy.indexOf(member.role) < roleHierarchy.indexOf(minRole)) {
    throw new ForbiddenException(`Requires ${minRole} role`)
  }
}
```

### 3. Invitation is a token — expires, single-use

```typescript
model OrgInvitation {
  id             String    @id @default(cuid())
  organizationId String
  email          String
  role           OrgRole
  token          String    @unique  // crypto.randomBytes(32).toString('hex')
  expiresAt      DateTime  // 7 days
  acceptedAt     DateTime?
  createdBy      String    // userId of inviter
  createdAt      DateTime  @default(now())

  @@index([organizationId])
  @@index([token])
}
```

- Token is single-use — mark acceptedAt on use, reject if already set
- Expired tokens rejected — check expiresAt server-side, always
- Never re-use tokens — generate new invitation if expired

### 4. Owner cannot be removed — ownership must be transferred first

```typescript
async function removeMember(requesterId: string, orgId: string, targetUserId: string): Promise<void> {
  await requireOrgRole(requesterId, orgId, 'ADMIN')
  const target = await assertOrgAccess(targetUserId, orgId)
  if (target.role === 'OWNER') {
    throw new ConflictException('Cannot remove owner — transfer ownership first')
  }
  // proceed with removal
}
```

### 5. Seat limit enforced at invite time (not just at billing)

```typescript
// Check before sending invite
const currentActiveMembers = await prisma.orgMember.count({
  where: { organizationId, status: 'ACTIVE' }
})
const seatLimit = await getOrgSeatLimit(organizationId)  // from subscription
if (currentActiveMembers >= seatLimit) {
  throw new ConflictException('Seat limit reached — upgrade to invite more members')
}
```

---

## Data Model

```prisma
model Organization {
  id          String      @id @default(cuid())
  name        String
  slug        String      @unique
  ownerId     String
  logoUrl     String?
  isActive    Boolean     @default(true)
  createdAt   DateTime    @default(now())
  updatedAt   DateTime    @updatedAt

  members     OrgMember[]
  invitations OrgInvitation[]

  @@index([ownerId])
}

model OrgMember {
  id             String    @id @default(cuid())
  organizationId String
  userId         String
  role           OrgRole   @default(MEMBER)
  status         MemberStatus @default(ACTIVE)
  joinedAt       DateTime  @default(now())
  removedAt      DateTime?

  organization   Organization @relation(fields: [organizationId], references: [id])

  @@unique([organizationId, userId])
  @@index([userId])
  @@index([organizationId])
}

model OrgInvitation {
  id             String    @id @default(cuid())
  organizationId String
  email          String
  role           OrgRole
  token          String    @unique
  expiresAt      DateTime
  acceptedAt     DateTime?
  createdBy      String
  createdAt      DateTime  @default(now())

  @@index([organizationId])
  @@index([email])
}

enum OrgRole      { OWNER ADMIN MEMBER VIEWER }
enum MemberStatus { INVITED ACTIVE SUSPENDED REMOVED }
```

---

## Common Mistakes

| Mistake | Correct pattern |
|---------|----------------|
| Global role check for org resources | Per-org role check via OrgMember |
| Query org data without organizationId | Always include organizationId in WHERE |
| Reuse expired invitation token | Generate new invitation |
| Allow removing the owner | Require ownership transfer first |
| Count seats at billing only | Enforce seat limit at invite time |
| Hard delete members | Soft delete — set status=REMOVED, keep record |
| One role system for both platform and org | Separate platform roles (auth) from org roles (OrgMember) |
