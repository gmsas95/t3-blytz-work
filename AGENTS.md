# AGENTS.md

This file guides AI coding agents working on this T3 Stack project.

## Build, Lint, and Typecheck Commands

### Development
- `npm run dev` - Start Next.js dev server with Turbo (http://localhost:3000)
- `npm run build` - Build for production
- `npm run start` - Start production server
- `npm run preview` - Build and start production server

### Code Quality
- `npm run lint` - Run ESLint
- `npm run lint:fix` - Auto-fix ESLint issues
- `npm run typecheck` - Run TypeScript type check (tsc --noEmit)
- `npm run check` - Run both lint and typecheck

### Formatting
- `npm run format:check` - Check code formatting with Prettier
- `npm run format:write` - Auto-format code with Prettier

### Database
- `npm run db:push` - Push Prisma schema changes to database (dev)
- `npm run db:generate` - Generate Prisma migration
- `npm run db:migrate` - Deploy migrations (production)
- `npm run db:studio` - Open Prisma Studio UI

### Testing
No test framework is currently configured. Add test commands here when implementing tests.

## Code Style Guidelines

### TypeScript
- Strict mode enabled with `noUncheckedIndexedAccess`
- Path alias: `~/*` maps to `./src/*` (e.g., `~/server/auth`)
- Use `import type` for type-only imports (ESLint enforces inline type imports)
- Prefix unused variables/parameters with underscore `_` (e.g., `function foo(_unused: string)`)
- `server-only` import for server-exclusive code

### Imports
```typescript
// Type imports (inline)
import type { User } from "~/server/auth";
import type { AppRouter } from "~/server/api/root";

// Value imports
import { db } from "~/server/db";
import { api } from "~/trpc/react";
```

### tRPC Procedures
- Use `publicProcedure` for unauthenticated endpoints
- Use `protectedProcedure` for authenticated endpoints (requires valid session)
- Use Zod schemas for input validation
- Access `ctx.session.user.id` in protected procedures
- Throw `TRPCError` with appropriate codes for errors:

```typescript
import { z } from "zod";
import { TRPCError } from "@trpc/server";
import { protectedProcedure, createTRPCRouter } from "~/server/api/trpc";

export const exampleRouter = createTRPCRouter({
  get: protectedProcedure
    .input(z.object({ id: z.string() }))
    .query(async ({ ctx, input }) => {
      const item = await ctx.db.item.findUnique({
        where: { id: input.id },
      });
      if (!item) {
        throw new TRPCError({ code: "NOT_FOUND", message: "Item not found" });
      }
      return item;
    }),
});
```

### React Components
- Use "use client" directive for client components (hooks, event handlers)
- Use server components by default (no "use client")
- Use tRPC hooks in client components: `api.post.getLatest.useSuspenseQuery()`
- Use `api.useUtils()` for cache invalidation
- Prefetch data in server components: `void api.post.getLatest.prefetch()`

```typescript
"use client";

import { useState } from "react";
import { api } from "~/trpc/react";

export function ExampleComponent() {
  const [data] = api.example.get.useSuspenseQuery({ id: "123" });
  const mutation = api.example.update.useMutation({
    onSuccess: async () => {
      await utils.example.invalidate();
    },
  });

  return <div>{data.name}</div>;
}
```

### Database (Prisma)
- Access database via `ctx.db` in tRPC procedures
- Use Prisma Client directly in server components: `import { db } from "~/server/db"`
- Follow existing schema patterns in `prisma/schema.prisma`
- Run migrations after schema changes: `npm run db:push` (dev) or `npm run db:generate`

### Authentication (NextAuth)
- Current session accessed via `await auth()` in server components
- In tRPC procedures, access via `ctx.session.user`
- Use `protectedProcedure` middleware for auth-required endpoints
- Configure providers in `src/server/auth/config.ts`

### Styling (Tailwind CSS v4)
- Use utility classes from Tailwind CSS
- Prettier plugin sorts classes automatically
- Font variable: `--font-geist-sans` for Geist font
- No custom CSS in components - use Tailwind classes

### Error Handling
- In tRPC procedures: `throw new TRPCError({ code: "NOT_FOUND" })`
- In server components: return null or handle gracefully
- Log errors with `console.error()` for debugging
- User-facing errors should be descriptive but not expose internals

### File Naming
- Components: PascalCase (e.g., `UserProfile.tsx`)
- Utilities: camelCase (e.g., `formatDate.ts`)
- Types: kebab-case for files, PascalCase for exports
- Routes: lowercase with hyphens in folders (e.g., `user-profile/page.tsx`)

### Zod Schemas
- Define inline in tRPC procedures or export separately
- Use `.optional()` for nullable fields
- Use `.min()/.max()` for validation constraints
- Use `.refine()` for custom validation

### Code Organization
- **Server code**: `src/server/api/routers/` (tRPC procedures)
- **Client code**: `src/app/_components/` (React components)
- **Utilities**: `src/lib/` (helper functions)
- **Types**: Co-located with usage or in feature folders
- **Styles**: `src/styles/globals.css` (Tailwind CSS)

### Running Single File Checks
```bash
# Lint single file
npx next lint --file src/app/page.tsx

# Typecheck single file
npx tsc --noEmit src/app/page.tsx

# Format single file
npx prettier --write src/app/page.tsx
```

### Before Committing
1. Run `npm run check` (lint + typecheck)
2. Run `npm run format:write` (format code)
3. Ensure zero TypeScript errors
4. Test the changed functionality manually

## Tech Stack
- **Framework**: Next.js 15 with App Router
- **Type Safety**: TypeScript 5.8 (strict mode)
- **API**: tRPC (end-to-end type safety)
- **Database**: PostgreSQL + Prisma 6
- **Auth**: NextAuth.js v5 (beta)
- **Styling**: Tailwind CSS v4
- **UI**: React 19
- **State**: TanStack Query (via tRPC)
- **Validation**: Zod

## Project Notes
- This is a T3 Stack project created with create-t3-app
- Migration from Firebase Auth + Fastify backend to NextAuth + tRPC
- Fresh build (no data migration from legacy codebase)
- Design tokens preserved from blytz.work reference repo
- Focus on type safety and developer experience
