# T3 Stack Development Setup Guide

## Prerequisites

Before starting development, ensure you have:

- **Node.js** 18+ (recommended 20)
- **PostgreSQL** 15+ (or Docker for database)
- **Git** for version control
- **Docker** (optional, for containerized development)
- **VS Code** (or preferred code editor)
- **pNPM** or **npm** package manager

---

## Quick Start

### 1. Clone/Navigate to Project

```bash
cd /home/gmsas95/t3.blytz.work
```

### 2. Install Dependencies

```bash
npm install
```

This installs all T3 stack dependencies:
- Next.js
- React
- TypeScript
- Tailwind CSS
- tRPC
- Prisma
- NextAuth.js
- Radix UI components
- Additional dependencies (bcryptjs, etc.)

### 3. Configure Environment Variables

Create `.env.local` file:

```bash
# Database
DATABASE_URL="postgresql://blytz:blytz@localhost:5432/blytz"

# NextAuth.js
NEXTAUTH_URL="http://localhost:3000"
NEXTAUTH_SECRET="generate-with-openssl-rand-base64-32"

# Google OAuth (get from Google Cloud Console)
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Stripe Test Keys
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY="pk_test_..."

# Cloudflare R2 (file uploads)
AWS_ACCESS_KEY_ID="your-r2-access-key"
AWS_SECRET_ACCESS_KEY="your-r2-secret-key"
AWS_S3_BUCKET="blytz-uploads"
AWS_REGION="auto"
```

### 4. Set Up Database

**Option A: Local PostgreSQL**
```bash
# Start PostgreSQL (if not running)
sudo systemctl start postgresql

# Create database
createdb blytz

# Or use Docker
docker run -d --name blytz-postgres \
  -e POSTGRES_USER=blytz \
  -e POSTGRES_PASSWORD=blytz \
  -e POSTGRES_DB=blytz \
  -p 5432:5432 \
  postgres:16
```

**Option B: Docker Compose (Recommended)**
```bash
# Start PostgreSQL container
docker run -d --name blytz-postgres \
  -e POSTGRES_USER=blytz \
  -e POSTGRES_PASSWORD=blytz \
  -e POSTGRES_DB=blytz \
  -v blytz_pgdata:/var/lib/postgresql/data \
  -p 5432:5432 \
  postgres:16-alpine
```

### 5. Initialize Prisma

```bash
# Generate Prisma client
npm run db:push

# Or run migrations (if using migrate dev)
npm run db:dev

# Verify database connection
npm run db:studio  # Opens Prisma Studio in browser
```

### 6. Start Development Server

```bash
npm run dev
```

The development server will start at `http://localhost:3000`

---

## Project Structure Overview

```
t3.blytz.work/
├── src/
│   ├── app/              # Next.js App Router (pages)
│   ├── server/           # tRPC backend (procedures)
│   ├── components/       # React components
│   ├── lib/             # Utilities (trpc, prisma, auth)
│   └── styles/          # Global styles
├── prisma/
│   ├── schema.prisma      # Database schema
│   └── migrations/       # SQL migrations
├── public/              # Static assets
├── .env.local           # Environment variables (gitignored)
└── package.json
```

---

## Development Workflow

### 1. Make Code Changes

Edit files in `src/` directory. The development server will hot-reload automatically.

### 2. Database Schema Changes

If you modify `prisma/schema.prisma`:

```bash
# Push changes to database (development)
npm run db:push

# Or create migration (production-ready)
npx prisma migrate dev --name description

# Regenerate Prisma client
npx prisma generate
```

### 3. Type Checking

T3 Stack provides full type safety. To check types:

```bash
# Run TypeScript compiler check
npx tsc --noEmit

# Or check only for type errors
npm run type-check
```

### 4. Running Tests

```bash
# Run all tests (when implemented)
npm test

# Run tests in watch mode
npm test:watch

# Run with coverage
npm test:coverage
```

---

## Common Development Tasks

### Add a New tRPC Procedure

1. Create or update router file in `src/server/`:

