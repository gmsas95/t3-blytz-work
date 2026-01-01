# T3 Stack Migration Checklist

## Phase 1: Project Initialization (Week 1)

### Week 1: Day 1-2 - Project Setup
- [ ] Navigate to `/home/gmsas95/t3.blytz.work`
- [ ] Run `npx create-t3-app@latest .`
- [ ] Select options during setup:
  - [ ] TypeScript: Yes
  - [ ] Tailwind CSS: Yes
  - [ ] tRPC: Yes
  - [ ] Prisma: Yes
  - [ ] NextAuth: Yes
  - [ ] App Router: Yes
  - [ ] src directory: No
- [ ] Verify project structure created
- [ ] Run `npm install` to install dependencies
- [ ] Install additional dependencies: `bcryptjs`, `@radix-ui/react-icons`
- [ ] Install dev dependencies: `@types/bcryptjs`

### Week 1: Day 3-4 - Copy Existing Assets
- [ ] Copy Radix UI components from `/home/gmsas95/blytz.work/frontend/src/components`
- [ ] Copy Tailwind config from `/home/gmsas95/blytz.work/frontend/tailwind.config.ts`
- [ ] Copy global styles from `/home/gmsas95/blytz.work/frontend/src/styles/globals.css`
- [ ] Copy utility functions from `/home/gmsas95/blytz.work/frontend/src/lib`
- [ ] Copy types from `/home/gmsas95/blytz.work/frontend/src/types.ts`
- [ ] Copy public assets from `/home/gmsas95/blytz.work/frontend/public`
- [ ] Verify all files copied correctly

### Week 1: Day 5-7 - Database Setup
- [ ] Copy current Prisma schema to `/home/gmsas95/t3.blytz.work/prisma/schema.prisma`
- [ ] Simplify schema (remove schema annotations, remove chat models)
- [ ] Add NextAuth.js models (Account, Session, VerificationToken)
- [ ] Add password field to User model
- [ ] Create PostgreSQL database
- [ ] Set up DATABASE_URL in `.env.local`
- [ ] Run `npx prisma generate`
- [ ] Run `npm run db:push` to create tables
- [ ] Verify tables created using `npm run db:studio`

### Week 1: Day 7 - Environment Configuration
- [ ] Create `.env.local` file
- [ ] Add DATABASE_URL
- [ ] Generate and add NEXTAUTH_SECRET
- [ ] Add NEXTAUTH_URL
- [ ] Add Google OAuth credentials (if available)
- [ ] Add Stripe test keys
- [ ] Add Cloudflare R2 credentials
- [ ] Verify `.env.local` is in `.gitignore`
- [ ] Test environment variables are loaded

### Week 1: Completion Criteria
- [ ] T3 project created successfully
- [ ] All dependencies installed
- [ ] Database set up with all tables
- [ ] Environment variables configured
- [ ] Development server starts without errors
- [ ] Tailwind CSS working
- [ ] Radix UI components rendering

---

## Phase 2: Authentication Migration (Week 2)

### Week 2: Day 1-2 - NextAuth.js Configuration
- [ ] Create `src/app/api/auth/[...nextauth]/route.ts`
- [ ] Configure NextAuth options
- [ ] Set up Google provider
- [ ] Set up Credentials provider
- [ ] Configure session strategy (jwt)
- [ ] Add callbacks for session and jwt
- [ ] Configure pages (signIn, signOut, error)
- [ ] Test NextAuth configuration compiles

### Week 2: Day 3-4 - Database Models for Auth
- [ ] Update Prisma User model (add password, image, name fields)
- [ ] Add Account model
- [ ] Add Session model
- [ ] Add VerificationToken model
- [ ] Set up relations (User ← Account, User ← Session)
- [ ] Run `npx prisma generate`
- [ ] Run `npm run db:push`
- [ ] Verify auth tables created

### Week 2: Day 5-6 - Auth Pages
- [ ] Create `/login` page (`src/app/login/page.tsx`)
- [ ] Copy existing auth form design from blytz.work
- [ ] Implement email/password login
- [ ] Implement Google OAuth button
- [ ] Add error handling
- [ ] Add loading states
- [ ] Test login flow works

