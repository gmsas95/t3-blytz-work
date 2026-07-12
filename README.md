> ⚠️ **Archived — early prototype.** This was an early iteration of the Blytz Marketplace. Superseded by [blytz.app.go](https://github.com/gmsas95/blytz.app.go).\n\n
## Project Overview

**Project Name**: BlytzWork - T3 Stack Rewrite  
**Location**: `/home/gmsas95/t3.blytz.work`  
**Status**: 🚀 Implementation Phase 1 - Project Initialization  
**Start Date**: January 1, 2026  
**Estimated Completion**: 8 weeks (February 26, 2026)

---

## What is T3 Stack?

The T3 Stack is a modern web development stack that prioritizes type-safety, developer experience, and rapid development:

- **T**ypescript - Type-safe JavaScript from database to UI
- **T**ailwind CSS - Utility-first CSS framework
- **t**RPC - End-to-end type-safe APIs
- **P**risma - Type-safe ORM for database operations
- **N**ext.js App Router - React framework with file-based routing
- **N**extAuth.js - Complete authentication solution

**Why Migrate to T3 Stack?**

1. **Type Safety**: Full type safety from Prisma → tRPC → React UI
2. **Simplified Architecture**: Single monorepo (no separate backend/frontend)
3. **Developer Experience**: Zero-config API routes, built-in auth flows
4. **Better Performance**: Next.js App Router optimized for production
5. **Modern Tooling**: State-of-the-art development experience

---

## Current vs T3 Stack Architecture

### Current Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Frontend                          │
│  Next.js 16.0.7  (3000)                             │
│  React 19.2.0                                          │
│  Radix UI Components (60+)                             │
│  Custom API Client (fetch wrapper)                       │
└─────────────────────────────────────────────────────────────┘
                            │ HTTP
                            │ REST API
┌─────────────────────────────────────────────────────────────┐
│                    Backend                             │
│  Fastify 5.6.0 (3001)                                 │
│  Socket.IO Server (chat, notifications)                  │
│  Firebase Admin SDK (auth sync)                          │
│  Stripe Webhooks (payments)                               │
└─────────────────────────────────────────────────────────────┘
                            │
                    ┌───────┴───────┐
                    │               │
            ┌───────▼────┐   ┌───▼──────────┐
            │ PostgreSQL   │   │ Cloudflare R2 │
            │   + Prisma  │   │  (file storage)│
            └─────────────┘   └────────────────┘
```

### T3 Stack Architecture (Target)

```
┌─────────────────────────────────────────────────────────────┐
│              Next.js App Router (Unified)               │
│  Frontend + tRPC + API Routes (3000)                    │
│                                                             │
│  ┌────────────┬─────────────┬────────────────────┐      │
│  │  Frontend  │   tRPC     │  API Routes       │      │
│  │   Pages    │  Backend    │  (webhooks)       │      │
│  │            │            │                    │      │
│  │ React 19   │  Server     │  Stripe            │      │
│  │ Radix UI   │  Procedures │  Webhooks          │      │
│  └────────────┴─────────────┴────────────────────┘      │
└─────────────────────────────────────────────────────────────┘
                            │
                    ┌───────┴───────┐
                    │               │
            ┌───────▼────┐   ┌───▼──────────┐
            │ PostgreSQL   │   │ Cloudflare R2 │
            │   + Prisma  │   │  (file storage)│
            └─────────────┘   └────────────────┘
```

**Key Changes:**
- ❌ Remove separate Fastify backend server
- ❌ Remove Socket.IO server (deferred)
- ❌ Remove Firebase Auth (replaced with NextAuth.js)
- ✅ Unified Next.js server (frontend + backend)
- ✅ Type-safe tRPC for API
- ✅ NextAuth.js for authentication

---

## Tech Stack Comparison

| Layer | Current | T3 Stack | Migration Complexity |
|-------|---------|-----------|---------------------|
| **Runtime** | Node.js 20 | Node.js 20 | Low |
| **Framework** | Next.js 16 + Fastify | Next.js 16 (unified) | Medium |
| **Language** | TypeScript 5.9.3 | TypeScript 5.x | Low |
| **Database** | PostgreSQL 15 + Prisma 6.19 | PostgreSQL + Prisma | Low |
| **ORM** | Prisma | Prisma (same) | None |
| **API** | REST + custom client | tRPC (type-safe) | Medium |
| **Auth** | Firebase Admin SDK | NextAuth.js | High |
| **Real-time** | Socket.IO | Defered (later: Pusher) | N/A |
| **Payments** | Stripe + Fastify | Stripe + API Routes | Low |
| **UI Library** | Radix UI (60 components) | Radix UI (keep same) | Low |
| **Styling** | Tailwind CSS | Tailwind CSS (same config) | None |
| **State Mgmt** | React Hook Form | TanStack Query + tRPC | Medium |
| **Type Safety** | Partial (custom types) | Full (DB → UI) | Low |
| **Deployment** | 2 containers (frontend + backend) | 1 container (unified) | Low |

---

## Migration Strategy

### Approach: Fresh Build (Clean Slate)

**Decision**: Complete rewrite with fresh database start

**Rationale:**
- No legacy code to maintain
- Clean architecture from day one
- Faster development (no backward compatibility)
- Better opportunity to refine data models
- Simpler testing (no migration edge cases)

**Trade-off:**
- ❌ No existing user data
- ❌ No existing data migration
- ✅ Clean database schema
- ✅ Modern design patterns from start

---

## Project Structure

```
t3.blytz.work/
├── src/
│   ├── app/                          # Next.js App Router
│   │   ├── (auth)/                    # Auth route group
│   │   │   ├── login/
│   │   │   │   └── page.tsx
│   │   │   ├── register/
│   │   │   │   └── page.tsx
│   │   │   └── layout.tsx
│   │   ├── (dashboard)/                # Dashboard route group (protected)
│   │   │   ├── va/
│   │   │   │   ├── dashboard/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── onboarding/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── profile/
│   │   │   │   │   ├── create/
│   │   │   │   │   │   └── page.tsx
│   │   │   │   │   └── [id]/
│   │   │   │   │       └── page.tsx
│   │   │   ├── employer/
│   │   │   │   ├── dashboard/
│   │   │   │   │   └── page.tsx
│   │   │   │   ├── onboarding/
│   │   │   │   │   └── page.tsx
│   │   │   │   └── discover/
│   │   │   │       └── page.tsx
│   │   │   └── layout.tsx
│   │   ├── contract/
│   │   │   └── [id]/
│   │   │       └── page.tsx
│   │   ├── api/                        # API routes (webhooks only)
│   │   │   └── stripe/
│   │   │       └── webhook/
│   │   │           └── route.ts
│   │   ├── layout.tsx                  # Root layout
│   │   ├── page.tsx                   # Homepage
│   │   ├── pricing/
│   │   │   └── page.tsx
│   │   ├── faq/
│   │   │   └── page.tsx
│   │   ├── about/
│   │   │   └── page.tsx
│   │   ├── privacy/
│   │   │   └── page.tsx
│   │   └── terms/
│   │       └── page.tsx
│   ├── server/                        # tRPC backend
│   │   ├── trpc.ts                    # Main tRPC router setup
│   │   ├── context.ts                 # tRPC context (auth + prisma)
│   │   ├── auth.ts                    # Auth procedures
│   │   ├── va.ts                      # VA procedures
│   │   ├── company.ts                 # Company procedures
│   │   ├── jobs.ts                    # Job marketplace procedures
│   │   ├── contracts.ts               # Contract procedures
│   │   ├── payments.ts                # Payment procedures
│   │   ├── upload.ts                  # File upload procedures
│   │   └── index.ts                   # Combined router
│   ├── components/                    # React components
│   │   ├── auth/                      # Auth components (keep existing)
│   │   ├── ui/                        # Radix UI components (keep existing)
│   │   ├── dashboard/                 # Dashboard components
│   │   ├── forms/                     # Form components
│   │   └── layouts/                   # Layout components
│   ├── lib/                          # Utilities
│   │   ├── trpc.ts                   # tRPC client setup
│   │   ├── prisma.ts                 # Prisma client
│   │   ├── auth.ts                   # Auth helpers
│   │   ├── utils.ts                  # Helper functions
│   │   └── api.ts                    # Legacy API client (remove during migration)
│   ├── styles/                       # Styles
│   │   ├── globals.css                # Global styles (keep existing)
│   │   └── tailwind.config.ts        # Tailwind config (copy from blytz.work)
│   └── types/                        # TypeScript types
│       └── index.ts                  # Shared types
├── prisma/
│   ├── schema.prisma                  # Prisma schema (simplified)
│   └── migrations/                    # Database migrations
├── public/                           # Static assets
│   ├── images/                        # Images (copy from blytz.work)
│   └── icons/                        # Icons
├── .env.local                        # Environment variables
├── .env.example                     # Environment template
├── .gitignore
├── next.config.js                    # Next.js config
├── tailwind.config.ts               # Tailwind config
├── tsconfig.json                    # TypeScript config
└── package.json
```

---

## Implementation Phases

### Phase 1: Project Initialization (Week 1)

**Goal**: Set up T3 stack foundation with core configuration

**Tasks:**
- [ ] Create T3 stack project using `create-t3-app`
- [ ] Copy existing Radix UI components from blytz.work
- [ ] Copy Tailwind config and global styles (preserve design)
- [ ] Install additional dependencies (bcryptjs, Radix UI icons)
- [ ] Configure Prisma schema (simplified version)
- [ ] Set up PostgreSQL database
- [ ] Configure environment variables
- [ ] Run initial migrations

**Deliverables:**
- ✅ Working T3 project structure
- ✅ Database with all tables
- ✅ Tailwind configured with existing design tokens
- ✅ Radix UI components copied

**Duration**: 1 week  
**Dependencies**: None

---

### Phase 2: Authentication Migration (Week 2)

**Goal**: Replace Firebase Auth with NextAuth.js

**Tasks:**
- [ ] Configure NextAuth.js with Credentials provider
- [ ] Configure NextAuth.js with Google OAuth
- [ ] Update Prisma schema for NextAuth.js (Account, Session, VerificationToken models)
- [ ] Create login page (`/login`)
- [ ] Create registration page (`/register`)
- [ ] Create registration API route (password hashing with bcrypt)
- [ ] Implement protected route middleware
- [ ] Test authentication flow (signup, login, Google OAuth)
- [ ] Test role-based redirection (VA vs Company dashboards)

**Deliverables:**
- ✅ NextAuth.js configured
- ✅ Working login/register pages
- ✅ Google OAuth integration
- ✅ Protected routes with middleware
- ✅ Session management

**Duration**: 1 week  
**Dependencies**: Phase 1

---

### Phase 3: tRPC API Implementation (Weeks 3-4)

**Goal**: Implement all backend procedures using tRPC

**Tasks:**

**Week 3: Core Procedures**
- [ ] Set up tRPC context with Prisma and auth
- [ ] Create protected procedure helper (middleware)
- [ ] Implement auth procedures (`getProfile`, `updateProfile`, `completeProfile`)
- [ ] Implement VA procedures (`getProfile`, `createProfile`, `updateProfile`, `browseProfiles`)
- [ ] Implement company procedures (`createProfile`, `getProfile`)

**Week 4: Business Logic Procedures**
- [ ] Implement jobs procedures (`createJob`, `browseJobs`)
- [ ] Implement contracts procedures (`createContract`, `getContracts`)
- [ ] Implement upload procedures (`generatePresignedUrl`)
- [ ] Implement payments procedures (`createCheckoutSession`)
- [ ] Test all procedures independently

**Deliverables:**
- ✅ Complete tRPC backend
- ✅ All procedures tested
- ✅ Type-safe API from database to server

**Duration**: 2 weeks  
**Dependencies**: Phase 2

---

### Phase 4: Frontend Migration (Weeks 5-6)

**Goal**: Migrate all pages to use tRPC and NextAuth.js

**Tasks:**

**Week 5: Core Pages**
- [ ] Set up tRPC client
- [ ] Wrap app with SessionProvider and tRPC Provider
- [ ] Migrate VA dashboard page
- [ ] Migrate employer dashboard page
- [ ] Migrate VA onboarding page
- [ ] Migrate employer onboarding page
- [ ] Update all components to use tRPC hooks

**Week 6: Complete Pages**
- [ ] Migrate VA profile creation page
- [ ] Migrate VA profile detail page
- [ ] Migrate employer discover page
- [ ] Migrate contract pages
- [ ] Migrate public pages (home, pricing, FAQ, about)
- [ ] Update navigation and layout components
- [ ] Test all page flows

**Deliverables:**
- ✅ All pages migrated to tRPC
- ✅ No custom API calls remaining
- ✅ Full type safety from database to UI
- ✅ All existing designs preserved

**Duration**: 2 weeks  
**Dependencies**: Phase 3

---

### Phase 5: Payments Integration (Week 7)

**Goal**: Set up Stripe payment processing

**Tasks:**
- [ ] Create Stripe webhook handler (`/api/stripe/webhook`)
- [ ] Implement checkout session creation procedure
- [ ] Handle `checkout.session.completed` event
- [ ] Handle `invoice.payment_succeeded` event
- [ ] Update contract status on payment
- [ ] Test payment flow end-to-end
- [ ] Configure Stripe test environment

**Deliverables:**
- ✅ Stripe webhooks configured
- ✅ Payment checkout flow working
- ✅ Contract status automation

**Duration**: 1 week  
**Dependencies**: Phase 3

---

### Phase 6: Testing & Deployment (Week 8)

**Goal**: Ensure production readiness

**Tasks:**
- [ ] Run full TypeScript type check
- [ ] Test all user flows manually
- [ ] Create Docker configuration
- [ ] Set up environment for production
- [ ] Deploy to staging environment
- [ ] Final testing on staging
- [ ] Documentation updates

**Deliverables:**
- ✅ Zero TypeScript errors
- ✅ All flows tested
- ✅ Docker configuration ready
- ✅ Staging deployment successful

**Duration**: 1 week  
**Dependencies**: All phases

---

## Design Preservation Plan

### Colors (from blytz.work/frontend/src/styles/globals.css)

Preserve exact color scheme:
- Primary colors
- Secondary colors
- Accent colors
- Background colors
- Text colors

### Typography

Preserve fonts and sizes:
- Font families
- Heading weights
- Body text sizes
- Line heights

### Components

Keep existing Radix UI components with same styling:
- Buttons (primary, secondary, outline)
- Cards (with shadows, borders)
- Form inputs (same padding, border radius)
- Modals (same animation, backdrop)
- Tables (same spacing, borders)

### Layouts

Preserve spacing and layout:
- Container max-widths
- Padding/margin values
- Grid/flex layouts
- Responsive breakpoints

---

## Prisma Schema Simplification

### Changes from Current Schema

**Remove:**
- `@@map("table_name")` annotations (use default schema)
- `@@schema("blytz_hire")` annotations (use default schema)
- Chat-related models (Message, Chat) - deferred
- Notification model - deferred

**Add for NextAuth.js:**
```prisma
model Account {
  id                String  @id @default(cuid())
  userId            String
  type              String
  provider          String
  providerAccountId String
  refresh_token     String? @db.Text
  access_token      String? @db.Text
  expires_at        Int?
  token_type        String?
  scope             String?
  id_token          String? @db.Text
  session_state      String?

  user User @relation(fields: [userId], references: [id], onDelete: Cascade)

  @@unique([provider, providerAccountId])
}

model Session {
  id           String   @id @default(cuid())
  sessionToken String   @unique
  userId       String
  expires      DateTime
  user         User     @relation(fields: [userId], references: [id], onDelete: Cascade)
}

model VerificationToken {
  identifier String
  token      String   @unique
  expires    DateTime

  @@unique([identifier, token])
}
```

**Add password field to User:**
```prisma
model User {
  // ... existing fields
  password    String?  // Hashed password for credentials auth
  image       String?  // Avatar URL (for NextAuth.js)
  name        String?  // Display name (for NextAuth.js)
}
```

---

## Environment Variables

Create `.env.local`:

```bash
# Database
DATABASE_URL="postgresql://blytz:blytz@localhost:5432/blytz"

# NextAuth.js
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="<generate-with-openssl-rand-base64-32>"

# Google OAuth
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Stripe
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."

# Cloudflare R2 (file uploads)
AWS_ACCESS_KEY_ID="your-r2-access-key"
AWS_SECRET_ACCESS_KEY="your-r2-secret-key"
AWS_S3_BUCKET="blytz-uploads"
AWS_REGION="auto"
AWS_S3_BUCKET_URL="https://<account-id>.r2.cloudflarestorage.com"
```

---

## Development Commands

### Setup
```bash
cd /home/gmsas95/t3.blytz.work
npm install
npm run db:push  # Push Prisma schema to database
npm run dev       # Start development server
```

### Database
```bash
npm run db:push    # Push schema changes
npm run db:studio  # Open Prisma Studio
npm run db:seed    # Seed database (when implemented)
```

### Development
```bash
npm run dev         # Start Next.js dev server (http://localhost:3000)
npm run build       # Build for production
npm run start       # Start production server
```

### Type Checking
```bash
npm run type-check   # Run TypeScript type check
npx tsc --noEmit  # Alternative type check
```

### Docker
```bash
docker-compose up -d    # Start all services
docker-compose down     # Stop all services
docker-compose logs -f  # View logs
```

---

## Testing Checklist

### Authentication Flow
- [ ] User can register with email/password
- [ ] User can login with email/password
- [ ] User can login with Google OAuth
- [ ] Session persists on page refresh
- [ ] User can logout
- [ ] Protected routes redirect to login
- [ ] User redirected to correct dashboard based on role
- [ ] Token refresh works correctly

### VA Flows
- [ ] VA can create profile
- [ ] VA can update profile
- [ ] VA can browse other VA profiles
- [ ] VA dashboard loads profile data
- [ ] VA onboarding flow completes
- [ ] Profile image uploads to R2
- [ ] Resume uploads to R2

### Employer Flows
- [ ] Company can create profile
- [ ] Company can browse VA profiles
- [ ] Company can filter/search VAs
- [ ] Employer dashboard loads data
- [ ] Employer onboarding flow completes
- [ ] Job posting works
- [ ] Contract creation works

### Payment Flow
- [ ] Checkout session created successfully
- [ ] Stripe checkout UI loads
- [ ] Payment completes
- [ ] Webhook receives event
- [ ] Contract status updates
- [ ] Payment record created

---

## Timeline Summary

| Week | Phase | Status | Estimated Completion |
|------|--------|--------|---------------------|
| 1 | Project Initialization | ⏳ Not Started | January 7, 2026 |
| 2 | Authentication Migration | ⏳ Not Started | January 14, 2026 |
| 3-4 | tRPC API Implementation | ⏳ Not Started | January 28, 2026 |
| 5-6 | Frontend Migration | ⏳ Not Started | February 11, 2026 |
| 7 | Payments Integration | ⏳ Not Started | February 18, 2026 |
| 8 | Testing & Deployment | ⏳ Not Started | February 26, 2026 |

**Total Duration**: 8 weeks  
**Project Start**: January 1, 2026  
**Estimated Completion**: February 26, 2026

---

## Key Decisions & Rationale

### 1. Fresh Build (No Data Migration)
**Decision**: Start with empty database  
**Rationale**: 
- Avoid complex migration edge cases
- Cleaner architecture from day one
- Faster development (no backward compatibility concerns)
- Opportunity to refine data models

**Trade-off**: No existing users/data

---

### 2. Keep Radix UI Components
**Decision**: Preserve existing design system  
**Rationale**:
- Maintain brand consistency
- Preserve design tokens (colors, spacing, typography)
- Reduce frontend work (no component rewrites)
- Focus on backend/data layer changes

**Implementation**: Copy all Radix UI components with exact styling

---

### 3. NextAuth.js with Credentials + Google OAuth
**Decision**: Use NextAuth.js with password-based and Google auth  
**Rationale**:
- Type-safe, integrated with T3 stack
- Built-in session management
- OAuth providers supported out-of-box
- Good developer experience

**Trade-off**: Users need to set passwords (can't migrate Firebase users)

---

### 4. Skip Real-time Features (Deferred)
**Decision**: Defer chat/notifications to later phase  
**Rationale**:
- Faster to market with core features
- tRPC subscriptions still experimental
- Can add Pusher or similar later

**Future**: Consider Pusher ($25/mo) or tRPC subscriptions when stable

---

### 5. Single Unified Server
**Decision**: Single Next.js server (no separate backend)  
**Rationale**:
- Simpler deployment (1 container vs 2)
- Type safety across full stack
- Faster API calls (no network boundary)
- Lower infrastructure cost

**Implementation**: All backend logic in tRPC server procedures

---

## Risks & Mitigations

### Risk 1: Timeline Slippage
**Impact**: May exceed 8-week timeline  
**Probability**: Medium  
**Mitigation**:
- Start with MVP features first
- Defer nice-to-have features
- Regular progress checkpoints

---

### Risk 2: Design Inconsistency
**Impact**: Design may differ from original  
**Probability**: Low  
**Mitigation**:
- Copy exact Tailwind config
- Copy all Radix UI components
- Use existing CSS variables
- Regular design reviews

---

### Risk 3: Missing Features
**Impact**: Users may miss existing functionality  
**Probability**: Low  
**Mitigation**:
- Document all features before migration
- Test all existing user flows
- Feature parity checklist

---

### Risk 4: Stripe Integration Issues
**Impact**: Payments may not work correctly  
**Probability: Medium  
**Mitigation**:
- Use existing Stripe test keys
- Test webhook thoroughly
- Have fallback payment method for testing

---

### Risk 5: User Adoption (Fresh Start)
**Impact**: Users may not want to re-register  
**Probability**: High  
**Mitigation**:
- Clear communication about fresh start
- Import data from Firebase if needed later
- Offer incentives for early adopters

---

## Success Criteria

Project is successful when:

### Technical
- [ ] Zero TypeScript errors
- [ ] All tests passing
- [ ] Type safety from database to UI
- [ ] Performance metrics met (<100ms API calls)
- [ ] Clean codebase (no legacy code)

### Business
- [ ] User registration flow works
- [ ] User login flow works
- [ ] VA profile creation works
- [ ] Company profile creation works
- [ ] Job posting works
- [ ] Contract creation works
- [ ] Payment flow works

### Design
- [ ] Design matches original (colors, fonts, spacing)
- [ ] All Radix UI components preserved
- [ ] Responsive design maintained
- [ ] Accessibility standards met

### Deployment
- [ ] Docker configuration working
- [ ] Staging deployment successful
- [ ] Environment variables configured
- [ ] Health checks passing

---

## Next Steps

1. **Initialize T3 Project**
   ```bash
   cd /home/gmsas95/t3.blytz.work
   npx create-t3-app@latest . \
     --nextAuth \
     --prisma \
     --tailwind \
     --typescript \
     --trpc \
     --app \
     --src-dir=false
   ```

2. **Copy Existing Assets**
   - Copy Radix UI components from blytz.work
   - Copy Tailwind config
   - Copy global styles
   - Copy utility functions

3. **Configure Prisma Schema**
   - Simplify existing schema
   - Add NextAuth.js models
   - Remove chat/notification models

4. **Set Up Database**
   - Create PostgreSQL database
   - Run initial migrations

5. **Configure Authentication**
   - Set up NextAuth.js
   - Configure Google OAuth

---

## Documentation

### Developer Guides
- `SETUP.md` - How to set up development environment
- `DEPLOYMENT.md` - How to deploy to production
- `API.md` - tRPC API documentation
- `COMPONENTS.md` - Component library documentation

### Architecture Docs
- `ARCHITECTURE.md` - System architecture overview
- `DATABASE.md` - Database schema documentation
- `AUTH.md` - Authentication flow documentation

### Progress Tracking
- `IMPLEMENTATION.md` - Implementation progress
- `MIGRATION.md` - Migration checklist
- `ISSUES.md` - Known issues and solutions

---

## Contact & Support

**Project Lead**: gmsas95  
**Primary Repository**: `/home/gmsas95/t3.blytz.work`  
**Reference Repository**: `/home/gmsas95/blytz.work` (current version)  
**Documentation Location**: This file (`README.md`)

---

**Last Updated**: January 1, 2026  
**Version**: 1.0  
**Status**: 🚀 Ready to begin Phase 1
