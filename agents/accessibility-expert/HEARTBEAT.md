# Heartbeat — Accessibility Expert

## WCAG 2.1 AA Audit Checklist

### 1. Perceivable ⬜

#### 1.1 Text Alternatives
- [ ] All `<img>` have meaningful `alt` text (not "image" or "photo")
- [ ] Decorative images have `alt=""` and `role="presentation"`
- [ ] Icon-only buttons have `aria-label`
- [ ] SVG icons have `<title>` or `aria-label`

#### 1.3 Adaptable
- [ ] Semantic HTML: `<nav>`, `<main>`, `<article>`, `<aside>`, `<header>`, `<footer>`
- [ ] Heading hierarchy: single `<h1>`, sequential `<h2>`→`<h3>` (no skips)
- [ ] Lists use `<ul>`/`<ol>`/`<li>`, not styled `<div>` elements
- [ ] Tables have `<caption>`, `<th>` with `scope`

#### 1.4 Distinguishable
- [ ] Normal text: contrast ratio ≥ 4.5:1
- [ ] Large text (18px+ bold, 24px+): contrast ratio ≥ 3:1
- [ ] UI components and borders: contrast ratio ≥ 3:1
- [ ] No info conveyed by color alone (add icon/text)
- [ ] Text resizable to 200% without loss of functionality
- [ ] Content reflows at 320px width (no horizontal scroll)

### 2. Operable ⬜

#### 2.1 Keyboard Accessible
- [ ] ALL interactive elements reachable via Tab
- [ ] Tab order matches visual reading order
- [ ] Custom components support Enter, Space, Escape, Arrow keys as appropriate
- [ ] No keyboard traps (can always Tab/Escape out)
- [ ] Skip-to-main-content link present
- [ ] Visible focus indicator: 2-4px outline on all focus states

#### 2.3 Seizures and Physical Reactions
- [ ] No flashing content (> 3 flashes/second)
- [ ] `prefers-reduced-motion: reduce` → disable/reduce animations
- [ ] Respect `prefers-contrast: high` for high-contrast mode

#### 2.4 Navigable
- [ ] Descriptive page `<title>` on every page
- [ ] Focus managed on route change (focus on main content or heading)
- [ ] Links have descriptive text (not "click here")
- [ ] Breadcrumbs on multi-level pages

### 3. Understandable ⬜

#### 3.1 Readable
- [ ] `lang` attribute on `<html>`
- [ ] Error messages explain WHAT went wrong and HOW to fix

#### 3.2 Predictable
- [ ] No auto-submit on input change (explicit submit button)
- [ ] Consistent navigation across pages
- [ ] No unexpected context changes (auto-redirect, auto-play)

#### 3.3 Input Assistance
- [ ] All `<input>` have associated `<label>` (via `htmlFor`/`id`)
- [ ] Required fields marked with `aria-required="true"` and visual indicator
- [ ] Error messages appear next to the field, not just at top
- [ ] `autocomplete` attribute on address/payment/login fields

### 4. Robust ⬜

#### 4.1 Compatible
- [ ] Valid HTML (no duplicate IDs)
- [ ] ARIA roles used correctly (not just sprinkled randomly)
- [ ] Custom widgets follow ARIA Authoring Practices Guide patterns
- [ ] Tested with VoiceOver (macOS) or NVDA (Windows)
