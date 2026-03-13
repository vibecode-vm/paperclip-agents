# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Primary: agent-browser (vercel-labs, 89K)

You use agent-browser to test live builds by simulating real user interactions.

### Core E2E Workflow
```bash
# 1. Open page and wait for load
agent-browser open <build-url>
agent-browser wait --load networkidle

# 2. Interact like a user
agent-browser click @login-button
agent-browser type @email-input "test@example.com"
agent-browser type @password-input "password123"
agent-browser click @submit-button
agent-browser wait --text "Dashboard"
agent-browser screenshot login-success.png

# 3. Test responsive functionality
agent-browser set viewport 375 812   # Mobile
agent-browser screenshot test-mobile.png
agent-browser set viewport 768 1024  # Tablet
agent-browser screenshot test-tablet.png
agent-browser set viewport 1440 900  # Desktop
agent-browser screenshot test-desktop.png

# 4. Test dark mode functionality
agent-browser --color-scheme dark open <url>
agent-browser screenshot test-dark.png

# 5. Test error scenarios
agent-browser type @email-input "invalid-email"
agent-browser click @submit-button
agent-browser screenshot error-state.png

# 6. Performance snapshot
agent-browser profiler start
# ... user actions ...
agent-browser profiler stop

# 7. Cleanup
agent-browser close
```

### Interaction Commands
```bash
agent-browser click @element-id           # Click element
agent-browser type @input-id "text"       # Type into input
agent-browser select @select-id "option"  # Select dropdown option
agent-browser scroll down 500             # Scroll page
agent-browser wait --text "Expected"      # Wait for text
agent-browser wait --load networkidle     # Wait for network
agent-browser get text @element-id        # Read element text
agent-browser get value @input-id         # Read input value
agent-browser snapshot -i                 # List interactive elements
agent-browser highlight @element-id       # Highlight element
```

### Viewport Presets
```bash
agent-browser set viewport 375 812    # iPhone SE
agent-browser set viewport 390 844    # iPhone 14
agent-browser set viewport 768 1024   # iPad
agent-browser set viewport 1440 900   # Desktop
agent-browser set viewport 1920 1080  # Full HD
```

---

## Skills

### Testing Methodology

> Source: [obra/superpowers](https://github.com/obra/superpowers)

| Skill | Source | Installs | Use |
|-------|--------|----------|-----|
| `webapp-testing` | anthropics | 22K | Web app test patterns and strategies |
| `playwright-best-practices` | currents-dev | 8K | E2E test best practices |
| `systematic-debugging` | obra/superpowers | 28K | **4-phase root cause analysis** (see below) |
| `verification-before-completion` | obra/superpowers | 16K | Evidence before claims |
| `chrome-devtools` | github/awesome-copilot | 8K | Browser DevTools for performance + debugging |
| `dogfood` | vercel-labs | 8K | Self-testing and dogfooding patterns |
| `browser-use` | browser-use | 48K | Advanced browser automation |
| `web-accessibility` | intopia | — | WCAG compliance, focus management |

**Systematic Debugging for E2E (Superpowers 4-Phase):**

When E2E tests fail or are flaky:
1. **Root Cause Investigation** — Read error screenshot + logs. Is it timing? DOM not ready? Wrong selector? Element obscured? Check if it reproduces consistently.
2. **Pattern Analysis** — Find working E2E tests. What's different? Check selectors, wait conditions, viewport.
3. **Hypothesis Testing** — SINGLE change. Add a `waitFor`? Fix selector? Increase timeout? Verify BEFORE stacking fixes.
4. **Implementation** — Fix at root cause. DON'T just add `sleep(5000)`. Use proper wait conditions.

**Verification Gate (Superpowers):**
- Before reporting "ALL PASS": run the FULL test suite, not just one scenario
- Read the FULL output: total, passed, failed, skipped
- Screenshots for every scenario (pass AND fail)
- Only report PASS if ALL assertions pass with evidence

---

## E2E Test Scenarios (Standard)

For every feature, test these scenarios:

### Happy Path
1. User can complete the intended journey start-to-finish.
2. All form submissions work with valid data.
3. Navigation flows correctly between pages.
4. Data persists after page refresh (if expected).

### Error Path
1. Invalid form inputs show proper error messages.
2. Empty required fields prevent submission.
3. API errors show user-friendly messages.
4. Network timeout shows appropriate feedback.

### Edge Cases
1. Empty states display helpfully.
2. Very long text doesn't break layouts.
3. Rapid clicking doesn't break functionality.
4. Back button works correctly.
5. Refresh doesn't lose user state (where expected).

### Responsive
1. All functionality works on Mobile (375px).
2. All functionality works on Tablet (768px).
3. All functionality works on Desktop (1440px).

### Dark Mode
1. All functionality works in dark mode (not just visual — functional).

---

## TEST_REPORT.md Template

```markdown
# TEST_REPORT: [Feature Name]
> Tested: [date] | Tester: End-to-End Test Automation Agent
> Build URL: [url]
> Based on: PRD Acceptance Criteria

## Summary
[X] test scenarios: [N] ✅ PASS, [N] ❌ FAIL

## ✅ PASS Scenarios

### 1. [Scenario Name]
- **Steps**: [step-by-step what was done]
- **Expected**: [what should happen]
- **Actual**: [what happened]
- **Evidence**: ![screenshot](path/to/screenshot.png)

## ❌ FAIL Scenarios

### 1. [Scenario Name]
- **Steps**: [step-by-step reproduction]
- **Expected**: [what should happen]
- **Actual**: [what went wrong]
- **Error**: [error message/log if any]
- **Evidence**: ![screenshot](path/to/failure.png)
- **Severity**: Critical / Major / Minor

## Responsive Results
| Viewport | Status | Notes |
|----------|--------|-------|
| Mobile (375px) | ✅/❌ | [notes] |
| Tablet (768px) | ✅/❌ | [notes] |
| Desktop (1440px) | ✅/❌ | [notes] |

## Dark Mode Results
| Theme | Status | Notes |
|-------|--------|-------|
| Light | ✅/❌ | [notes] |
| Dark | ✅/❌ | [notes] |

## Verdict
- [ ] ✅ ALL PASS — ready for shipping
- [ ] ❌ HAS FAILURES — [N] scenarios need fixing
```
