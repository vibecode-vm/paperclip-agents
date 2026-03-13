# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Pre-Build: Documentation References

Before writing code, check `docs/lib/` for the latest library documentation (fetched by the Technical Research & Stack Validation Agent via Context7). These docs contain **current API signatures** — use them instead of training data.

```bash
# Check if docs exist for this feature's libraries
ls docs/lib/

# If missing, request from Research Agent via Orchestrator:
# "Need Context7 docs for [next.js server actions, zustand persist, etc.]"
```

⚠️ **If `docs/lib/` has documentation for a library you're using → READ IT FIRST.** It contains the latest API, not your training data.

---

## shadcn/ui CLI (ALWAYS use before custom components)
```bash
npx shadcn@latest init                    # Initialize
npx shadcn@latest add button card dialog  # Add components
npx shadcn@latest search                  # Search all registries
npx shadcn@latest info --json             # Check project config
npx shadcn@latest docs <component>        # Component docs + examples
npx shadcn@latest diff                    # Check for updates
```

### shadcn Rules
- **Search before building.** `npx shadcn@latest search` → only build custom if nothing exists.
- **Compose**: Dashboard = `Sidebar + Card + Chart + Table`. Settings = `Tabs + Card + Form`.
- **Variants first**: `variant="outline"`, `size="sm"` — use built-in before custom.
- **Semantic colors**: `bg-primary`, `text-muted-foreground` — **NEVER** `bg-blue-500`.
- **Icons**: `lucide-react` only. Import directly: `import { Search } from "lucide-react"`.
- **No emojis as icons.** Always SVG from a consistent icon family.

---

## Skills (Combined Best Practices)

### 1. React Performance — vercel-react-best-practices (197K)

**CRITICAL: Eliminate Waterfalls**
- `Promise.all()` for independent async operations
- Defer `await` into branches where actually used
- Use `<Suspense>` to stream content
- Start promises early, await late in API routes

**CRITICAL: Bundle Size**
- Import directly — avoid barrel files
- `next/dynamic` for heavy components
- Load analytics/logging after hydration
- Preload on hover/focus for perceived speed

**HIGH: Server Performance**
- `React.cache()` for per-request deduplication
- LRU cache for cross-request caching
- Minimize data serialized to client components
- `after()` for non-blocking operations
- Parallelize fetches by restructuring components

**MEDIUM: Re-render Optimization**
- Derive state during render, not in effects
- Functional `setState` for stable callbacks
- `useRef` for transient frequent values (scroll position, timers)
- `startTransition` for non-urgent updates
- Extract expensive work into memoized components
- Hoist default non-primitive props

---

### 2. Design Aesthetics — frontend-design (144K, Anthropic) + Impeccable (pbakaus)

