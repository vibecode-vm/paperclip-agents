# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Test-Driven Development — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**THE IRON LAW: NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST.**

Design test strategies that enforce RED-GREEN-REFACTOR:
- Tests MUST be run and verified to fail BEFORE implementation
- Tests MUST test real behavior, not mocks
- YAGNI: minimal implementation per test

**Testing Anti-Patterns (Superpowers):**
- Writing tests AFTER code → biased toward implementation, not behavior
- Testing mocks instead of real code → false confidence
- Vague test names ("it works") → ambiguous failures
- Multiple behaviors per test → hard to isolate failures
- Skipping RED phase → you don't know if the test tests the right thing

### 2. Web App Testing — webapp-testing (Anthropic, 22K)
Decision tree: server-side vs browser tests. Recon-then-action pattern.

### 3. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

When flaky tests surface: root cause investigation FIRST (read errors, reproduce, check recent changes). Pattern analysis. Hypothesis testing. Fix at root cause, not symptom.

---

## Templates

### TEST_STRATEGY.md
```markdown
# Test Strategy: [Feature Name]
> Author: Testability Expert
> Date: [date]
> Feature: [docs/features/<name>/GOAL.md]

## Risk Assessment

| Module | Complexity | Dependencies | Side Effects | Risk | Priority |
|--------|-----------|-------------|-------------|------|----------|
| [module] | High (CC=12) | DB, Auth | Mutations | 🔴 | P1 |
| [module] | Low (CC=3) | None | Pure | 🟢 | P3 |

## Test Pyramid

### Unit Tests (Target: 70%)
| What to test | Pattern | Mock? |
|-------------|---------|-------|
| [validation logic] | Schema test | No |
| [utility function] | Input/Output | No |
| [hook] | renderHook | Partial |

### Integration Tests (Target: 20%)
| What to test | Setup | Mock? |
|-------------|-------|-------|
| [API endpoint] | Test DB | External APIs only |
| [auth flow] | Supabase test | No |

### E2E Tests (Target: 10%)
| User Journey | Steps | Critical? |
|-------------|-------|-----------|
| [signup flow] | 5 steps | ✅ |

## Mock Strategy
| Dependency | Mock? | Why | How |
|-----------|-------|-----|-----|
| [external API] | ✅ | Unreliable in CI | MSW |
| [database] | ❌ | Need real queries | Test DB |

## Edge Cases
- [ ] [edge case 1]: [test description]
- [ ] [edge case 2]: [test description]

## Coverage Targets
| Metric | Minimum | Feature Target |
|--------|---------|---------------|
| Statements | 80% | 85% |
| Branches | 75% | 80% |
| Functions | 80% | 85% |
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Coverage targets (Quality Gate)
- Mandated test runner (Vitest)
