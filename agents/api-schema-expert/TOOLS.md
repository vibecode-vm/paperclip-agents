# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Context7 — context7 (am-will, 12K)
Fetch latest Zod, Next.js Route Handler docs for current API patterns.

### 2. Contract-First TDD — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: Define the schema BEFORE writing the endpoint.**

**Contract-First Workflow:**
1. 🔴 Define Zod request/response schemas → write test against schema
2. 🔴 Run test → MUST FAIL (endpoint doesn't exist yet)
3. 🟢 Implement Route Handler with MINIMAL code to pass
4. 🟢 Run → PASS
5. 🔵 Refactor (extract validation middleware, error handling)
6. ↻ Repeat for next endpoint

**Schema MUST be single source of truth:**
- Frontend imports schema for form validation
- Backend imports same schema for request validation
- Types inferred from schema — NEVER duplicate

### 3. Verification — verification-before-completion (obra/superpowers, 16K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

Verify: all schemas compile (`npx tsc --noEmit`), types infer correctly, no `any` leaks.

### 4. Code Review — requesting-code-review (obra/superpowers, 11K)
Review API contracts for consistency, completeness, and backward compatibility.

---

## Patterns

### Shared Zod Schema (Frontend + Backend)
```typescript
// src/lib/validations/users.ts
import { z } from 'zod'

// Request schemas
export const createUserSchema = z.object({
  email: z.string().email('Invalid email format'),
  name: z.string().min(2, 'Name must be at least 2 characters').max(100),
  role: z.enum(['user', 'admin', 'moderator']).default('user'),
})

export const updateUserSchema = createUserSchema.partial().omit({ email: true })

export const userQuerySchema = z.object({
  page: z.coerce.number().int().positive().default(1),
  limit: z.coerce.number().int().min(1).max(100).default(20),
  sort: z.enum(['createdAt', '-createdAt', 'name', '-name']).default('-createdAt'),
  filter: z.object({
    role: z.enum(['user', 'admin', 'moderator']).optional(),
    search: z.string().optional(),
  }).optional(),
})

export const userIdSchema = z.object({
  id: z.string().uuid('Invalid user ID'),
})

// Response schemas
export const userResponseSchema = z.object({
  id: z.string().uuid(),
  email: z.string().email(),
  name: z.string(),
  role: z.enum(['user', 'admin', 'moderator']),
  createdAt: z.string().datetime(),
  updatedAt: z.string().datetime(),
})

export const paginatedResponseSchema = <T extends z.ZodType>(itemSchema: T) =>
  z.object({
    data: z.array(itemSchema),
    pagination: z.object({
      page: z.number(),
      limit: z.number(),
      total: z.number(),
      pages: z.number(),
    }),
  })

// Inferred types
export type CreateUserInput = z.infer<typeof createUserSchema>
export type UpdateUserInput = z.infer<typeof updateUserSchema>
export type UserResponse = z.infer<typeof userResponseSchema>
export type UserQuery = z.infer<typeof userQuerySchema>
```

### API Versioning Pattern
```typescript
// src/app/api/v1/users/route.ts  — Version 1
// src/app/api/v2/users/route.ts  — Version 2 (breaking changes)

// Shared schemas stay compatible:
// v1: createUserSchema (email, name)
// v2: createUserSchemaV2 extends createUserSchema with required 'role'
```

---

## Templates

### API_SPEC.md
```markdown
# API_SPEC: [Feature Name]
> Author: REST API Schema Expert
> Date: [date]
> Version: v1

## Endpoints

### GET /api/[resource]
**Purpose**: List [resources] with pagination
**Auth**: Required
**Query**: `?page=1&limit=20&sort=-createdAt`
**Response 200**: `{ data: Resource[], pagination: {...} }`

### POST /api/[resource]
**Purpose**: Create [resource]
**Auth**: Required
**Body**: `CreateResourceInput` schema
**Response 201**: `{ data: Resource }`
**Response 400**: `{ error: "Validation failed", details: {...} }`
**Response 409**: `{ error: "Resource already exists" }`

## Zod Schemas
| Schema | File | Purpose |
|--------|------|---------|
| `createResourceSchema` | `src/lib/validations/[resource].ts` | POST body |
| `updateResourceSchema` | same | PATCH body |
| `resourceQuerySchema` | same | GET query params |
| `resourceResponseSchema` | same | Response typing |

## Breaking Changes from v[N-1]
- [list of changes]
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack (Zod, Next.js)
- Shared rules