```typescript
// src/server/my-feature.ts
import { z } from "zod";
import { router, protectedProcedure } from "./trpc";
import { prisma } from "@/lib/prisma";

export const myFeatureRouter = router({
  getItem: publicProcedure
    .input(z.string())
    .query(async ({ input }) => {
      return prisma.item.findUnique({ where: { id: input } });
    }),

  createItem: protectedProcedure
    .input(z.object({
      name: z.string(),
      value: z.number(),
    }))
    .mutation(async ({ ctx, input }) => {
      return prisma.item.create({
        data: {
          ...input,
          userId: ctx.user.id,
        },
      });
    }),
});
```

2. Add to main router in `src/server/index.ts`:

```typescript
import { appRouter } from "./trpc";
import { myFeatureRouter } from "./my-feature";

export const appRouter = appRouter({
  // ...existing routers
  myFeature: myFeatureRouter,
});
```

3. Use in frontend:

```typescript
import { trpc } from "@/lib/trpc";

export default function MyPage() {
  const { data: item, isLoading } = trpc.myFeature.getItem.useQuery("id");
  const createItem = trpc.myFeature.createItem.useMutation();

  const handleCreate = async () => {
    await createItem.mutateAsync({ name: "test", value: 42 });
  };

  // ...
}
```

### Add a New Page

1. Create page in `src/app/`:

```typescript
// src/app/my-page/page.tsx
"use client";

import { trpc } from "@/lib/trpc";

export default function MyPage() {
  const { data, isLoading } = trpc.myFeature.getItems.useQuery();

  if (isLoading) return <div>Loading...</div>;

  return (
    <div>
      <h1>My Page</h1>
      {data?.map(item => (
        <div key={item.id}>{item.name}</div>
      ))}
    </div>
  );
}
```

### Add Authentication to a Page

```typescript
// src/app/(protected)/my-page/page.tsx
import { getServerSession } from "next-auth";
import { authOptions } from "@/app/api/auth/[...nextauth]/route";
import { redirect } from "next/navigation";

export default async function MyPage() {
  const session = await getServerSession(authOptions);

  if (!session) {
    redirect("/login");
  }

  return <div>Protected content for {session.user.email}</div>;
}
```

---

## Database Operations

### Using Prisma Client

```typescript
import { prisma } from "@/lib/prisma";

// Create
const user = await prisma.user.create({
  data: {
    email: "test@example.com",
    name: "Test User",
    role: "va",
  },
});

// Read
const users = await prisma.user.findMany();
const user = await prisma.user.findUnique({
  where: { id: "user-id" },
  include: { vaProfile: true },
});

// Update
await prisma.user.update({
  where: { id: "user-id" },
  data: { name: "Updated Name" },
});

// Delete
await prisma.user.delete({
  where: { id: "user-id" },
});
```

### Using Prisma Studio

```bash
npm run db:studio
```

Prisma Studio will open at `http://localhost:5555`

---

## Debugging

### TypeScript Errors

T3 Stack catches type errors at compile time. Common issues:

1. **Missing imports**: Add missing imports
2. **Type mismatches**: Update types to match
3. **Prisma types**: Run `npx prisma generate`

### Runtime Errors

Check browser console and terminal logs. Common issues:

1. **Database connection**: Verify DATABASE_URL
2. **Missing env vars**: Check .env.local exists
3. **tRPC errors**: Check procedure implementation
4. **NextAuth errors**: Check NEXTAUTH_SECRET and NEXTAUTH_URL

### Debug Mode

For detailed logging:

```typescript
// In tRPC procedures
console.log("Input:", input);
console.log("Result:", result);

// In API routes
console.log("Request body:", await req.json());
```

---

## Testing API Procedures

### Using tRPC Playground

When running `npm run dev`, tRPC Playground is available at:
```
http://localhost:3000/api/trpc
```

Use it to test procedures without building frontend.

### Using curl

```bash
# Test protected endpoint
curl -X POST http://localhost:3000/api/auth/callback/credentials \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password"}'
```

---

## Environment-Specific Setup

### Development

- Database: PostgreSQL 16 (Docker)
- Environment: Local
- Hot reload: Enabled
- Debug logging: Enabled

### Staging

