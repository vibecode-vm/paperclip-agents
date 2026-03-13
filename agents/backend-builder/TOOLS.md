# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Pre-Build: Documentation References

Before writing code, check `docs/lib/` for the latest library documentation (fetched by the Stack Researcher via Context7). These docs contain **current API signatures** — use them instead of training data.

```bash
# Check if docs exist for backend libraries
ls docs/lib/

# If missing, request from Research Agent via Orchestrator/Gatekeeper:
# "Need Context7 docs for [drizzle-orm migrations, supabase auth ssr, zod transforms, etc.]"
```

⚠️ **If `docs/lib/` has documentation for a library you're using → READ IT FIRST.** It contains the latest API, not your training data.

---

## Skills

### 1. Database Best Practices — supabase-postgres-best-practices (32K)

**Schema Design:**
- Row Level Security (RLS) ALWAYS enabled on every table
- Indexes for all foreign keys and frequently queried columns
- Enums via `pgEnum` — no magic strings
- Timestamps: `created_at` + `updated_at` on every table (with `defaultNow()`)
- Soft delete via `deleted_at` column where appropriate
- UUID for primary keys (`defaultRandom()`)

**Security:**
- Prepared statements (automatic with Drizzle)
- Connection pooling via Supabase Pooler or PgBouncer
- RLS policies for SELECT, INSERT, UPDATE, DELETE separately
- Service role key only on server-side, never exposed to client

### 2. Security — security-best-practices (11K)

**API Security Checklist:**
- Input validation with Zod on EVERY endpoint (request body, params, query)
- Authentication check on every protected route
- Authorization check: does this user have access to this resource?
- Rate limiting on auth endpoints and public APIs
- CORS configuration for allowed origins
- CSRF protection for Server Actions (built into Next.js)
- No sensitive data in URL params (use request body)
- Secure headers (CSP, HSTS, X-Frame-Options) via `next.config.js`

### 3. Test-Driven Development — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**THE IRON LAW: NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**
Write code before the test? Delete it. Start over. No exceptions.