> Sources: [Anthropic frontend-design](https://github.com/anthropics/skills), [pbakaus/impeccable](https://github.com/pbakaus/impeccable)

**Before coding, commit to a BOLD aesthetic direction:**

1. **Purpose**: What problem does this interface solve? Who uses it?
2. **Tone**: Pick a DISTINCTIVE aesthetic — brutally minimal, luxury/refined, editorial/magazine, retro-futuristic, organic/natural, playful, brutalist, art deco, soft/pastel...
3. **Differentiation**: What makes this UNFORGETTABLE?

**Color (Impeccable — OKLCH):**
- Use `oklch()` NOT hex/HSL — perceptually uniform, consistent brightness across hues
- **Tinted neutrals**: No pure `#000`/`#fff`/gray — always add warmth (`oklch(0.15 0.01 250)`) or cool tint
- **60-30-10 Rule**: 60% dominant, 30% secondary, 10% accent
- Dark mode: shift Lightness ≤ 0.25 and Chroma ≤ 0.04 for surfaces

**Typography (Impeccable — Fluid):**
- NEVER use Inter, Roboto, Arial, Open Sans, system defaults
- Fluid sizing: `font-size: clamp(1rem, 0.5rem + 1.5vw, 1.5rem)`
- Modular scale (1.25 ratio): 1rem → 1.25 → 1.563 → 1.953 → 2.441
- Pair: display font + refined body (e.g., Playfair Display + Source Serif)

**Spacing (Impeccable — 4pt Grid):**
- Base unit: 4px. All spacing = multiples of 4
- Tokens: `--space-xs: 4px`, `--space-sm: 8px`, `--space-md: 16px`, `--space-lg: 24px`, `--space-xl: 32px`

**Motion (Impeccable — Intentional):**
- Micro: 100ms, Menu/Tooltip: 300ms, Page transitions: 500ms
- ONLY exponential easing: `cubic-bezier(0.16, 1, 0.3, 1)` (ease-out-expo)
- NEVER use generic `ease`, `bounce`, or `elastic`
- Only animate `transform` and `opacity` (GPU composited)
- ALWAYS respect `prefers-reduced-motion`

**AI Slop Test (Impeccable):**
> "If someone said 'AI made this,' would they believe it? If yes, that's the problem."
- No card-in-card nesting, no identical card grids, no glassmorphism-everywhere
- No cyan-on-dark, purple-to-blue gradients, neon accents
- No rounded rectangles with generic drop shadows
- No gradient text for "impact"

**Spatial Composition**: Unexpected layouts. Asymmetry. Overlap. Grid-breaking elements.
**Backgrounds**: Gradient meshes, noise textures, geometric patterns, grain overlays. Never plain solid colors.

---

### 3. UI/UX Quality — ui-ux-pro-max (56K)

#### Accessibility (CRITICAL)
- Color contrast: minimum 4.5:1 for normal text, 3:1 for large text
- Visible focus rings (2-4px) on all interactive elements
- `aria-label` for icon-only buttons
- Tab order matches visual order; full keyboard support
- `<label>` with `for` attribute on all form fields
- Skip-to-main-content link
- Sequential heading hierarchy (h1→h6, no level skip)
- Don't convey info by color alone — add icon/text
- Respect `prefers-reduced-motion` — disable/reduce animations

#### Touch & Interaction (CRITICAL)
- Touch targets: minimum 44×44px
- Minimum 8px gap between touch targets
- Don't rely on hover alone — use click/tap for primary interactions
- Disable button during async operations; show spinner
- `cursor-pointer` on all clickable elements
- `touch-action: manipulation` to remove 300ms tap delay

#### Pre-Delivery Checklist
- [ ] All icons from consistent icon family (lucide-react), no emojis
- [ ] All tappable elements have pressed feedback
- [ ] Touch targets ≥ 44×44px
- [ ] Micro-animation timing: 150-300ms with native-feeling easing
- [ ] Disabled states visually clear and non-interactive
- [ ] Primary text contrast ≥ 4.5:1 in light AND dark mode
- [ ] Secondary text contrast ≥ 3:1 in both modes
- [ ] Tested on mobile, tablet, and desktop
- [ ] 4/8px spacing rhythm maintained
- [ ] All form fields have labels, hints, and error messages
- [ ] Color is not the only indicator
- [ ] `prefers-reduced-motion` supported

---

### 4. Web Compliance — web-design-guidelines (155K, Vercel)

Fetches latest guidelines from:
```
https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
```
Run before delivery to check all files against Vercel's web interface standards.

---

### 5. Design System Architecture — frontend-design-system (supercent-io)

**Token Workflow:**
1. **Define Design Tokens**: Colors (primary scale 50-900), typography (font family, sizes, weights), spacing (8px base), border radius, shadows, breakpoints
2. **Define Layout + UX Goals**: Page type (landing/dashboard/form), information architecture with priority, responsive behavior
3. **Generate UI**: Apply tokens consistently across components
4. **Validate Accessibility**: Contrast, keyboard, screen reader
5. **Handoff**: Design tokens file + component specs

**Best Practices:**
- Start with content hierarchy — UI follows content priority
- 8px spacing system — no ad-hoc spacing
- Motion with intent — only animate meaningful transitions
- Test on mobile first
- Accessibility from design phase, not after

---

### 6. shadcn Component Patterns — shadcn/ui (11K)

- Use `npx shadcn@latest docs <component>` for usage examples
- Form pattern: `<Form>` + `<FormField>` + `<FormItem>` + `<FormControl>` + `<FormMessage>`
- Always use Radix primitives under the hood (Dialog, Popover, Select)
- Styling via `cn()` utility (clsx + tailwind-merge)
- Override via CSS variables in `globals.css`, not component props

---

### 7. TDD for Components — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: Write the component test BEFORE writing the component.**
Wrote a component before its test? Delete it. Start fresh from the test.

**Component TDD Cycle:**
1. 🔴 Write test: `render(<Component />)` → assert expected behavior
2. 🔴 Run → MUST FAIL
3. 🟢 Write MINIMAL component to pass
4. 🟢 Run ALL tests → PASS
5. 🔵 Refactor (extract hooks, improve names)
6. ↻ Repeat

### 8. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: NO FIXES WITHOUT ROOT CAUSE FIRST.**
1. Root Cause Investigation → 2. Pattern Analysis → 3. Hypothesis Testing → 4. Implementation

### 9. Git Worktrees — using-git-worktrees (obra/superpowers, 14K)

Isolate feature work on a new branch with clean worktree:
```bash
git worktree add ../feature-<name> -b feature/<name>
cd ../feature-<name>
npm ci && npm test  # verify clean baseline
```

### 10. Additional Skills

| Skill | Source | Installs | Use |
|-------|--------|----------|-----|
| `vercel-composition-patterns` | vercel-labs | 79K | Server/Client component boundaries |
| `next-best-practices` | vercel-labs | 31K | App Router patterns |
| `context7` | am-will | 12K | Fetch latest library docs (use via `docs/lib/`) |
| `tailwind-design-system` | wshobson | 17K | Tailwind token architecture |
| `react:components` | google-labs-code | 14K | Component patterns |
| `ui-component-patterns` | supercent-io | 10K | Component architecture |
| `state-management` | supercent-io | 10K | Zustand, context patterns |
| `responsive-design` | supercent-io | 10K | Responsive patterns + breakpoints |
| `shadcn` | shadcn/ui | 12K | shadcn/ui best practices |
| `web-accessibility` | intopia | — | WCAG compliance, focus management |
| `verification-before-completion` | obra | 16K | Proof before done |
| `receiving-code-review` | obra | 16K | Act on review feedback |
| `finishing-a-development-branch` | obra | 14K | Clean branch completion |

---

### 8. Component Architecture — vercel-composition-patterns (79K) + ui-component-patterns (10K)

#### Architecture Rules (HIGH)
- **No boolean props** for behavior customization → use composition instead
  ```tsx
  // ❌ <Card isCompact isBordered hasHeader />
  // ✅ <Card><Card.Header>...</Card.Header><Card.Body>...</Card.Body></Card>
  ```
- **Compound components** for complex UI → shared context between parts
- **Explicit variant components** over boolean modes
  ```tsx
  // ❌ <Button isPrimary isLarge />
  // ✅ <Button variant="primary" size="lg" />
  ```
- **Children over render props** → use `children` for composition
- **React 19+**: No `forwardRef` → use `ref` as prop; use `use()` instead of `useContext()`

#### Component Constraints (MUST)
1. **Single Responsibility**: One component = one job. Button handles buttons, Form handles forms.
2. **TypeScript interfaces required** on all component props.
3. **No business logic in UI components** — extract API calls, calculations to custom hooks.
4. **No props drilling beyond 3 levels** → use Context or Zustand.
5. **No inline objects/functions in JSX:**
   ```tsx
   // ❌ <Component style={{ color: 'red' }} onClick={() => handle()} />
   // ✅ const style = useMemo(() => ({ color: 'red' }), [])
   //    <Component style={style} onClick={handleClick} />
   ```

#### State Architecture (from state-management skill)
- **Immutability**: Never mutate state directly → spread operator
- **Minimal state**: Don't store derivable values (`items.length` not separate `count`)
- **Single source of truth**: No duplicate data across stores
- **Zustand best practices**: Always use `devtools` + `persist` middleware:
  ```tsx
  create<Store>()(devtools(persist((set, get) => ({ ... }), { name: 'key' })))
  ```

#### File Structure per Component
```
components/
├── ui/           # shadcn components (untouched originals)
├── feature-name/
│   ├── FeatureName.tsx        # Main component
│   ├── FeatureName.types.ts   # TypeScript interfaces
│   ├── useFeatureName.ts      # Custom hook (logic)
│   └── FeatureName.test.tsx   # Tests
```

---

## Package Patterns

### State Management
```tsx
// Server State → @tanstack/react-query
const { data } = useQuery({ queryKey: ['users'], queryFn: fetchUsers })

// Client State → zustand (with persist)
import { create } from 'zustand'
import { persist } from 'zustand/middleware'
const useStore = create(persist((set) => ({
  count: 0,
  inc: () => set((s) => ({ count: s.count + 1 })),
}), { name: 'app-store' }))

// URL State → nuqs
import { useQueryState } from 'nuqs'
const [search, setSearch] = useQueryState('q')
```

### Forms
```tsx
import { useForm } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import { z } from 'zod'

const schema = z.object({
  email: z.string().email(),
  name: z.string().min(2),
})
const form = useForm({ resolver: zodResolver(schema) })
```

### HTTP
```tsx
import ky from 'ky'
const api = ky.create({ prefixUrl: '/api', retry: 2 })
const users = await api.get('users').json()
```

### Conditional Classes
```tsx
import { clsx } from 'clsx'
<div className={clsx('base', isActive && 'bg-primary', size === 'lg' && 'p-6')} />
```

### Animation
```tsx
import { motion, AnimatePresence } from 'framer-motion'
<AnimatePresence>
  {isOpen && (
    <motion.div
      initial={{ opacity: 0, height: 0 }}
      animate={{ opacity: 1, height: 'auto' }}
      exit={{ opacity: 0, height: 0 }}
      transition={{ duration: 0.2, ease: 'easeInOut' }}
    />
  )}
</AnimatePresence>
```

### Toast Notifications
```tsx
import { toast } from 'sonner'
toast.success('Saved successfully')
toast.error('Something went wrong')
toast.promise(saveData(), { loading: 'Saving...', success: 'Done!', error: 'Failed' })
```
