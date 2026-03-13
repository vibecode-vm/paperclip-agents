# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Code Review — requesting-code-review (obra/superpowers, 11K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Pre-Review Checklist (MANDATORY before requesting review):**
- [ ] All tests pass
- [ ] Linter passes with zero errors
- [ ] Self-review complete (re-read every changed line)
- [ ] No debug/temporary code left
- [ ] Changes match spec/plan

**Review issues by severity:**
- 🔴 Critical — blocks progress, must fix
- 🟡 Important — should fix before merge
- 🟢 Nit — optional improvement

### 2. Receiving Code Review — receiving-code-review (obra/superpowers, 16K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Feedback Processing Protocol:**
1. **Read**: Understand the full feedback before responding
2. **Verify**: Check if reviewer's observation is factually correct
3. **Evaluate**: Is this a real issue or a style preference?
4. **Respond**: Explain disagreements with evidence, not ego
5. **Implement**: Fix confirmed issues, re-run tests, verify

### 3. TypeScript Advanced Types — typescript-advanced-types (wshobson, 13K)
Ensure proper type usage: discriminated unions, generics, utility types, `satisfies`.

---

## Templates

### QUALITY_REPORT.md
```markdown
# Quality Report: [Feature / Module]
> Author: Code Quality Expert
> Date: [date]
> Files Analyzed: [count]

## Summary

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| ESLint Errors | 0 | 0 | ✅ |
| ESLint Warnings | 3 | ≤ 5 | ✅ |
| TypeScript `any` | 0 | 0 | ✅ |
| Avg CC per function | 4.2 | ≤ 10 | ✅ |
| Max CC (single fn) | 12 | ≤ 10 | 🔴 |
| Bundle Size Impact | +12KB | — | 🟡 |

## 🔴 Blockers (must fix before shipping)

### 1. [File:Line] — [Issue]
**Smell**: [e.g., Long Method]
**Complexity**: CC = 12
**Fix**: Extract [X] into separate function
```diff
-function processAll(data) { /* 50 lines */ }
+function validateData(data) { /* 15 lines */ }
+function transformData(data) { /* 15 lines */ }
+function saveData(data) { /* 10 lines */ }
```

## 🟠 Important (fix before next sprint)

### 1. [File:Line] — [Issue]
...

## 🟡 Suggestions (improve when convenient)

### 1. [File:Line] — [Issue]
...
```

### TECH_DEBT_LOG.md
```markdown
# Tech Debt Log

> Last updated: [timestamp]

| # | Location | Debt Type | Impact | Effort | Priority | Added | Target Sprint |
|---|----------|----------|--------|--------|----------|-------|--------------|
| 1 | `src/utils/parser.ts` | High complexity (CC=15) | Bugs likely | 2h | 🔴 | Sprint 3 | Sprint 5 |
| 2 | `src/components/Form.tsx` | 3 `any` types | Type safety | 1h | 🟠 | Sprint 3 | Sprint 4 |
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack (TypeScript strict)
- Coverage targets
- Shared rules
