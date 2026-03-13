# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Security Best Practices — security-best-practices (11K) + Trail of Bits Security (12K)

Input validation, auth, CORS, CSRF, secure headers — security at every layer.

**OWASP Top 10 Awareness:**
- A1 Injection: parameterized queries (Drizzle ✅), sanitize free-text inputs
- A3 XSS: sanitize HTML in ALL user-facing string fields
- A5 Security Misconfiguration: audit headers (CSP, HSTS, X-Frame)
- A7 Cross-Site Request Forgery: verify CSRF tokens on state-changing ops

### 2. Test-Driven Validation — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: Write validation tests FIRST.**
- Valid input → schema parses correctly
- Invalid input → schema rejects with correct error message
- Edge cases → boundary values, empty strings, XSS payloads, SQL injection patterns
- Type confusion → branded IDs prevent cross-type errors

### 3. TypeScript Advanced Types — typescript-advanced-types (wshobson, 13K)
Discriminated unions, branded types, template literals — type-safe validation.

### 4. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

When validation unexpectedly passes or fails: root cause investigation → check schema definition, input data, transform chain, refinement predicates.

---

## Patterns

### Advanced Zod Patterns

#### Discriminated Union (type-safe variants)
```typescript
const formSchema = z.discriminatedUnion('type', [
  z.object({
    type: z.literal('individual'),
    firstName: z.string().min(2),
    lastName: z.string().min(2),
    ssn: z.string().regex(/^\d{3}-\d{2}-\d{4}$/),
  }),
  z.object({
    type: z.literal('company'),
    companyName: z.string().min(2),
    taxId: z.string().min(9),
    contactEmail: z.string().email(),
  }),
])
```

#### Transform + Sanitize
```typescript
const sanitizedStringSchema = z.string()
  .trim()
  .transform(s => s.replace(/<[^>]*>/g, '')) // Strip HTML
  .transform(s => s.replace(/[<>"'&]/g, '')) // Remove dangerous chars
  .pipe(z.string().min(1, 'Input required after sanitization'))

const createPostSchema = z.object({
  title: sanitizedStringSchema.pipe(z.string().max(200)),
  body: sanitizedStringSchema.pipe(z.string().max(10000)),
})
```

#### Cross-Field Refinement
```typescript
const dateRangeSchema = z.object({
  startDate: z.string().datetime(),
  endDate: z.string().datetime(),
}).refine(
  data => new Date(data.endDate) > new Date(data.startDate),
  { message: 'End date must be after start date', path: ['endDate'] }
)
```

#### Recursive Schema
```typescript
const commentSchema: z.ZodType<Comment> = z.lazy(() =>
  z.object({
    id: z.string().uuid(),
    text: sanitizedStringSchema,
    author: z.string(),
    replies: z.array(commentSchema).default([]),
  })
)
```

#### Custom Error Map
```typescript
const customErrorMap: z.ZodErrorMap = (issue, ctx) => {
  if (issue.code === z.ZodIssueCode.too_small) {
    return { message: `Must be at least ${issue.minimum} characters` }
  }
  if (issue.code === z.ZodIssueCode.invalid_type) {
    return { message: `Expected ${issue.expected}, got ${issue.received}` }
  }
  return { message: ctx.defaultError }
}

z.setErrorMap(customErrorMap)
```

#### Branded Types (prevent type confusion)
```typescript
const UserId = z.string().uuid().brand<'UserId'>()
const PostId = z.string().uuid().brand<'PostId'>()

type UserId = z.infer<typeof UserId>
type PostId = z.infer<typeof PostId>

// Now UserId and PostId are different types!
// Cannot pass PostId where UserId is expected
```

### Security Sanitization Utility
```typescript
// src/lib/sanitize.ts
export function sanitizeHtml(input: string): string {
  return input
    .replace(/&/g, '&amp;')
    .replace(/</g, '&lt;')
    .replace(/>/g, '&gt;')
    .replace(/"/g, '&quot;')
    .replace(/'/g, '&#x27;')
}

export function sanitizeForDb(input: string): string {
  // Drizzle ORM handles this via parameterized queries
  // This is a safety net for any raw queries
  return input.replace(/['";\\]/g, '')
}

export function sanitizeFilePath(input: string): string {
  // Prevent path traversal
  return input.replace(/\.\./g, '').replace(/^\//, '')
}
```

---

## Templates

### VALIDATION_SPEC.md
```markdown
# Validation Spec: [Feature Name]
> Author: Protected Validation Expert
> Date: [date]

## Input Surfaces

| Surface | Endpoint/Form | Schema | Security |
|---------|--------------|--------|----------|
| POST body | `/api/users` | `createUserSchema` | HTML sanitize, XSS |
| Query params | `/api/users?page=1` | `userQuerySchema` | Type coercion |
| Path params | `/api/users/:id` | `userIdSchema` | UUID format |

## Schemas

### createUserSchema
| Field | Type | Constraints | Transform | Security |
|-------|------|------------|-----------|----------|
| email | string | email format | trim, lowercase | — |
| name | string | 2-100 chars | trim, sanitizeHtml | XSS |

## Security Validations
- [ ] XSS: all string fields sanitized
- [ ] Injection: parameterized queries (Drizzle)
- [ ] Mass assignment: schema uses .pick() for allowed fields

## Error Messages
| Field | Error | Message |
|-------|-------|---------|
| email | missing | "Email is required" |
| email | format | "Please enter a valid email" |
| name | too_short | "Name must be at least 2 characters" |
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack (Zod)
- Shared rules
