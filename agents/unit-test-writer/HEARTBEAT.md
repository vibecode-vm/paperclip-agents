# HEARTBEAT.md -- Component & Integration Test Writer Checklist

## 0. Context Discovery (PFLICHT — siehe SHARED_CONFIG.md Rule 0)
- Prüfe ob `../../SYSTEM_STATE.md`, `../../MEMORY.md` existieren → LESEN
- Prüfe ob `docs/features/<feature>/STATUS.md` existiert → LESEN
- Lese `../SHARED_CONFIG.md` — Rules, Anti-Hallucination Protocol
- Wenn Files fehlen → erstelle sie aus Templates

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch test-writing tasks from Feature Lifecycle Orchestrator.
- Prioritize `in_progress` → `todo`.

## 3. Goal Check (MANDATORY)
- Read the GOAL.md for the feature being tested.
- Understand which components/hooks/stores were created or modified.
- These become your test targets.

## 4. Environment Check
- Is vitest installed? → `npm list vitest --depth=0`
- Is @testing-library/react installed? → `npm list @testing-library/react --depth=0`
- If not → `npm install -D vitest @testing-library/react @testing-library/jest-dom @testing-library/user-event msw @vitejs/plugin-react`

## 5. Analyze Components to Test
- Read all new/modified source files.
- For each file, identify:
  - Props and their types → render tests per variant
  - User interactions → interaction tests
  - API calls → MSW mock tests
  - State changes → store tests
  - Edge cases → empty, error, loading, overflow

## 6. Write Tests
For each source file, create a test file:
```
FeatureName.tsx → FeatureName.test.tsx
useFeatureName.ts → useFeatureName.test.ts
featureStore.ts → featureStore.test.ts
```

**Test categories per component:**
1. ✅ Renders correctly with default props
2. ✅ Renders each variant/state (if applicable)
3. ✅ Handles user interactions
4. ✅ Shows loading state
5. ✅ Shows error state
6. ✅ Handles empty data
7. ✅ Accessibility (queryable by role/label)

## 7. Run Tests
```bash
npx vitest run
```
- Read the FULL output.
- Count: total, passed, failed, skipped.
- Check exit code (0 = all pass).

## 7.5 🛡️ Self-Check BEFORE Report (Anti-Hallucination)
```
STOP — Bevor du Test-Ergebnisse reportest:
1. Teste ich NUR Code der tatsächlich existiert? → Keine Tests für nicht-existierende Components
2. Sind meine Test-Fälle aus den PRD ACs abgeleitet? → Wenn nicht, überprüfen
3. Habe ich "Verbesserungen" am Code vorgeschlagen? → Nur TESTEN, nicht verbessern
4. Habe ich Tests für Features geschrieben die nicht gebaut wurden? → ENTFERNEN
```

## 8. Report Results

**If ALL PASS:**
- Comment on issue: "✅ X/X tests passed. Coverage: [summary]. Ready for Auditor review."
- **Update STATUS.md**: "Unit Tests: X/X PASS"
- Update status.

**If ANY FAIL:**
- Comment on issue with:
  - Which tests failed
  - Expected vs received
  - Likely cause
  - Suggestion for fix
- **Update STATUS.md**: "Unit Tests: X FAIL — [details]"
- Send back to Frontend Builder.
- Do NOT pass to Auditor.

## 9. Exit
- Comment on in_progress work.
- Tests are committed alongside source code.

## Rules
- NEVER modify source code. Only write test files.
- NEVER skip failing tests.
- NEVER report PASS without running `npx vitest run` first.
- DO NOT test shadcn/ui components (tested upstream).
- Use accessible queries (getByRole, getByLabelText) over test IDs.
- One test file per source file. Name: `[SourceFile].test.tsx`.
- **NUR existierenden Code testen — keine Tests für Fantasie-Components.**
- **Self-Check (Step 7.5) ist PFLICHT vor Abgabe.**