- Database: PostgreSQL 16 (production)
- Environment: Dokploy VPS
- Environment variables: Dokploy secrets
- Logging: Production level

### Production

- Database: Managed PostgreSQL (AWS RDS or similar)
- Environment: Production VPS
- Environment variables: Secrets manager
- Logging: Centralized (e.g., Sentry)
- CDN: For static assets

---

## Useful Commands

### Package Scripts

```bash
npm run dev           # Start development server (localhost:3000)
npm run build         # Build for production
npm run start         # Start production server
npm run lint          # Run ESLint
npm run type-check    # Run TypeScript type check
```

### Database Scripts

```bash
npm run db:push       # Push schema to database
npm run db:studio     # Open Prisma Studio
npm run db:dev        # Create and run migration
npm run db:migrate    # Apply migrations
```

### Docker Commands

```bash
docker-compose up -d     # Start services in background
docker-compose down      # Stop and remove services
docker-compose logs -f  # View logs
docker-compose restart  # Restart services
```

---

## Troubleshooting

### Database Connection Issues

**Problem**: "Can't reach database server"

**Solutions**:
1. Check PostgreSQL is running:
   ```bash
   docker ps | grep postgres
   ```
2. Verify DATABASE_URL in .env.local
3. Check firewall settings
4. Test connection with psql:
   ```bash
   psql postgresql://blytz:blytz@localhost:5432/blytz
   ```

### Module Not Found Errors

**Problem**: "Module not found: @/lib/prisma"

**Solutions**:
1. Run `npm run db:push` (regenerates Prisma client)
2. Check import paths in tsconfig.json
3. Restart development server

### NextAuth Issues

**Problem**: Auth not working

**Solutions**:
1. Verify NEXTAUTH_URL (no trailing slash)
2. Check NEXTAUTH_SECRET is set
3. Check Google OAuth credentials
4. Verify callback URLs in Google Console

### Tailwind CSS Not Working

**Problem**: Styles not applied

**Solutions**:
1. Verify tailwind.config.ts is correct
2. Check globals.css imports in layout.tsx
3. Restart development server
4. Check Tailwind CSS class names are correct

---

## Best Practices

### 1. Type Safety

Always use types from Prisma and tRPC:

```typescript
// Good
import type { User } from "@prisma/client";

// Bad
const user: any = {};
```

### 2. Protected Procedures

Use `protectedProcedure` for auth-required operations:

```typescript
export const myRouter = router({
  myAction: protectedProcedure.mutation(async ({ ctx }) => {
    // ctx.user is guaranteed to exist
  }),
});
```

### 3. Error Handling

Use tRPC's built-in error handling:

```typescript
import { TRPCError } from "@trpc/server";

throw new TRPCError({
  code: "NOT_FOUND",
  message: "Item not found",
});
```

### 4. Database Queries

Use `include` and `select` for optimized queries:

```typescript
// Good (only fetch needed fields)
const user = await prisma.user.findUnique({
  where: { id },
  select: { name: true, email: true },
});

// Bad (fetches everything)
const user = await prisma.user.findUnique({
  where: { id },
});
```

### 5. Environment Variables

Never commit `.env.local`. Use `.env.example`:

```bash
# .env.example (commit to git)
DATABASE_URL="postgresql://..."
NEXTAUTH_SECRET="..."
```

```bash
# .env.local (don't commit)
DATABASE_URL="postgresql://..."
NEXTAUTH_SECRET="your-actual-secret"
```

---

## Getting Help

### Documentation

- `README.md` - Project overview
- `MIGRATION_CHECKLIST.md` - Implementation checklist
- `DESIGN_PRESERVATION.md` - Design guidelines

### Internal Documentation

- T3 Stack: https://create.t3.gg/
- tRPC: https://trpc.io/docs
- Next.js: https://nextjs.org/docs
- Prisma: https://www.prisma.io/docs
- NextAuth.js: https://next-auth.js.org

### Troubleshooting Issues

1. Check logs in terminal
2. Check browser console
3. Verify environment variables
4. Check database connection
5. Restart development server

---

**Last Updated**: January 1, 2026  
**Version**: 1.0