### Week 2: Day 6-7 - Registration & Protected Routes
- [ ] Create `/register` page (`src/app/register/page.tsx`)
- [ ] Copy existing registration form design
- [ ] Implement registration with password hashing
- [ ] Create registration API route (`/api/register/route.ts`)
- [ ] Add bcrypt password hashing
- [ ] Add user creation to database
- [ ] Create `src/middleware.ts`
- [ ] Configure protected route middleware
- [ ] Test registration flow works
- [ ] Test protected routes redirect to login

### Week 2: Completion Criteria
- [ ] NextAuth.js configured and working
- [ ] Login page functional
- [ ] Registration page functional
- [ ] Google OAuth working
- [ ] Password hashing implemented
- [ ] Protected routes middleware working
- [ ] Session management working
- [ ] User can login and access protected routes

---

## Phase 3: tRPC API Implementation (Weeks 3-4)

### Week 3: Day 1-2 - tRPC Context & Setup
- [ ] Create `src/server/trpc.ts` (tRPC router setup)
- [ ] Create `src/server/context.ts` (auth + prisma context)
- [ ] Set up tRPC init with superjson transformer
- [ ] Create protected procedure helper (middleware)
- [ ] Create `src/server/index.ts` (combined router)
- [ ] Test tRPC context compiles

### Week 3: Day 3-4 - Auth Procedures
- [ ] Create `src/server/auth.ts`
- [ ] Implement `getProfile` procedure (protected)
- [ ] Implement `updateProfile` procedure (protected)
- [ ] Implement `completeProfile` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Test auth procedures independently

### Week 3: Day 5-7 - VA Procedures
- [ ] Create `src/server/va.ts`
- [ ] Implement `getProfile` procedure (public + protected)
- [ ] Implement `createProfile` procedure (protected)
- [ ] Implement `updateProfile` procedure (protected)
- [ ] Implement `browseProfiles` procedure (public)
- [ ] Implement `getStats` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Test all VA procedures

### Week 4: Day 1-2 - Company Procedures
- [ ] Create `src/server/company.ts`
- [ ] Implement `getProfile` procedure (public + protected)
- [ ] Implement `createProfile` procedure (protected)
- [ ] Implement `updateProfile` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Test all company procedures

### Week 4: Day 3-4 - Jobs Procedures
- [ ] Create `src/server/jobs.ts`
- [ ] Implement `createJob` procedure (protected)
- [ ] Implement `browseJobs` procedure (public)
- [ ] Implement `getJobDetails` procedure (public)
- [ ] Implement `updateJob` procedure (protected)
- [ ] Implement `deleteJob` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Test all jobs procedures

### Week 4: Day 5-6 - Contracts & Upload
- [ ] Create `src/server/contracts.ts`
- [ ] Implement `createContract` procedure (protected)
- [ ] Implement `getContracts` procedure (protected)
- [ ] Implement `updateContract` procedure (protected)
- [ ] Create `src/server/upload.ts`
- [ ] Implement `generatePresignedUrl` procedure (protected)
- [ ] Implement `deleteFile` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Test all contracts and upload procedures

### Week 4: Day 7 - Payments & Combine Routers
- [ ] Create `src/server/payments.ts`
- [ ] Implement `createCheckoutSession` procedure (protected)
- [ ] Implement `getPayments` procedure (protected)
- [ ] Add Zod validation for all inputs
- [ ] Update `src/server/index.ts` to combine all routers
- [ ] Test all payments procedures
- [ ] Test combined router compiles

### Week 3-4: Completion Criteria
- [ ] tRPC context set up with auth and prisma
- [ ] Protected procedure helper working
- [ ] All auth procedures implemented and tested
- [ ] All VA procedures implemented and tested
- [ ] All company procedures implemented and tested
- [ ] All jobs procedures implemented and tested
- [ ] All contracts procedures implemented and tested
- [ ] All upload procedures implemented and tested
- [ ] All payments procedures implemented and tested
- [ ] All routers combined into main router
- [ ] Type safety verified (no any types)

---

## Phase 4: Frontend Migration (Weeks 5-6)

### Week 5: Day 1-2 - tRPC Client Setup
- [ ] Create `src/lib/trpc.ts` (tRPC client setup)
- [ ] Configure superjson transformer
- [ ] Set up tRPC React hooks
- [ ] Update `src/app/layout.tsx` with tRPC Provider
- [ ] Update `src/app/layout.tsx` with SessionProvider
- [ ] Update `src/app/layout.tsx` with QueryClientProvider
- [ ] Test providers render correctly

