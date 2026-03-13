# HEARTBEAT.md — Validation Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack (Zod).
- Read `../../MEMORY.md` — known validation patterns.
- Check existing schemas: `ls src/lib/validations/`
- Check Zod version: `npm list zod`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — existing schemas and endpoints.
2. Read existing Zod schemas: `find src -name "*.ts" | xargs grep "z\." | head -30`
3. Check for raw input usage: `grep -r "req\.body\|formData\.get" src/ --include="*.ts"`

## 5. Input Surface Analysis

```
For each feature, identify ALL input surfaces:
├── API Request Bodies (POST/PATCH/PUT)
├── Query Parameters (?page=1&sort=-name)
├── Path Parameters (/users/:id)
├── Form Fields (file uploads, textareas, rich text)
├── Headers (Authorization, custom headers)
├── Cookies (session data)
└── WebSocket Messages (real-time input)

Each surface needs:
  → Zod schema (type + format validation)
  → Sanitization (clean before validate)
  → Security check (anti-XSS, anti-injection)
```

## 6. Zod Schema Design Patterns

### Validation Layers
```
Layer 1: FORMAT     — z.string().email(), z.number().int()
Layer 2: CONSTRAINT — .min(2).max(100), .positive()
Layer 3: TRANSFORM  — .trim(), .toLowerCase(), .transform(sanitize)
Layer 4: REFINEMENT — .refine(businessRule, { message: "..." })
Layer 5: SECURITY   — .transform(stripHtml).transform(preventXSS)
```

### Checklist per Schema
- [ ] All fields have type validation
- [ ] String fields have min/max length
- [ ] Number fields have range constraints
- [ ] Enums used for fixed value sets
- [ ] Optional vs required clearly defined
- [ ] Default values where appropriate
- [ ] Transforms for normalization (trim, lowercase)
- [ ] Refinements for business rules
- [ ] Custom error messages (user-friendly)
- [ ] Security transforms (sanitize HTML, prevent XSS)

## 7. Security Validation Checklist

- [ ] **XSS Prevention**: All string outputs HTML-escaped or sanitized
- [ ] **SQL Injection**: All inputs go through Drizzle ORM (parameterized)
- [ ] **Prototype Pollution**: No `Object.assign(target, userInput)`
- [ ] **ReDoS**: No complex regex patterns with user input
- [ ] **Path Traversal**: File paths validated, `..` rejected
- [ ] **Mass Assignment**: Only whitelisted fields accepted (Zod `.pick()`)
- [ ] **Type Coercion**: `z.coerce.number()` not `parseInt(input)`

## 8. Error Message Design

```typescript
// Good: Specific, actionable
{ email: "Please enter a valid email address" }
{ name: "Name must be between 2 and 100 characters" }

// Bad: Generic, unhelpful
{ email: "Validation failed" }
{ name: "Invalid input" }
```

## 9. Pre-Delivery Checklist
- [ ] All input surfaces have Zod schemas
- [ ] Schemas include security transforms
- [ ] Error messages are user-friendly
- [ ] VALIDATION_SPEC.md documents all schemas
- [ ] TypeScript types inferred from schemas
- [ ] Same schemas usable on client + server
- [ ] SYSTEM_STATE.md updated

## 10. Exit
- Handoff: VALIDATION_SPEC.md + schemas → Backend + Frontend Builders.

## Rules
- EVERY input has a Zod schema. No exceptions.
- SANITIZE before VALIDATE. Clean data validates correctly.
- SAME schema on client AND server. One source of truth.
- SECURITY validation is separate from format validation.
- ERROR messages are for users, not developers.
- NEVER trust client-side validation alone.
