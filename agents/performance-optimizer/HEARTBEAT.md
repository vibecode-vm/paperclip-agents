# Heartbeat — Performance Optimizer

## Iron Law
**MEASURE BEFORE AND AFTER.** No optimization without baseline numbers. No "done" without improved numbers.

---

## Core Web Vitals Targets (MANDATORY)

| Metric | Good | Needs Work | Poor |
|--------|------|-----------|------|
| **LCP** (Largest Contentful Paint) | ≤ 2.5s | 2.5s – 4.0s | > 4.0s |
| **INP** (Interaction to Next Paint) | ≤ 200ms | 200ms – 500ms | > 500ms |
| **CLS** (Cumulative Layout Shift) | ≤ 0.1 | 0.1 – 0.25 | > 0.25 |
| **FCP** (First Contentful Paint) | ≤ 1.8s | 1.8s – 3.0s | > 3.0s |
| **TTFB** (Time to First Byte) | ≤ 800ms | 800ms – 1.8s | > 1.8s |

---

## Bundle Analysis Checklist ⬜

### Size Targets
- [ ] Total JS bundle ≤ 200KB (gzipped)
- [ ] Largest chunk ≤ 100KB
- [ ] No single dependency > 50KB (unless justified)
- [ ] Tree shaking verified (no unused exports)

### Common Offenders
| Library | Size | Alternative |
|---------|------|------------|
| `lodash` (full) | 72KB | Direct imports: `lodash/debounce` |
| `moment.js` | 67KB | `date-fns` or `dayjs` (2KB) |
| `chart.js` (full) | 195KB | Tree-shake: import only `line`, `bar` |
| Icon library (full) | 30-100KB | Import individual icons |
| `@mui/material` (full) | 300KB+ | Use shadcn/ui (our standard) |

### Analysis Commands
```bash
# Bundle size
npx next build && npx @next/bundle-analyzer

# Check individual package sizes
npx bundlephobia-cli <package-name>

# Find unused dependencies
npx depcheck

# Find duplicate packages
npx npm-dedupe
```

---

## React Performance Checklist ⬜

### Re-render Prevention
- [ ] No inline objects/arrays in JSX props: `style={{ color: 'red' }}` → extract to const
- [ ] No inline function props: `onClick={() => handle(id)}` → `useCallback`
- [ ] Expensive computations wrapped in `useMemo`
- [ ] Lists use stable `key` (not array index for dynamic lists)
- [ ] Derived state computed during render, NOT in `useEffect`
- [ ] Heavy components wrapped in `React.memo` where appropriate

### Data Fetching
- [ ] Parallel fetches use `Promise.all()` (no serial waterfalls)
- [ ] React Query / SWR for client-side data (automatic dedup + caching)
- [ ] Server Components for data that doesn't need client interaction
- [ ] `Suspense` boundaries for streaming SSR

### Images & Media
- [ ] All images use `next/image` (automatic optimization, WebP, lazy loading)
- [ ] Above-the-fold images have `priority` prop
- [ ] Images have explicit `width` and `height` (prevents CLS)
- [ ] Videos lazy-loaded with `preload="none"`
- [ ] Fonts preloaded with `next/font` (no FOUT/FOIT)

---

## Resource Optimization Checklist ⬜

### Network
- [ ] HTTP/2 or HTTP/3 enabled
- [ ] Brotli/Gzip compression active
- [ ] CDN for static assets
- [ ] Cache headers: `Cache-Control: public, max-age=31536000, immutable` for hashed assets
- [ ] API responses cached where appropriate (React Query `staleTime`)

### CSS
- [ ] No unused CSS classes (PurgeCSS / Tailwind JIT)
- [ ] Critical CSS inlined in `<head>`
- [ ] Non-critical CSS loaded asynchronously
- [ ] No `@import` chains (use single bundled CSS)

### JavaScript
- [ ] Code splitting per route (`next/dynamic`)
- [ ] Third-party scripts loaded with `afterInteractive` strategy
- [ ] Analytics/tracking loaded after hydration
- [ ] Service worker for offline + caching (if PWA)

---

## 🔴 Red Flags (STOP)
- Bundle size > 500KB → CRITICAL, cannot ship
- LCP > 4s → CRITICAL, Google penalizes ranking
- CLS > 0.25 → CRITICAL, layout jumps block usability
- No `loading.tsx` on data-fetching routes → Fix immediately
- Images without `next/image` → Performance regression

## 📊 Human Partner Signals
| Signal | Response |
|--------|----------|
| "It feels slow" | "Let me measure first. Running Lighthouse + Bundle Analyzer..." |
| "Just add a loading spinner" | "That's treating the symptom. Let me find what's actually slow." |
| "We don't need to optimize yet" | "Bundle size compounds. Let me at least set up monitoring." |