### Week 5: Day 3-4 - VA Dashboard Migration
- [ ] Migrate `src/app/va/dashboard/page.tsx`
- [ ] Replace custom API calls with tRPC hooks
- [ ] Replace useState for data with tRPC useQuery
- [ ] Keep existing dashboard component design
- [ ] Test VA dashboard loads profile data
- [ ] Test VA dashboard loads stats

### Week 5: Day 5-6 - Employer Dashboard Migration
- [ ] Migrate `src/app/employer/dashboard/page.tsx`
- [ ] Replace custom API calls with tRPC hooks
- [ ] Keep existing dashboard component design
- [ ] Test employer dashboard loads company data
- [ ] Test employer dashboard loads VA list

### Week 5: Day 7 - Onboarding Pages
- [ ] Migrate `src/app/va/onboarding/page.tsx`
- [ ] Replace custom API calls with tRPC mutations
- [ ] Keep existing onboarding design
- [ ] Test VA onboarding flow
- [ ] Migrate `src/app/employer/onboarding/page.tsx`
- [ ] Replace custom API calls with tRPC mutations
- [ ] Keep existing onboarding design
- [ ] Test employer onboarding flow

### Week 6: Day 1-2 - Profile Pages
- [ ] Migrate `src/app/va/profile/create/page.tsx`
- [ ] Replace file upload with tRPC procedure
- [ ] Replace profile creation with tRPC mutation
- [ ] Keep existing profile creation design
- [ ] Test VA profile creation
- [ ] Migrate `src/app/va/profile/[id]/page.tsx`
- [ ] Replace custom API calls with tRPC hooks
- [ ] Keep existing profile detail design
- [ ] Test VA profile detail page

### Week 6: Day 3-4 - Discover & Contract Pages
- [ ] Migrate `src/app/employer/discover/page.tsx`
- [ ] Replace VA browse with tRPC useQuery
- [ ] Replace filters with tRPC inputs
- [ ] Keep existing discover design
- [ ] Test employer discover page
- [ ] Migrate `src/app/contract/[id]/page.tsx`
- [ ] Replace contract details with tRPC useQuery
- [ ] Keep existing contract design
- [ ] Test contract detail page

### Week 6: Day 5-6 - Public Pages
- [ ] Migrate `src/app/page.tsx` (homepage)
- [ ] Replace data fetching with tRPC hooks
- [ ] Keep existing homepage design
- [ ] Migrate `src/app/pricing/page.tsx`
- [ ] Keep existing pricing design
- [ ] Migrate `src/app/faq/page.tsx`
- [ ] Keep existing FAQ design
- [ ] Migrate `src/app/about/page.tsx`
- [ ] Keep existing about design

### Week 6: Day 7 - Layout & Navigation
- [ ] Update `src/app/layout.tsx` root layout
- [ ] Update navigation components
- [ ] Update footer components
- [ ] Update header components
- [ ] Test navigation works correctly
- [ ] Test protected routes redirect
- [ ] Test role-based redirection

### Week 5-6: Completion Criteria
- [ ] tRPC client configured
- [ ] All pages migrated to tRPC
- [ ] No custom API calls remaining
- [ ] All existing designs preserved
- [ ] Type safety from database to UI verified
- [ ] All user flows tested

---

## Phase 5: Payments Integration (Week 7)

### Week 7: Day 1-2 - Stripe Setup
- [ ] Verify Stripe test keys in `.env.local`
- [ ] Install Stripe Node.js SDK if not present
- [ ] Create Stripe client instance
- [ ] Test Stripe API connection
- [ ] Verify test mode is enabled

### Week 7: Day 3-4 - Stripe Webhooks
- [ ] Create `src/app/api/stripe/webhook/route.ts`
- [ ] Set up webhook signature verification
- [ ] Implement `checkout.session.completed` handler
- [ ] Implement `invoice.payment_succeeded` handler
- [ ] Add error handling for webhook
- [ ] Test webhook with Stripe CLI

### Week 7: Day 5-6 - Payment Procedures
- [ ] Update `src/server/payments.ts`
- [ ] Implement `createCheckoutSession` procedure
- [ ] Add Stripe checkout session creation
- [ ] Configure success and cancel URLs
- [ ] Add metadata for contract tracking
- [ ] Test checkout session creation

