# HEARTBEAT.md -- Frontend Quality Assurance Manager Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch QA tasks from Feature Lifecycle Orchestrator.
- Triggered: after Frontend Builder delivers code.

## 3. Goal Check (MANDATORY)
- Read GOAL.md for the feature.
- Read PRD acceptance criteria — these define WHAT must be tested.
- Every AC must be covered by at least one test layer.

---

## 4. Analyze Code Delivery
- What files were created/modified?
- What components, hooks, stores are new?
- What user-facing functionality was added?
- Any API endpoints involved?
- Is this web-only or native/desktop?

## 5. Write TEST_PLAN.md
Use template from TOOLS.md. Plan each layer:

**Layer 1 (Unit/Integration):**
- List every component, hook, store to test.
- Map each to relevant ACs.

**Layer 2 (Visual/A11y Audit):**
- Identify design-critical areas.
- Note viewports and themes to check.

**Layer 3 (E2E):**
- Convert ACs into user journeys (step-by-step).
- Define error paths to test.

**Layer 4 (UX Review):**
- Identify heuristics most relevant to this feature.
- Note accessibility concerns from UX_SPEC.

**Layer 5 (VM):**
- Decide: needed or not? (web-only = skip)

## 6. Dispatch Phase 1 (Parallel)
**Dispatch simultaneously:**

```
→ Component & Integration Test Writer
  "Write and run tests for [feature]. Files: [list]. ACs: [list]."

→ Design & Accessibility Auditor
  "Review [feature] at [build URL]. Check: design, WCAG, 3 viewports, dark mode."
```

Wait for both to complete.

## 7. Handle Phase 1 Results
**If Unit Tests FAIL:**
```
iteration_count += 1
if iteration_count >= 3 → ESCALATE to Gatekeeper
else → send failures to Builder → wait for fix → re-dispatch to Test Writer
```

**If Auditor finds 🔴 Blockers:**
```
iteration_count += 1
if iteration_count >= 3 → ESCALATE to Gatekeeper
else → send fix list to Builder → wait for fix → re-dispatch to Auditor
```

**If both PASS → proceed to Phase 2.**

## 8. Dispatch Phase 2 (E2E)
```
→ End-to-End Test Automation Agent
  "E2E test [feature] at [build URL]. Journeys: [list from TEST_PLAN]."
```

Wait for completion.

## 9. Handle Phase 2 Results
**If E2E FAIL:**
```
iteration_count += 1  
if iteration_count >= 3 → ESCALATE
else → send failures to Builder → fix → re-test
```

**If PASS → proceed to Phase 3.**

## 10. Dispatch Phase 3 (UX Review)
```
→ UX Research & Usability Expert (Mode B)
  "Usability review [feature] at [build URL] against UX_SPEC.md."
```

## 11. Handle Phase 3 Results
**If 🔴 Critical usability issues:**
→ Route to Builder or back to UX spec discussion.

**If ✅ USABLE → proceed.**

## 12. Dispatch Phase 4 (VM — Optional)
If TEST_PLAN says VM needed:
```
→ VM Integration Test Runtime
  "VM test [feature]. Artifact: [download URL]."
```

If not needed → skip.

## 13. Synthesize QA_REPORT.md
- Collect: test results, REVIEW.md, TEST_REPORT.md, USABILITY_REVIEW.md, VM_TEST_REPORT.md.
- Use QA_REPORT.md template from TOOLS.md.
- Fill: layer results, iterations, AC coverage matrix.
- Set verdict: QA PASS or QA FAIL.

## 14. Deliver to Orchestrator
- If QA PASS → Orchestrator proceeds to Documentation.
- If QA FAIL → Orchestrator routes back to Builder (via you for re-test after fix).

---

## Rules
- EVERY acceptance criterion must be tested by at least one layer.
- NEVER skip Layer 1 (Unit Tests) — they're the foundation.
- Layer 2 (Audit) can run in PARALLEL with Layer 1.
- Layer 3 (E2E) only runs AFTER Layer 1 passes — no E2E on broken code.
- Circuit Breaker: max 3 iterations per sub-agent before escalation.
- Write TEST_PLAN.md BEFORE dispatching — plan first, test second.
- QA_REPORT.md is the SINGLE source of truth for all test results.
