# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## VM Desktop Automation

### Connecting to VM
```bash
# Connect to a VM environment
vm-desktop connect <vm-id>

# Check VM environment
vm-desktop exec "uname -a"         # OS info
vm-desktop exec "cat /etc/os-release"  # Linux distro
vm-desktop exec "free -h"           # Memory
vm-desktop exec "df -h"             # Disk space
```

### Installation Testing
```bash
# Download and install
vm-desktop exec "wget <download-url> -O installer"
vm-desktop exec "chmod +x installer && ./installer"
vm-desktop screenshot install-step.png

# npm-based
vm-desktop exec "cd /app && npm install"
vm-desktop exec "cd /app && npm run build"
vm-desktop exec "cd /app && npm run start &"
vm-desktop screenshot app-running.png

# Electron/Tauri app
vm-desktop exec "dpkg -i app.deb"    # Debian
vm-desktop exec "rpm -i app.rpm"      # Red Hat
vm-desktop screenshot native-installed.png
```

### Desktop Interaction
```bash
# Click on elements
vm-desktop click <x> <y>
vm-desktop click @element-id

# Type text
vm-desktop type "Hello World"
vm-desktop type @input-field "test value"

# Keyboard shortcuts
vm-desktop key ctrl+s     # Save
vm-desktop key ctrl+o     # Open
vm-desktop key alt+f4     # Close
vm-desktop key enter      # Confirm

# Screenshots
vm-desktop screenshot test-result.png
vm-desktop screenshot --region 0,0,800,600 cropped.png

# Video recording
vm-desktop record start test-session.mp4
# ... actions ...
vm-desktop record stop
```

### File System Tests
```bash
# Test file operations
vm-desktop exec "ls -la /app/output/"
vm-desktop exec "test -f /app/config.json && echo 'EXISTS' || echo 'MISSING'"
vm-desktop exec "cat /app/output/result.json"
```

---

## Skills

### 1. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**When VM tests fail:**
1. **Root Cause**: Screenshot the state. Is the app running? (check `ps aux`). Is the display visible? (check VM connection). Is the window focused?
2. **Pattern Analysis**: Compare VM config (memory, display resolution, OS packages) with working environment.
3. **Hypothesis Testing**: ONE change — different display resolution? More memory? Missing system dependency?
4. **Fix**: At root cause. Don't just "retry the test."

**VM-Specific Diagnostics:**
```bash
vm-desktop exec "ps aux | grep <app-name>"      # Is app running?
vm-desktop exec "lsof -i :3000"                  # Is port in use?
vm-desktop exec "cat /var/log/syslog | tail -20"  # System logs
vm-desktop exec "dmesg | tail -10"                # Kernel messages
```

### 2. Condition-Based Waiting — condition-based-waiting (obra/superpowers)

For GUI automation: NEVER use fixed `sleep()`:
- Wait for window title to appear
- Wait for process to start (`pgrep <app>`)
- Wait for port to be available (`nc -z localhost 3000`)
- Use polling with timeout: check every 500ms, fail after 30s

### 3. Verification — verification-before-completion (obra/superpowers, 16K)
Evidence before claims: screenshot every test step, show `ps` output, show exit codes.

| Skill | Source | Installs | Use |
|-------|--------|----------|-----|
| `agent-browser` | vercel-labs | 89K | If browser opens inside VM |
| `chrome-devtools` | skills.sh | ~5K | Browser debugging inside VM |

---

## VM_TEST_REPORT.md Template

```markdown
# VM_TEST_REPORT: [Feature Name]
> Tested: [date] | Tester: VM Integration Test Runtime
> Environment: [OS, VM type, specs]
> Build: [version/commit/download URL]

## Environment Details
| Property | Value |
|----------|-------|
| OS | [e.g., Ubuntu 22.04 LTS] |
| Kernel | [e.g., 5.15.0-generic] |
| Memory | [e.g., 4 GB] |
| Display | [e.g., 1920x1080] |
| VM Type | [e.g., KVM, VNC, RDP] |

## Installation Test
| Step | Expected | Actual | Status | Screenshot |
|------|----------|--------|--------|------------|
| Download | File downloads | File downloaded (45 MB) | ✅ | download.png |
| Install | Installs without error | Installed successfully | ✅ | install.png |
| Launch | App window appears | Window appeared (2.1s) | ✅ | launch.png |

## Functional Tests
| Test | Expected | Actual | Status | Screenshot |
|------|----------|--------|--------|------------|
| [Test name] | [expected behavior] | [actual behavior] | ✅/❌ | test.png |

## Performance
| Metric | Target | Actual | Status |
|--------|--------|--------|--------|
| Cold start | < 5s | 2.1s | ✅ |
| Memory usage | < 500 MB | 320 MB | ✅ |
| CPU idle | < 5% | 2% | ✅ |

## Cross-Platform (if applicable)
| OS | Install | Launch | Core Features | Status |
|----|---------|--------|---------------|--------|
| Ubuntu 22.04 | ✅ | ✅ | ✅ | ✅ |
| Windows 11 | ✅ | ✅ | ✅ | ✅ |
| macOS 14 | ❌ | - | - | ❌ |

## Verdict
- [ ] ✅ VM PASS — Software runs correctly in real environment
- [ ] ❌ VM FAIL — [Description of failures]
```