### Week 7: Day 6-7 - Contract Payment Integration
- [ ] Update contract payment logic
- [ ] Link payment to contract in database
- [ ] Update contract status on payment success
- [ ] Create payment record in database
- [ ] Test end-to-end payment flow

### Week 7: Completion Criteria
- [ ] Stripe webhooks configured
- [ ] Payment checkout flow working
- [ ] Contract status updates on payment
- [ ] Payment records created correctly
- [ ] Error handling in place
- [ ] All payment flows tested

---

## Phase 6: Testing & Deployment (Week 8)

### Week 8: Day 1-2 - Type Safety & Linting
- [ ] Run `npm run type-check` (zero errors)
- [ ] Run `npm run lint` (zero errors)
- [ ] Fix any TypeScript errors
- [ ] Fix any linting errors
- [ ] Verify no `any` types used
- [ ] Verify proper error handling

### Week 8: Day 3-4 - Manual Testing
- [ ] Test registration flow (email/password)
- [ ] Test login flow (email/password)
- [ ] Test Google OAuth login
- [ ] Test VA profile creation
- [ ] Test VA profile update
- [ ] Test VA dashboard loading
- [ ] Test VA onboarding flow
- [ ] Test company profile creation
- [ ] Test employer dashboard loading
- [ ] Test employer onboarding flow
- [ ] Test VA browse/discover
- [ ] Test job posting
- [ ] Test contract creation
- [ ] Test payment checkout
- [ ] Test file upload to R2

### Week 8: Day 5-6 - Docker Configuration
- [ ] Create `docker-compose.yml`
- [ ] Create `Dockerfile` for Next.js
- [ ] Configure environment variables for Docker
- [ ] Set up PostgreSQL volume
- [ ] Configure health checks
- [ ] Test Docker build
- [ ] Test Docker startup

### Week 8: Day 6-7 - Deployment
- [ ] Configure domain for deployment
- [ ] Set up production environment variables
- [ ] Deploy to staging environment
- [ ] Test staging deployment
- [ ] Verify all features work on staging
- [ ] Check logs for errors
- [ ] Monitor performance

### Week 8: Day 7 - Final Checklist
- [ ] All TypeScript errors resolved
- [ ] All linting errors resolved
- [ ] All user flows tested
- [ ] All features working on staging
- [ ] Performance metrics met
- [ ] Security review complete
- [ ] Documentation updated
- [ ] Ready for production

### Week 8: Completion Criteria
- [ ] Zero TypeScript errors
- [ ] Zero linting errors
- [ ] All flows tested manually
- [ ] Docker configuration working
- [ ] Staging deployment successful
- [ ] Production-ready status achieved

---

## Overall Migration Checklist

### Technical Requirements
- [ ] T3 stack project initialized
- [ ] All dependencies installed
- [ ] Database schema migrated
- [ ] NextAuth.js configured
- [ ] tRPC backend implemented
- [ ] Frontend migrated to tRPC
- [ ] Stripe payments integrated
- [ ] File uploads working (R2)
- [ ] Docker configuration ready
- [ ] Deployment successful

### Design Requirements
- [ ] All colors preserved
- [ ] All fonts preserved
- [ ] All spacing preserved
- [ ] All Radix UI components preserved
- [ ] Responsive design maintained
- [ ] Accessibility standards met

### Functional Requirements
- [ ] User registration works
- [ ] User login works
- [ ] Google OAuth works
- [ ] VA profile creation works
- [ ] Company profile creation works
- [ ] VA browsing works
- [ ] Job posting works
- [ ] Contract creation works
- [ ] Payment processing works
- [ ] File uploads work
- [ ] Protected routes work

### Quality Requirements
- [ ] Type safety verified (DB→UI)
- [ ] Zero TypeScript errors
- [ ] Zero linting errors
- [ ] All flows tested
- [ ] Error handling in place
- [ ] Security best practices followed

### Documentation Requirements
- [ ] README.md complete
- [ ] SETUP.md complete
- [ ] MIGRATION_CHECKLIST.md complete
- [ ] DESIGN_PRESERVATION.md complete
- [ ] Code comments added
- [ ] API documentation updated

---

**Last Updated**: January 1, 2026  
**Version**: 1.0  
**Status**: 📋 Ready for Phase 1
