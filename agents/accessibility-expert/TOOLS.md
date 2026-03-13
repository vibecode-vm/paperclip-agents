# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Web Accessibility — web-accessibility (intopia, WCAG)

> Source: [Intopia Web Accessibility](https://intopia.digital)

**Core Principles (POUR):**
- **Perceivable**: Can users perceive ALL content? (alt text, contrast, captions)
- **Operable**: Can users operate ALL controls? (keyboard, touch, voice)
- **Understandable**: Can users understand ALL content and controls? (labels, errors, language)
- **Robust**: Does it work with assistive technology? (ARIA, semantic HTML, screen readers)

### 2. Agent Browser — agent-browser (vercel-labs, 89K)

For testing keyboard navigation and screen reader compatibility:
```bash
# Tab through all interactive elements
agent-browser open <url>
agent-browser keyboard Tab    # Tab to first element
agent-browser keyboard Tab    # Tab to next
agent-browser keyboard Enter  # Activate
agent-browser keyboard Escape # Dismiss

# Check focus visibility
agent-browser screenshot focus-state-<element>.png

# Test responsive/zoom
agent-browser set viewport 320 568  # 320px width (reflow test)
agent-browser screenshot reflow-320.png
```

### 3. Verification — verification-before-completion (obra/superpowers, 16K)

Evidence before claiming "accessible":
- Screenshot every focus state
- Tab through entire page → document the order
- Verify contrast ratios with specific numbers
- Test with `prefers-reduced-motion` active

---

## Testing Commands

```bash
# Check for missing alt text
grep -rn "<img" src/ --include="*.tsx" | grep -v "alt="

# Check for missing labels
grep -rn "<input\|<select\|<textarea" src/ --include="*.tsx" | grep -v "aria-label\|aria-labelledby\|id="

# Check for onClick without keyboard handler
grep -rn "onClick" src/ --include="*.tsx" | grep -v "onKeyDown\|onKeyPress\|button\|Button\|<a "

# Check for color-only indicators
grep -rn "text-red\|text-green\|text-yellow\|bg-red\|bg-green" src/ --include="*.tsx"

# Check heading hierarchy
grep -rn "<h[1-6]" src/ --include="*.tsx" | sort

# Check for role usage
grep -rn "role=" src/ --include="*.tsx"

# Run axe-core (if available)
npx @axe-core/cli <url>
```

---

## Templates

### A11Y_AUDIT.md
```markdown
# A11Y_AUDIT: [Feature / Page]
> Auditor: Accessibility Expert
> Date: [date]
> Standard: WCAG 2.1 AA
> Scope: [components/pages audited]

## Summary
| Category | Pass | Fail | N/A |
|----------|------|------|-----|
| Perceivable | [N] | [N] | [N] |
| Operable | [N] | [N] | [N] |
| Understandable | [N] | [N] | [N] |
| Robust | [N] | [N] | [N] |

## ❌ Failures

### 1. [Title] — WCAG [criterion number]
**Location**: `[component/file:line]`
**Issue**: [describe what's wrong]
**Impact**: [who is affected and how]
**Fix**:
```diff
-inaccessible code
+accessible code
```
**Severity**: 🔴 Critical / 🟡 Important / 🟢 Suggestion

## Keyboard Navigation Map
| Element | Tab Order | Enter | Escape | Arrows | Status |
|---------|-----------|-------|--------|--------|--------|
| [element] | [N] | [action] | [action] | [action] | ✅/❌ |

## Contrast Ratios
| Element | Foreground | Background | Ratio | Required | Status |
|---------|-----------|-----------|-------|----------|--------|
| Body text | [color] | [color] | [X]:1 | 4.5:1 | ✅/❌ |

## Verdict
- [ ] ✅ PASS — WCAG 2.1 AA compliant
- [ ] ❌ FAIL — [N] failures must be fixed
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack
- Shared rules
