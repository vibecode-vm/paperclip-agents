# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Security Best Practices — security-best-practices (supercent-io, 11K)
Auth patterns, CSRF protection, XSS prevention, secure coding patterns.

### 2. Trail of Bits Security — trail-of-bits-security (skills.sh, 12K)
Vulnerability detection, penetration testing patterns, OWASP coverage.

### 3. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)
> Source: [obra/superpowers](https://github.com/obra/superpowers)

For tracing security vulnerabilities: root cause investigation at each component boundary.

### 4. Verification — verification-before-completion (obra/superpowers, 16K)
Evidence before claiming "secure":
- Show `npm audit` output
- Show header check output
- Show test results for auth bypass attempts

---

## Scan Commands

```bash
# Dependency vulnerabilities
npm audit
npm audit --production  # production deps only

# Search for hardcoded secrets
grep -rn "password\|secret\|api.key\|token" src/ --include="*.ts" --include="*.env*" -i

# Search for dangerous patterns
grep -rn "eval(\|innerHTML\|dangerouslySetInnerHTML\|new Function" src/ --include="*.ts" --include="*.tsx"

# Check for unparameterized queries
grep -rn "sql\`\|\.raw(" src/ --include="*.ts"

# Check security headers
curl -I https://<domain> 2>/dev/null | grep -i "strict-transport\|content-security\|x-frame\|x-content-type"

# Check CORS
curl -H "Origin: https://evil.com" -I https://<domain>/api/health 2>/dev/null | grep -i "access-control"
```

---

## Templates

### SECURITY_AUDIT.md
```markdown
# SECURITY_AUDIT: [Project / Feature]
> Auditor: Security Auditor
> Date: [date]
> Scope: [files/endpoints audited]

## Summary
| Severity | Count |
|----------|-------|
| 🔴 Critical | [N] |
| 🟠 High | [N] |
| 🟡 Medium | [N] |
| 🟢 Low | [N] |

## 🔴 Critical Findings

### 1. [Title] — [OWASP Category]
**Location**: `[file:line]`
**Risk**: [describe the attack vector]
**Exploit**: [how to exploit this]
**Fix**:
```diff
-vulnerable code
+secure code
```

## 🟠 High Findings
### 1. [Title] — [OWASP Category]
...

## ✅ Passed Checks
- [list of things that passed audit]

## Dependency Report
```
npm audit output here
```

## Verdict
- [ ] 🔴 BLOCK — critical vulnerabilities must be fixed before deploy
- [ ] 🟡 CONDITIONAL — high issues should be fixed, deploy at own risk
- [ ] ✅ PASS — no critical/high issues found
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack
- Shared rules
