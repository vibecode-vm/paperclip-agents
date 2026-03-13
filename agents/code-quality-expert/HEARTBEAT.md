# HEARTBEAT.md — Code Quality Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack, coverage targets.
- Read `../../MEMORY.md` — known quality patterns.
- Check lint config: `ls .eslintrc* eslint.config.* .prettierrc*`
- Check TS config: `cat tsconfig.json | grep strict`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — what exists.
2. Run lint: `npm run lint` or `npx eslint src/`
3. Run type check: `npx tsc --noEmit`
4. Check for `any` types: `grep -r ": any" src/ --include="*.ts" --include="*.tsx" | wc -l`

## 5. Static Analysis

### ESLint Check
```bash
npx eslint src/ --format=json > /tmp/eslint-report.json
# Parse: errors, warnings, fixable count
```

### TypeScript Strict Check
```bash
npx tsc --noEmit 2>&1 | tail -20
# Count: errors, any-usage, missing types
```

### Bundle Size Check
```bash
npx next build
# Check .next/analyze/ if @next/bundle-analyzer installed
```

## 6. Complexity Analysis

### Thresholds
| Metric | OK | Warning | Critical |
|--------|-----|---------|----------|
| Cyclomatic Complexity (per function) | ≤ 5 | 6-10 | > 10 |
| Function length (lines) | ≤ 30 | 31-50 | > 50 |
| File length (lines) | ≤ 300 | 301-500 | > 500 |
| Nesting depth | ≤ 3 | 4 | > 4 |
| Function parameters | ≤ 3 | 4-5 | > 5 |

## 7. Code Smell Detection

| Smell | Indicator | Severity | Fix |
|-------|-----------|----------|-----|
| Long Method | > 30 lines | 🟠 | Extract method |
| God Class | > 5 responsibilities | 🔴 | Split class |
| Feature Envy | Accesses other class's data heavily | 🟠 | Move method |
| Duplicate Code | > 3 similar blocks | 🟡 | Extract shared function |
| Dead Code | Unreachable/unused code | 🟡 | Remove |
| Deep Nesting | > 3 levels | 🟠 | Early returns, guard clauses |
| Magic Numbers | Hardcoded values | 🟡 | Named constants |
| any Types | TypeScript `any` | 🔴 | Proper typing |

## 8. SOLID Compliance

- [ ] **S** — Single Responsibility: each function/class does ONE thing
- [ ] **O** — Open/Closed: extendable without modifying existing code
- [ ] **L** — Liskov Substitution: subtypes are substitutable
- [ ] **I** — Interface Segregation: no unused interface members
- [ ] **D** — Dependency Inversion: depend on abstractions

## 9. Pre-Delivery Checklist
- [ ] QUALITY_REPORT.md written with all findings
- [ ] Issues categorized: 🔴 Blocker / 🟠 Important / 🟡 Suggestion
- [ ] Refactoring steps provided for each blocker
- [ ] ESLint / TypeScript errors at zero
- [ ] TECH_DEBT_LOG.md updated if shortcuts found

## 10. Exit
- Handoff: QUALITY_REPORT.md → Builder for fixes.
- Update TECH_DEBT_LOG.md if new debt identified.

## Rules
- `any` types are BLOCKERS, not suggestions.
- Refactoring requires test coverage — never refactor untested code.
- Style-only issues are SUGGESTIONS, not blockers.
- Every finding has a specific remediation step.
- TECH_DEBT_LOG.md is always current.
