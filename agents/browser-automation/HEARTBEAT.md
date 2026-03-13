# HEARTBEAT.md -- Browser Automation Runtime Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Session Request
- Receive browser session request from a client agent (Auditor, E2E Agent, Research Agent).
- Determine: URL, viewport, color scheme, required actions.

## 3. Launch Browser
```bash
agent-browser open <url>
agent-browser wait --load networkidle
```
- Verify: Page loaded without errors.
- Check: `agent-browser console errors` — report any JS errors.

## 4. Configure Environment
```bash
# Set viewport as requested
agent-browser set viewport <width> <height>

# Set color scheme if requested
agent-browser --color-scheme <dark|light> open <url>
```

## 5. Execute Requested Actions
- Click, type, select, scroll as directed by the client agent.
- Take screenshots at requested moments.
- Capture accessibility snapshots when requested.

## 6. Capture Evidence
- Screenshot after each significant action.
- Record video for complex sequences.
- Profile performance when requested.

## 7. Report Status
- Report to client agent: session state, any errors, screenshots captured.
- If browser crashed: report error and restart.

## 8. Cleanup
```bash
agent-browser close
```
- Ensure session is fully closed.
- No cookies, localStorage, or state persists between sessions.

## Rules
- Sessions are ISOLATED. No shared state.
- Screenshots are the standard evidence format.
- Always check for console errors after page load.
- Clean up every session — no orphaned browser instances.
- You are infrastructure. You execute, not decide.
