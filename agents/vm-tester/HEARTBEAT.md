# HEARTBEAT.md -- VM Integration Test Runtime Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch VM test tasks from Feature Lifecycle Orchestrator.
- Only triggered when Feature Lifecycle Orchestrator determines VM test is needed.

## 3. Goal Check (MANDATORY)
- Read the GOAL.md for the feature being tested.
- Confirm this feature REQUIRES VM testing (not just browser).
- If web-only feature → report back: "VM test not needed, E2E Agent handles this."

## 4. Environment Setup
```bash
vm-desktop connect <vm-id>
vm-desktop exec "uname -a"
vm-desktop exec "free -h"
vm-desktop screenshot environment-baseline.png
```

## 5. Installation Test
- Download the build artifact.
- Install on a clean VM.
- Screenshot every installation step.
- Record: install time, errors, warnings.

```bash
vm-desktop exec "wget <download-url> -O installer"
vm-desktop exec "./installer"
vm-desktop screenshot installation-complete.png
```

## 6. Launch Test
- Start the application.
- Record: startup time, initial state.
- Screenshot the main window/screen.

```bash
vm-desktop exec "./app &"
vm-desktop screenshot app-launched.png
```

## 7. Functional Tests
For each native feature:
- Test file operations (open, save, export).
- Test native dialogs (file picker, alerts, confirmations).
- Test keyboard shortcuts.
- Test system tray / taskbar integration.
- Screenshot every result.

## 8. Performance Check
- Cold start time.
- Memory usage (idle).
- CPU usage (idle).
- Compare against targets.

```bash
vm-desktop exec "ps aux | grep app"
vm-desktop exec "free -h"
```

## 9. Cross-Platform (if applicable)
- Repeat tests on additional OS environments.
- Document platform-specific differences.

## 10. Write VM_TEST_REPORT.md
- Use template from TOOLS.md.
- Every test step needs a screenshot.
- Include environment details.
- Clear PASS/FAIL verdict.

## 11. Cleanup
```bash
vm-desktop exec "pkill app"  # Stop application
vm-desktop disconnect
```

## Rules
- Screenshot at EVERY step — no claims without visual evidence.
- Always report environment details (OS, memory, display).
- If VM test is not needed → say so and exit. Don't run unnecessary tests.
- Never modify source code. Only test and report.
- Use isolated VMs — never test on production.
- Always clean up after testing.
