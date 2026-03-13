# HEARTBEAT.md — Testability Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — coverage targets, mandated stack.
- Read `../../MEMORY.md` — known testing gotchas.
- Check test config: `ls vitest.config.* jest.config.*`
- Check existing test coverage: `npx vitest run --coverage`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — what components exist.
2. Scan existing tests: `find src -name "*.test.*" -o -name "*.spec.*"`
3. Check coverage report: `npx vitest run --coverage`

## 5. Testability Analysis

### Code Risk Assessment
```
For each module/component:
  1. Cyclomatic complexity: branches, loops, conditions
  2. External dependencies: APIs, DB, filesystem, time
  3. Side effects: mutations, I/O, network calls
  4. Input surface: how many parameters, combinations
  →  Risk = Complexity × Dependencies × Side Effects
```

### Testability Score
| Score | Meaning | Action |
|-------|---------|--------|
| 🟢 High | Pure functions, clear I/O | Test directly |
| 🟡 Medium | Some dependencies, testable with mocks | Mock strategy needed |
| 🔴 Low | Tightly coupled, many side effects | Refactor first, then test |

## 6. Test Pyramid Design

```
       /  E2E  \      ← 10%: Critical user journeys
      /----------\
     / Integration \   ← 20%: API endpoints, DB queries, auth flows
    /--------------\
   /   Unit Tests   \  ← 70%: Pure logic, validation, utilities, hooks
  /------------------\
```

### Unit Tests (70%)
- Pure functions and utilities
- Zod schema validation
- State management logic (Zustand stores)
- Custom hooks (with renderHook)
- Component rendering (with user events)

### Integration Tests (20%)
- API Route Handlers (request → response)
- Database queries (with test DB)
- Auth flows (login → session → protected route)
- Server Actions (form → mutation → revalidation)

### E2E Tests (10%)
- Critical user journeys (signup, login, core feature)
- Cross-page navigation flows
- Payment/checkout flows (if applicable)

## 7. Mock Strategy Decision Tree

```
Should I mock this?
├── External API? → YES, mock with MSW or vi.fn()
├── Database? → DEPENDS
│   ├── Unit test → YES, mock the DB client
│   └── Integration test → NO, use test DB
├── Time/Date? → YES, use vi.useFakeTimers()
├── Random values? → YES, seed the RNG
├── Business logic? → NO! Test the real thing
├── Validation (Zod)? → NO! Test against real schemas
└── UI rendering? → NO! Test with Testing Library
```

## 8. Edge Case Checklist
- [ ] Empty input (null, undefined, "", [], {})
- [ ] Boundary values (0, 1, MAX_INT, max length)
- [ ] Invalid types (string where number expected)
- [ ] Unicode / special characters
- [ ] Concurrent access (race conditions)
- [ ] Network failure (timeout, 500, offline)
- [ ] Auth edge cases (expired token, wrong role)
- [ ] Pagination limits (page 0, negative, beyond total)

## 9. Pre-Delivery Checklist
- [ ] TEST_STRATEGY.md written with pyramid, mock strategy, edge cases
- [ ] Risk assessment documented per module
- [ ] Coverage targets defined (meets SHARED_CONFIG minimums)
- [ ] Flaky test risks identified
- [ ] Handoff to Unit Test Writer / E2E Tester

## 10. Exit
- Handoff: TEST_STRATEGY.md → Unit Test Writer + E2E Tester

## Rules
- Test BEHAVIOR, not implementation details.
- Test PYRAMID, not test ice cream cone.
- MOCK external dependencies, NOT internal logic.
- EVERY edge case documented gets a test.
- FLAKY tests are bugs — fix immediately.
