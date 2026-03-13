# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)
> Core philosophy: "Random fixes waste time and create new bugs."

**The 4-Phase Process:**

#### Phase 1: Root Cause Investigation
```bash
# Read error messages
cat logs/error.log | tail -50

# Check recent changes
git diff HEAD~3
git log --oneline -10

# Multi-component diagnostics
echo "=== Layer 1: Request ===" && curl -v http://localhost:3000/api/test
echo "=== Layer 2: Middleware ===" && grep -n "middleware" src/app/api/*/route.ts
echo "=== Layer 3: Database ===" && npx drizzle-kit check
echo "=== Layer 4: Response ===" && curl -s http://localhost:3000/api/health | jq .
```

**Root Cause Tracing (backward from symptom):**
1. Where does the bad value appear? Note the exact line.
2. What called this function with the bad value? Trace UP.
3. Keep tracing until you find the SOURCE.
4. Fix at SOURCE, not at symptom.

#### Phase 2: Pattern Analysis
```bash
# Find working examples of similar code
grep -rn "similar-pattern" src/ --include="*.ts" -l

# Compare working vs broken
diff working-file.ts broken-file.ts
```

#### Phase 3: Hypothesis Testing
```bash
# Make SMALLEST change to test ONE hypothesis
# Run tests to verify
npx vitest run path/to/affected-test.ts
```

#### Phase 4: Implementation
```bash
# After root cause identified, fix and verify
npx vitest run      # All tests pass
npx tsc --noEmit    # Type check passes
npm run lint        # Lint passes
```

### 2. Verification — verification-before-completion (obra/superpowers, 16K)

Evidence before claims:
1. Execute the command that proves the fix
2. Read the output COMPLETELY
3. Verify output confirms the claim
4. Only THEN declare the bug fixed

### 3. Defense in Depth — defense-in-depth (obra/superpowers)

When fixing a bug, add defensive layers:
- Add input validation at the boundaries
- Add assertion checks at critical points
- Add meaningful error messages that explain WHAT went wrong
- Add monitoring/logging for the failure condition

### 4. Condition-Based Waiting — condition-based-waiting (obra/superpowers)

For timing-related bugs:
- NEVER use `sleep()` or `setTimeout()` as fixes
- Wait for CONDITIONS: "wait until element exists", "wait until response received"
- Use polling with timeout: check condition every 100ms, fail after 10s
- Flaky tests are usually timing bugs — find the race condition

---

## Templates

### ROOT_CAUSE.md
```markdown
# ROOT_CAUSE: [Bug Title]
> Debugger: Systematic Debugger
> Date: [date]
> Reported by: [Agent / Human]

## Symptom
[What was observed — error message, test failure, unexpected behavior]

## Root Cause
**[One sentence: The exact cause]**

### Evidence
- Error at: `[file:line]`
- Data flow: [where the bad value originates]
- Introduced by: [commit/change that caused it]

### Investigation Trail
1. Phase 1: [what was checked, what was found]
2. Phase 2: [what was compared, differences found]
3. Phase 3: [hypothesis tested, result]

## Fix
```diff
- [old code]
+ [new code]
```

## Regression Test
```typescript
test('[describes the bug scenario]', () => {
  // This test would have caught the bug
})
```

## Prevention
- [ ] Add [validation/check/guard] to prevent recurrence
- [ ] Add [monitoring/alert] for this failure mode
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack with versions
- Shared rules (Goal Check, Evidence, Circuit Breaker)
- Anti-Hallucination Protocol
