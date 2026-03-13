# HEARTBEAT.md — API Schema Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack (Zod, Next.js).
- Read `../../MEMORY.md` — known API patterns.
- Check existing schemas: `ls src/lib/validations/`
- Check existing API routes: `find src/app/api -name "route.ts"`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — existing endpoints, schemas.
2. Read `docs/api/` — existing API documentation.
3. Check existing Zod schemas: `ls src/lib/validations/`

## 5. API Design Standards

### Naming Conventions
```
Endpoints:   /api/{resource}           (plural, kebab-case)
             /api/{resource}/{id}      (single item)
             /api/{resource}/{id}/{sub} (nested resource)

Methods:     GET     = Read (list or detail)
             POST    = Create
             PATCH   = Partial update
             DELETE  = Remove
             PUT     = Full replace (rare)

Query:       ?page=1&limit=20          (pagination)
             ?sort=-createdAt          (sort, - = desc)
             ?filter[status]=active    (filter)
             ?include=author,comments  (relations)
```

### Response Envelope
```typescript
// Success (single item)
{ data: T }

// Success (list)
{ data: T[], pagination: { page, limit, total, pages } }

// Error
{ error: string, details?: Record<string, string[]> }
```

### Error Code Standards
| Status | When | Example |
|--------|------|---------|
| 400 | Validation failed | Missing required field |
| 401 | Not authenticated | No/invalid token |
| 403 | Not authorized | Wrong role/permissions |
| 404 | Resource not found | Invalid ID |
| 409 | Conflict | Duplicate email |
| 422 | Unprocessable | Business rule violation |
| 429 | Rate limited | Too many requests |
| 500 | Server error | Unexpected failure |

## 6. Zod Schema Design
- [ ] Request body schema (input validation)
- [ ] Query params schema (with coercion for numbers/booleans)
- [ ] Path params schema (UUID validation)
- [ ] Response schema (output typing)
- [ ] Error response schema (consistent format)
- [ ] Export inferred TypeScript types
- [ ] Schemas in `src/lib/validations/{resource}.ts`

## 7. Pre-Delivery Checklist
- [ ] All endpoints have complete Zod schemas (request + response)
- [ ] API_SPEC.md covers: endpoints, auth, schemas, errors, examples
- [ ] Naming follows conventions (plural, kebab-case)
- [ ] Error codes documented per endpoint
- [ ] Pagination standard applied consistently
- [ ] TypeScript types exported from schemas
- [ ] SYSTEM_STATE.md updated with new schemas

## 8. Exit
- Comment on in_progress work.
- HANDOFF to Backend Builder with API_SPEC.md.

## Rules
- Contract FIRST, implementation SECOND.
- ONE Zod schema = validation + TypeScript types + documentation.
- CONSISTENT naming, error codes, and response formats.
- NEVER break existing contracts — version instead.
- All schemas are SHARED between frontend and backend.
