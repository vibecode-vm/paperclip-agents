# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. React Performance — vercel-react-best-practices (197K)

> Source: Vercel Labs

**Eliminate waterfalls:**
- `Promise.all()` for independent async operations
- Start promises early, await late
- Use `<Suspense>` to stream content incrementally

**Bundle optimization:**
- Import directly — never barrel files
- `next/dynamic` for heavy components
- Code-split by route (App Router does this automatically)

### 2. Agent Browser — agent-browser (vercel-labs, 89K)

For running Lighthouse and performance profiling:
```bash
agent-browser open <url>
agent-browser profiler start
# ... user interactions ...
agent-browser profiler stop
agent-browser screenshot performance-<page>.png
```

### 3. Verification — verification-before-completion (obra/superpowers, 16K)
Evidence before claiming "optimized":
- Show before/after bundle sizes
- Show before/after Lighthouse scores
- Show before/after Core Web Vitals

---

## Analysis Commands

```bash
# Bundle analysis
npx next build 2>&1 | tee build-output.txt
# Check first-load JS per route

# Package size check
npx bundlecost <package>  # or bundlephobia.com

# Find large imports
grep -rn "import .* from" src/ --include="*.ts" --include="*.tsx" | \
  awk -F"from" '{print $2}' | sort | uniq -c | sort -rn | head -20

# Find barrel file imports
grep -rn "from '\.\./\.\./index'" src/ --include="*.ts" --include="*.tsx"
grep -rn "from '\.\./'" src/ --include="*.ts" --include="*.tsx" | grep -v "\./"

# Image optimization check
find src/ -name "*.png" -o -name "*.jpg" -o -name "*.jpeg" | \
  xargs -I{} ls -lh {} | awk '$5 > "100K" {print}'
```

---

## Templates

### PERFORMANCE_REPORT.md
```markdown
# PERFORMANCE_REPORT: [Feature / Page]
> Analyzer: Performance Optimizer
> Date: [date]
> Scope: [pages/routes analyzed]

## Core Web Vitals
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| LCP | [X]s | ≤ 2.5s | ✅/❌ |
| FID/INP | [X]ms | ≤ 100ms | ✅/❌ |
| CLS | [X] | ≤ 0.1 | ✅/❌ |
| TTFB | [X]ms | ≤ 800ms | ✅/❌ |

## Bundle Size
| Route | First Load JS | Status |
|-------|--------------|--------|
| `/` | [X]KB | ✅/⚠️ |
| `/dashboard` | [X]KB | ✅/⚠️ |

## Top Optimizations (ranked by impact)

### 1. [Title] — Impact: [High/Medium/Low]
**Current**: [what's happening]
**Optimization**: [what to do]
**Expected saving**: [KB / ms / score points]
```diff
-current code
+optimized code
```

## Verdict
- [ ] ✅ PASS — all Core Web Vitals in "Good" range
- [ ] ⚠️ NEEDS WORK — [N] metrics need improvement
- [ ] ❌ FAIL — critical performance issues
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack with versions
- Performance targets
