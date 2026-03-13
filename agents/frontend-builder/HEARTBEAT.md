# HEARTBEAT.md -- Frontend Builder Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack, MCP servers, shared rules.
- Read `../../MEMORY.md` — known gotchas, architecture decisions.
- shadcn initialized? → `npx shadcn@latest info --json`
- If not → `npx shadcn@latest init`
- All mandatory packages installed? (see SHARED_CONFIG.md stack)
- If not → `npm install <missing>`

## 4. Read System State + Docs (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — know what exists: routes, components, stores, APIs.
2. Read `docs/lib/` — latest library docs (fetched by Stack Researcher via Context7).
3. If `docs/lib/` has docs for libraries you'll use → READ THEM. They have the latest API.

**After finishing work:**
Update `../../SYSTEM_STATE.md` with everything you created/changed.

**Rules for SYSTEM_STATE.md:**
- Create on first task in a new project.
- Update after: adding components, new stores, new routes, dependency changes.
- Include package versions (run `npm list --depth=0` to verify).
- This file is the single source of truth for any agent joining the project.

## 5. Component Development
1. **Search first**: `npx shadcn@latest search` — does a component exist?
2. **Add if found**: `npx shadcn@latest add <component>`
3. **Build if not**: Follow composition patterns (see TOOLS.md)
4. **Always**:
   - Server Components by default
   - `"use client"` only when hooks/interactivity needed
   - Semantic colors, built-in variants
   - `lucide-react` for icons
   - `framer-motion` for animations
   - `sonner` for toasts
   - Compound components over boolean props
   - Single responsibility: one component = one job
   - No business logic in UI components — extract to custom hooks

## 6. State & Data
- Server state → `@tanstack/react-query`
- Client state → `zustand` (with `devtools` + `persist` middleware)
- URL state → `nuqs`
- Forms → `react-hook-form` + `zod`
- HTTP → `ky`
- Charts → `recharts`
- Dates → `date-fns`
- Classes → `clsx`

**State Rules:**
- Immutability: NEVER mutate state directly
- Minimal state: don't store derivable values (use `items.length`, not separate `count`)
- Single source of truth: no duplicate data across stores
- Local state first: only go global when needed
- No props drilling beyond 3 levels → use Context or Zustand

## 7. Performance Check
Apply vercel-react-best-practices:
- [ ] No request waterfalls (Promise.all, Suspense)
- [ ] No barrel imports (direct imports only)
- [ ] Heavy components are lazy-loaded (next/dynamic)
- [ ] Server Components for non-interactive content
- [ ] Minimal data serialized to client
- [ ] No inline objects/functions in JSX props
- [ ] Use `useCallback`/`useMemo` for expensive operations only

## 8. Responsive
- Mobile-first (375px → tablet → desktop)
- Request UI-Tester for cross-viewport validation

## 9. Pre-Delivery Checklist (from ui-ux-pro-max)
- [ ] All icons from lucide-react, no emojis
- [ ] Touch targets ≥ 44×44px
- [ ] Primary text contrast ≥ 4.5:1 (light + dark)
- [ ] `prefers-reduced-motion` respected
- [ ] Keyboard navigation works (tab order = visual order)
- [ ] All form fields have `<label>`, hints, error messages
- [ ] Loading buttons disabled during async (show spinner)
- [ ] `cursor-pointer` on all clickable elements
- [ ] Tested on mobile, tablet, desktop

## 10. Update SYSTEM_STATE.md (MANDATORY)
- Update `../../SYSTEM_STATE.md` with:
  - New routes added
  - New components created (with location and type)
  - New Zustand stores (with persist status)
  - New API endpoints or Server Actions
  - New dependencies installed
  - Known issues discovered
- Update `../../MEMORY.md` if you learned new gotchas or patterns.
- Timestamp the update.

## 11. Exit
- Comment on in_progress work.
- Structured PR: What → Why → How → Screenshots.

## Rules
- ALWAYS latest package versions.
- shadcn first, custom second.
- Never raw color values in Tailwind.
- TypeScript strict mode. Zod at boundaries.
- SYSTEM_STATE.md is always current.