**Backend TDD Cycle (RED-GREEN-REFACTOR):**
1. 🔴 Define API contract (Zod schema) → write failing test for expected behavior
2. 🔴 Run test → MUST FAIL (if it passes, you're testing existing behavior)
3. 🟢 Implement MINIMAL Route Handler / Server Action to pass
4. 🟢 Run ALL tests → ALL MUST PASS
5. 🔵 Refactor if needed — tests stay green
6. ↻ Next failing test for next behavior

**YAGNI**: Don't add config options, extra params, or "improvements" beyond what the test requires.

### 4. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

**4-Phase Process:**
1. **Root Cause Investigation** — Read error carefully (status code, body, stack trace). Reproduce consistently. Check recent changes (git diff). Check database state, auth context, RLS policies, Drizzle query (log SQL).
2. **Pattern Analysis** — Find working examples in codebase. Compare differences.
3. **Hypothesis Testing** — SINGLE hypothesis, SMALLEST change, verify. Don't stack fixes.
4. **Implementation** — Fix at root cause, not symptom. Verify with evidence.

### 5. Verification — verification-before-completion (obra, 16K)

**Before claiming "API is done":**
1. Run ALL tests: `npx vitest run src/app/api/`
2. Check migration status: `npx drizzle-kit check`
3. Verify RLS policies are active: query with anon key
4. Test error paths: invalid input, unauthorized, not found
5. Show evidence: test output, curl examples, response bodies

### Additional Skills

| Skill | Source | Installs | Use |
|-------|--------|----------|-----|
| `context7` | am-will | 12K | Fetch latest Drizzle/Supabase docs |
| `code-review` | supercent-io | 11K | Review API patterns |
| `receiving-code-review` | obra | 16K | Act on review feedback |
| `finishing-a-development-branch` | obra | 14K | Clean branch completion |

---

## Drizzle ORM Patterns

### Schema Definition
```typescript
// src/db/schema/users.ts
import { pgTable, uuid, text, timestamp, pgEnum } from 'drizzle-orm/pg-core'

export const roleEnum = pgEnum('role', ['user', 'admin', 'moderator'])

export const users = pgTable('users', {
  id: uuid('id').primaryKey().defaultRandom(),
  email: text('email').notNull().unique(),
  name: text('name').notNull(),
  role: roleEnum('role').notNull().default('user'),
  avatarUrl: text('avatar_url'),
  createdAt: timestamp('created_at', { withTimezone: true }).notNull().defaultNow(),
  updatedAt: timestamp('updated_at', { withTimezone: true }).notNull().defaultNow(),
})
```

### Relations
```typescript
// src/db/schema/relations.ts
import { relations } from 'drizzle-orm'
import { users } from './users'
import { posts } from './posts'

export const usersRelations = relations(users, ({ many }) => ({
  posts: many(posts),
}))

export const postsRelations = relations(posts, ({ one }) => ({
  author: one(users, {
    fields: [posts.authorId],
    references: [users.id],
  }),
}))
```

### Database Client
```typescript
// src/db/index.ts
import { drizzle } from 'drizzle-orm/postgres-js'
import postgres from 'postgres'
import * as schema from './schema'

const connectionString = process.env.DATABASE_URL!
const client = postgres(connectionString)
export const db = drizzle(client, { schema })
```

### Migrations
```bash
# Generate migration from schema changes
npx drizzle-kit generate

# Apply migrations
npx drizzle-kit migrate

# Open Drizzle Studio (visual DB browser)
npx drizzle-kit studio

# Check migration status
npx drizzle-kit check
```

---

## API Route Handler Patterns

### GET — List with Pagination
```typescript
// src/app/api/users/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { z } from 'zod'
import { db } from '@/db'
import { users } from '@/db/schema'
import { desc, count } from 'drizzle-orm'

const querySchema = z.object({
  page: z.coerce.number().int().positive().default(1),
  limit: z.coerce.number().int().min(1).max(100).default(20),
})

export async function GET(request: NextRequest) {
  const params = querySchema.parse(
    Object.fromEntries(request.nextUrl.searchParams)
  )

  const [data, [{ total }]] = await Promise.all([
    db.select()
      .from(users)
      .orderBy(desc(users.createdAt))
      .limit(params.limit)
      .offset((params.page - 1) * params.limit),
    db.select({ total: count() }).from(users),
  ])

  return NextResponse.json({
    data,
    pagination: {
      page: params.page,
      limit: params.limit,
      total,
      pages: Math.ceil(total / params.limit),
    },
  })
}
```

### POST — Create with Validation
```typescript
// src/app/api/users/route.ts (continued)
const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
  role: z.enum(['user', 'admin', 'moderator']).default('user'),
})

export async function POST(request: NextRequest) {
  const body = await request.json()
  const validated = createUserSchema.parse(body)

  const [user] = await db.insert(users).values(validated).returning()

  return NextResponse.json(user, { status: 201 })
}
```

### Dynamic Route — GET by ID + PATCH + DELETE
```typescript
// src/app/api/users/[id]/route.ts
import { NextRequest, NextResponse } from 'next/server'
import { z } from 'zod'
import { db } from '@/db'
import { users } from '@/db/schema'
import { eq } from 'drizzle-orm'

const paramsSchema = z.object({
  id: z.string().uuid(),
})

const updateUserSchema = z.object({
  name: z.string().min(2).max(100).optional(),
  role: z.enum(['user', 'admin', 'moderator']).optional(),
  avatarUrl: z.string().url().nullable().optional(),
})

export async function GET(
  _request: NextRequest,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = paramsSchema.parse(await params)
  const [user] = await db.select().from(users).where(eq(users.id, id))

  if (!user) {
    return NextResponse.json({ error: 'User not found' }, { status: 404 })
  }

  return NextResponse.json(user)
}

export async function PATCH(
  request: NextRequest,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = paramsSchema.parse(await params)
  const body = await request.json()
  const validated = updateUserSchema.parse(body)

  const [user] = await db.update(users)
    .set({ ...validated, updatedAt: new Date() })
    .where(eq(users.id, id))
    .returning()

  if (!user) {
    return NextResponse.json({ error: 'User not found' }, { status: 404 })
  }

  return NextResponse.json(user)
}

export async function DELETE(
  _request: NextRequest,
  { params }: { params: Promise<{ id: string }> }
) {
  const { id } = paramsSchema.parse(await params)
  const [user] = await db.delete(users).where(eq(users.id, id)).returning()

  if (!user) {
    return NextResponse.json({ error: 'User not found' }, { status: 404 })
  }

  return NextResponse.json({ deleted: true })
}
```

---

## Server Action Patterns

### Mutation with Revalidation
```typescript
// src/app/actions/users.ts
'use server'

import { z } from 'zod'
import { db } from '@/db'
import { users } from '@/db/schema'
import { revalidatePath } from 'next/cache'

const createUserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(2).max(100),
})

export async function createUser(formData: FormData) {
  const validated = createUserSchema.parse({
    email: formData.get('email'),
    name: formData.get('name'),
  })

  await db.insert(users).values(validated)
  revalidatePath('/users')
}
```

---

## Error Handling Pattern

### Standardized API Error Response
```typescript
// src/lib/api-error.ts
import { NextResponse } from 'next/server'
import { ZodError } from 'zod'

export function handleApiError(error: unknown) {
  if (error instanceof ZodError) {
    return NextResponse.json(
      { error: 'Validation failed', details: error.flatten().fieldErrors },
      { status: 400 }
    )
  }

  if (error instanceof Error) {
    console.error('[API Error]', { message: error.message, stack: error.stack })

    if (error.message.includes('unique constraint')) {
      return NextResponse.json(
        { error: 'Resource already exists' },
        { status: 409 }
      )
    }

    if (error.message.includes('not found')) {
      return NextResponse.json(
        { error: 'Resource not found' },
        { status: 404 }
      )
    }
  }

  return NextResponse.json(
    { error: 'Internal server error' },
    { status: 500 }
  )
}

// Usage in Route Handler:
// export async function POST(request: NextRequest) {
//   try {
//     // ... implementation
//   } catch (error) {
//     return handleApiError(error)
//   }
// }
```

---

## Supabase Auth (SSR) Pattern

### Create Supabase Client (Server)
```typescript
// src/lib/supabase/server.ts
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() { return cookieStore.getAll() },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value, options }) =>
            cookieStore.set(name, value, options)
          )
        },
      },
    }
  )
}
```

### Auth Middleware
```typescript
// src/middleware.ts
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'

export async function middleware(request: NextRequest) {
  let supabaseResponse = NextResponse.next({ request })

  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() { return request.cookies.getAll() },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value }) => request.cookies.set(name, value))
          supabaseResponse = NextResponse.next({ request })
          cookiesToSet.forEach(({ name, value, options }) =>
            supabaseResponse.cookies.set(name, value, options)
          )
        },
      },
    }
  )

  const { data: { user } } = await supabase.auth.getUser()

  if (!user && request.nextUrl.pathname.startsWith('/dashboard')) {
    const url = request.nextUrl.clone()
    url.pathname = '/login'
    return NextResponse.redirect(url)
  }

  return supabaseResponse
}

export const config = {
  matcher: ['/dashboard/:path*', '/api/protected/:path*'],
}
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack with versions
- MCP server configs (Context7, Firecrawl)
- Shared rules (Goal Check, Evidence, Circuit Breaker, HANDOFF, Skill Priority)
- Project folder structure
- Coverage targets

---

## Backend File Structure

```
src/
├── app/
│   ├── api/                    ← Route Handlers
│   │   ├── users/
│   │   │   ├── route.ts        ← GET (list) + POST (create)
│   │   │   └── [id]/
│   │   │       └── route.ts    ← GET + PATCH + DELETE
│   │   └── auth/
│   │       ├── login/route.ts
│   │       └── register/route.ts
│   └── actions/                ← Server Actions
│       └── users.ts
├── db/
│   ├── index.ts                ← Database client
│   ├── schema/                 ← Drizzle schema files
│   │   ├── index.ts            ← Schema barrel export
│   │   ├── users.ts
│   │   └── posts.ts
│   ├── relations.ts            ← Drizzle relations
│   └── seed.ts                 ← Seed data script
├── lib/
│   ├── api-error.ts            ← Error handling utility
│   ├── auth.ts                 ← Auth helper functions
│   ├── supabase/
│   │   ├── server.ts           ← Server Supabase client
│   │   └── client.ts           ← Browser Supabase client
│   └── validations/            ← Shared Zod schemas
│       ├── users.ts
│       └── posts.ts
├── middleware.ts                ← Auth middleware
└── types/                      ← Shared TypeScript types
    └── api.ts                  ← API request/response types
```

---

## Templates

### API_SPEC.md
```markdown
# API_SPEC: [Feature Name]
> Author: API & Database Backend Builder
> Date: [date]
> Based on: GOAL.md + PRD

## Endpoints

### POST /api/[resource]
**Purpose**: Create a new [resource]

**Auth**: Required (Bearer token)

**Request Body:**
\`\`\`json
{
  "field1": "string (required, min 2 chars)",
  "field2": "number (optional, default: 0)"
}
\`\`\`

**Response 201:**
\`\`\`json
{
  "id": "uuid",
  "field1": "string",
  "field2": 0,
  "createdAt": "2026-03-12T00:00:00Z"
}
\`\`\`

**Response 400:**
\`\`\`json
{
  "error": "Validation failed",
  "details": { "field1": ["Required"] }
}
\`\`\`

**Response 401:**
\`\`\`json
{ "error": "Unauthorized" }
\`\`\`

## Database Schema Changes

| Table | Column | Type | Constraints |
|-------|--------|------|-------------|
| [table] | id | uuid | PK, default random |
| [table] | [column] | text | NOT NULL |

## RLS Policies

| Policy | Table | Operation | Rule |
|--------|-------|-----------|------|
| Users can read own | users | SELECT | `auth.uid() = id` |
| Users can update own | users | UPDATE | `auth.uid() = id` |

## Migration Plan
1. Generate migration: `npx drizzle-kit generate`
2. Review generated SQL
3. Apply: `npx drizzle-kit migrate`
4. Verify: Query the table
```

### API Integration Test Pattern
```typescript
// src/app/api/users/route.test.ts
import { describe, it, expect, beforeEach } from 'vitest'
import { db } from '@/db'
import { users } from '@/db/schema'

describe('POST /api/users', () => {
  beforeEach(async () => {
    await db.delete(users) // Clean state
  })

  it('creates user with valid data', async () => {
    const response = await fetch('http://localhost:3000/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: 'test@example.com', name: 'Test User' }),
    })

    expect(response.status).toBe(201)
    const user = await response.json()
    expect(user.email).toBe('test@example.com')
    expect(user.id).toBeDefined()
  })

  it('rejects invalid email', async () => {
    const response = await fetch('http://localhost:3000/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: 'not-an-email', name: 'Test' }),
    })

    expect(response.status).toBe(400)
    const error = await response.json()
    expect(error.details.email).toBeDefined()
  })

  it('rejects duplicate email', async () => {
    // Insert first user
    await db.insert(users).values({ email: 'test@example.com', name: 'First' })

    const response = await fetch('http://localhost:3000/api/users', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ email: 'test@example.com', name: 'Second' }),
    })

    expect(response.status).toBe(409)
  })
})
```
