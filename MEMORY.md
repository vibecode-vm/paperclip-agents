# MEMORY.md — Gelernte Patterns & Entscheidungen

> Curated Knowledge Base für alle Agents.
> Wird nach jedem Feature-Zyklus vom Docs Manager aktualisiert.
> Agents sollen diese Datei lesen um bekannte Patterns und Gotchas zu kennen.

---

## Architektur-Entscheidungen

| Entscheidung | Warum | Datum | Feature |
|---|---|---|---|
| | | | |

## Stack-Spezifische Gotchas

### Next.js 15
- `getServerSideProps` ist deprecated → Server Components nutzen
- App Router: `layout.tsx` ist shared, `page.tsx` ist unique
- Server Actions: `"use server"` am Anfang der Datei

### React 19
- `forwardRef` nicht mehr nötig → `ref` als normaler Prop
- `use()` statt `useContext()` für Context
- Server Components sind default → `"use client"` nur wenn nötig

### Tailwind CSS 4
- `@theme` statt `tailwind.config.js`
- CSS Custom Properties native
- `@import "tailwindcss"` statt `@tailwind base/components/utilities`

### Zustand
- Persist: Key muss unique sein pro Store
- Immer `devtools` + `persist` Middleware nutzen
- Kein `set({ ...state })` → `set((s) => ({ count: s.count + 1 }))`

### shadcn/ui
- `onOpenChange` statt `onClose` bei Dialog/Sheet/Popover
- `cn()` für conditional classes (clsx + tailwind-merge)
- Override via CSS Variablen in `globals.css`, NICHT Component Props

## Gelernte Patterns

| Pattern | Lösung | Anwendung |
|---------|--------|-----------|
| | | |

## Was NICHT funktioniert hat

| Was versucht | Warum gescheitert | Bessere Alternative |
|---|---|---|
| | | |
