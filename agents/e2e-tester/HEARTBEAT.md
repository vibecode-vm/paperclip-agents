# HEARTBEAT.md -- End-to-End Test Automation Agent Checklist

## 0. Context Discovery (PFLICHT — siehe SHARED_CONFIG.md Rule 0)
- Prüfe ob `../../SYSTEM_STATE.md`, `../../MEMORY.md` existieren → LESEN
- Prüfe ob `docs/features/<feature>/STATUS.md` existiert → LESEN
- Lese `../SHARED_CONFIG.md` — Rules, Anti-Hallucination Protocol
- Wenn Files fehlen → erstelle sie aus Templates

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch test tasks from Feature Lifecycle Orchestrator.
- Prioritize `in_progress` → `todo`.

## 3. Goal Check (MANDATORY)
- Read the GOAL.md for the feature being tested.
- Read the PRD acceptance criteria.
- These ACs become your test scenarios.

## 4. Read Context
- Read `GOAL.md` → understand what should be achieved.
- Read PRD → extract acceptance criteria as test cases.
- Read `DESIGN_SPEC.md` → understand expected interactions.
- Know what you're testing AGAINST.

## 5. Open Build
```bash
agent-browser open <build-url>
agent-browser wait --load networkidle
```

## 6. Happy Path Tests
For each acceptance criterion:
```bash
# Execute the user journey
agent-browser click @element
agent-browser type @input "value"
agent-browser click @submit
agent-browser wait --text "Expected result"
agent-browser screenshot happy-path-[name].png
```
Record: PASS ✅ or FAIL ❌ with screenshot.

## 7. Error Path Tests
```bash
# Test invalid inputs
agent-browser type @email "not-an-email"
agent-browser click @submit
agent-browser screenshot error-[name].png

# Test empty submissions
agent-browser click @submit  # without filling required fields
agent-browser screenshot empty-[name].png
```

## 8. Responsive Tests
```bash
# Mobile
agent-browser set viewport 375 812
agent-browser screenshot responsive-mobile.png
# Verify functionality works at this viewport

# Tablet
agent-browser set viewport 768 1024
agent-browser screenshot responsive-tablet.png

# Desktop
agent-browser set viewport 1440 900
agent-browser screenshot responsive-desktop.png
```

## 9. Dark Mode Tests
```bash
agent-browser --color-scheme dark open <url>
agent-browser screenshot dark-mode.png
# Verify functionality works in dark mode
```

## 10. Edge Case Tests
- Empty states: What shows when there's no data?
- Long text: Does overflow break anything functionally?
- Rapid actions: Does double-clicking break state?
- Back button: Does navigation work correctly?
- Page refresh: Does state persist where expected?

## 10.5 🛡️ Self-Check BEFORE Delivery (Anti-Hallucination)
```
STOP — Bevor du TEST_REPORT.md ablieferst:
1. Lies PRD ACs nochmal → Habe ich NUR die geforderten User Journeys getestet?
2. Habe ich Test-Szenarien erfunden die NICHT in den ACs stehen? → ENTFERNEN
3. Sind alle ACs durch mindestens ein Test-Szenario abgedeckt? → Wenn nicht, HINZUFÜGEN
4. Habe ich Feature-Requests als "Findings" getarnt? → ENTFERNEN
```

## 11. Write TEST_REPORT.md
- Use template from TOOLS.md.
- Every scenario needs: steps, expected, actual, screenshot.
- Clear PASS ✅ or FAIL ❌ verdict.
- Include responsive + dark mode result tables.
- **Update STATUS.md**: "E2E Tests: X PASS, Y FAIL"
- Verdict: ALL PASS or HAS FAILURES.

## 12. Cleanup
```bash
agent-browser close
```

## Rules
- Screenshots for EVERY test result — pass or fail.
- Test FUNCTIONALITY, not visual appearance.
- Every acceptance criterion = at least one test scenario.
- FAIL means: include exact reproduction steps.
- Never modify code. Only test and report.
- Always test on all 3 viewports + both themes.
- **NUR ACs aus PRD/GOAL.md testen — keine erfundenen Szenarien.**
- **Self-Check (Step 10.5) ist PFLICHT vor Abgabe.**
