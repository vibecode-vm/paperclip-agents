# HEARTBEAT.md -- API & Database Backend Builder Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack, MCP servers, shared rules.
- Read `../../MEMORY.md` — known gotchas, architecture decisions.
- Database connection? → Verify `DATABASE_URL` env var
- Supabase configured? → Verify `NEXT_PUBLIC_SUPABASE_URL` + `NEXT_PUBLIC_SUPABASE_ANON_KEY`
- Drizzle initialized? → Check `drizzle.config.ts` exists
- If not → `npx drizzle-kit init`
- All mandatory packages installed? (see SOUL.md stack)
- If not → `npm install <missing>`

## 4. Read System State + Docs (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — know what exists: routes, components, stores, APIs, DB tables.
2. Read `docs/lib/` — latest library docs (fetched by Stack Researcher via Context7).
3. If `docs/lib/` has docs for Drizzle, Supabase, Zod → READ THEM. They have the latest API.
4. Read existing schema files → `ls src/db/schema/`
5. Check current migration status → `npx drizzle-kit check`

**After finishing work:**
Update `../../SYSTEM_STATE.md` with everything you created/changed.

## 5. Schema Design (Drizzle ORM)
1. **Schema first**: Define tables in `src/db/schema/[name].ts`
2. **Relations**: Define in `src/db/relations.ts`
3. **Enums**: Use `pgEnum` — no magic strings
4. **Conventions**:
   - UUID primary keys (`defaultRandom()`)
   - `created_at` + `updated_at` on every table
   - Soft delete via `deleted_at` where appropriate
   - snake_case for DB columns, camelCase for TypeScript
5. **Generate migration**: `npx drizzle-kit generate`
6. **Review SQL**: Read the generated migration file — verify it does what you expect
7. **Apply**: `npx drizzle-kit migrate`

## 6. Validation Layer (Zod)
- Create Zod schemas in `src/lib/validations/[name].ts`
- **Shared schemas**: Frontend + Backend use the SAME Zod schemas
- Schema for every: request body, query params, path params
- Export inferred types: `export type CreateUserInput = z.infer<typeof createUserSchema>`

## 7. API Implementation
**For each endpoint:**
1. Choose: Route Handler (complex logic, streaming) vs Server Action (simple mutations)
2. Add Zod validation at the entry point
3. Check auth: `const { data: { user } } = await supabase.auth.getUser()`
4. Implement business logic
5. Handle errors with `handleApiError()` utility
6. Return typed response

**Patterns:**
- `GET` list → pagination + sorting + filtering
- `GET` detail → 404 if not found
- `POST` create → 201 + return created resource
- `PATCH` update → partial updates + 404 if not found
- `DELETE` → 404 if not found, return `{ deleted: true }`

## 8. Auth & Security
- [ ] RLS policies defined for every new table
- [ ] Auth middleware covers protected routes
- [ ] Input validated with Zod on every endpoint
- [ ] No secrets in code (all via `process.env`)
- [ ] Error responses don't leak internal details
- [ ] Rate limiting on auth endpoints (if applicable)

## 9. Testing
- Write integration tests for every endpoint (see TOOLS.md patterns)
- Run tests: `npx vitest run src/app/api/`
- Run with coverage: `npx vitest run --coverage src/app/api/`
- Verify: all tests PASS, coverage meets targets (see SHARED_CONFIG.md)
- Test error paths: invalid input, unauthorized, not found, duplicate

## 10. Pre-Delivery Checklist
- [ ] All migrations generated and applied cleanly
- [ ] RLS policies active on all new tables
- [ ] Zod validation on every endpoint (request + response types)
- [ ] Error handling: 400, 401, 403, 404, 409, 500 responses tested
- [ ] Auth: protected routes require authentication
- [ ] No `any` types — everything typed with TypeScript + Zod
- [ ] API tests PASS with coverage ≥ targets
- [ ] `npm audit` — no critical vulnerabilities
- [ ] Seed data script works (if applicable)
- [ ] SYSTEM_STATE.md updated with new endpoints, tables, schemas

## 11. Update SYSTEM_STATE.md (MANDATORY)
- Update `../../SYSTEM_STATE.md` with:
  - New API endpoints (path, method, auth required)
  - New database tables (name, key columns)
  - New Server Actions (name, purpose)
  - New Zod schemas (shared with frontend)
  - New environment variables needed
  - Migration status
  - Known issues discovered
- Update `../../MEMORY.md` if you learned new gotchas or patterns.
- Timestamp the update.

## 12. Exit
- Comment on in_progress work.
- Structured PR: What → Why → How → Migration steps → Breaking changes.

## Rules
- Schema FIRST, implementation SECOND. Never write API code without a schema.
- ALWAYS latest package versions.
- Zod at EVERY boundary (request body, query params, path params, external API responses).
- RLS on EVERY table. No exceptions.
- Tests for EVERY endpoint. Happy path + error paths.
- TypeScript strict mode. No `any` types.
- SYSTEM_STATE.md is always current.
- Never expose secrets in logs or responses.
- Always include `X-Paperclip-Run-Id` on mutating Paperclip API calls.
