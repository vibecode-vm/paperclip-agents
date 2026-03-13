# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### Impeccable Design — impeccable/frontend-design (pbakaus)

> Source: [pbakaus/impeccable](https://github.com/pbakaus/impeccable) — "The design language that makes your AI harness better at design."
> Built on Anthropic's frontend-design skill with deeper expertise and more control.

**Before starting, commit to a BOLD aesthetic direction:**
1. **Purpose**: What problem does this interface solve? Who uses it?
2. **Tone**: Pick an EXTREME — brutally minimal, maximalist chaos, retro-futuristic, organic/natural, luxury/refined, playful/toy-like, editorial/magazine, brutalist/raw, art deco/geometric, soft/pastel, industrial/utilitarian
3. **Constraints**: Technical requirements (framework, performance, accessibility)
4. **Differentiation**: What makes this UNFORGETTABLE? What's the ONE thing someone will remember?

**CRITICAL**: Bold maximalism and refined minimalism both work — the key is **intentionality, not intensity**.

---

### 🎨 Typography (Impeccable Reference)

**Choose fonts that are beautiful, unique, and interesting.**

**Avoid the invisible defaults**: Inter, Roboto, Open Sans, Lato, Montserrat, Arial — these make designs feel generic.

**Better Google Fonts alternatives:**
| Instead of | Use |
|-----------|-----|
| Inter | Instrument Sans, Plus Jakarta Sans, Outfit |
| Roboto | Onest, Figtree, Urbanist |
| Open Sans | Source Sans 3, Nunito Sans, DM Sans |
| Editorial/premium | Fraunces, Newsreader, Lora |

**Modular Type Scale** — Use FEWER sizes with MORE contrast (5-size system):

| Role | Ratio | Use Case |
|------|-------|----------|
| xs | 0.75rem | Captions, legal |
| sm | 0.875rem | Secondary UI, metadata |
| base | 1rem | Body text |
| lg | 1.25-1.5rem | Subheadings, lead text |
| xl+ | 2-4rem | Headlines, hero text |

**Fluid Type** — Use `clamp()` for responsive type sizing:
```css
--text-display: clamp(2.5rem, 5vw + 1rem, 4.5rem);
--text-body: clamp(1rem, 0.5vw + 0.875rem, 1.125rem);
```

**Vertical Rhythm**: Line-height (e.g., 24px for 16px/1.5) becomes the base unit for ALL vertical spacing.

**Pairing Principle**: You often don't NEED a second font. One family in multiple weights creates cleaner hierarchy than two competing typefaces.

**DON'T**: Use monospace as lazy "technical/developer" shorthand.
**DON'T**: Put large icons with rounded corners above every heading — they make sites look templated.

---

### 🎨 Color & Theme (Impeccable Reference — OKLCH)

**Stop using HSL. Use OKLCH** — perceptually uniform, meaning equal steps in lightness LOOK equal.

```css
/* OKLCH: lightness (0-100%), chroma (0-0.4+), hue (0-360) */
--color-primary: oklch(60% 0.15 250);
--color-primary-light: oklch(85% 0.08 250);  /* Reduce chroma at extremes */
--color-primary-dark: oklch(35% 0.12 250);
```

**Tinted Neutrals** — Pure gray is dead. Add subtle brand hue:
```css
/* Dead grays → NO */
--gray-100: oklch(95% 0 0);

/* Warm-tinted grays → YES */
--gray-100: oklch(95% 0.01 60);
--gray-900: oklch(15% 0.01 60);
```

**60-30-10 Rule** (visual weight, not pixel count):
- **60%**: Neutral backgrounds, white space, base surfaces
- **30%**: Secondary colors — text, borders, inactive states
- **10%**: Accent — CTAs, highlights, focus states

**Palette Structure**:
| Role | Purpose |
|------|---------|
| Primary | Brand, CTAs, key actions (1 color, 3-5 shades) |
| Neutral | Text, backgrounds, borders (9-11 shade scale, TINTED) |
| Semantic | Success, error, warning, info (4 colors, 2-3 shades each) |
| Surface | Cards, modals, overlays (2-3 elevation levels) |

**Dark Mode is NOT inverted light mode** — reduce chroma, adjust contrast, rethink surfaces.

**DON'T**: Use gray text on colored backgrounds — use a shade of the background color instead.
**DON'T**: Use pure black (#000) or pure white (#fff) — always tint.
**DON'T**: Use the AI color palette: cyan-on-dark, purple-to-blue gradients, neon accents on dark.
**DON'T**: Use gradient text for "impact" — decorative, not meaningful.

---

### 🎨 Layout & Space (Impeccable Reference — 4pt Grid)

**Use 4pt base, not 8pt** — 8pt is too coarse. Scale: 4, 8, 12, 16, 24, 32, 48, 64, 96px.

**Visual Hierarchy — The Squint Test**: Blur your eyes. Can you still identify the most important element, second most important, and clear groupings? If everything looks same weight, you have a hierarchy problem.

**Hierarchy uses MULTIPLE dimensions simultaneously**:
| Tool | Strong | Weak |
|------|--------|------|
| Size | 3:1 ratio+ | <2:1 ratio |
| Weight | Bold vs Regular | Medium vs Regular |
| Color | High contrast | Similar tones |
| Position | Top/left | Bottom/right |
| Space | Surrounded by whitespace | Crowded |

**Use `gap` instead of margins** for sibling spacing — eliminates margin collapse.
**Container queries for components**, viewport queries for pages.

**DON'T**: Wrap everything in cards — spacing creates grouping naturally.
**DON'T**: Nest cards inside cards — visual noise, flatten the hierarchy.
**DON'T**: Use identical card grids — same-sized cards with icon + heading + text, repeated endlessly.
**DON'T**: Center everything — left-aligned with asymmetric layouts feels more designed.
**DON'T**: Use the same spacing everywhere — without rhythm, layouts feel monotonous.

---

### 🎨 Motion Design (Impeccable Reference — 100/300/500 Rule)

| Duration | Use Case |
|----------|----------|
| 100-150ms | Instant feedback (button press, toggle, color) |
| 200-300ms | State changes (menu open, tooltip, hover) |
| 300-500ms | Layout changes (accordion, modal, drawer) |
| 500-800ms | Entrance animations (page load, hero reveals) |

**Exit = 75% of entrance duration.**

**Easing — NEVER use generic `ease`:**
| Curve | Use For | CSS |
|-------|---------|-----|
| ease-out | Elements entering | `cubic-bezier(0.16, 1, 0.3, 1)` |
| ease-in | Elements leaving | `cubic-bezier(0.7, 0, 0.84, 0)` |
| ease-in-out | State toggles | `cubic-bezier(0.65, 0, 0.35, 1)` |

**Exponential curves for micro-interactions:**
```css
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);    /* Smooth, refined (default) */
--ease-out-quint: cubic-bezier(0.22, 1, 0.36, 1);    /* Slightly dramatic */
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);      /* Snappy, confident */
```

**Only animate `transform` and `opacity`** — everything else causes layout recalculation.
**For height animations**: Use `grid-template-rows: 0fr → 1fr` instead of animated height.
**`prefers-reduced-motion`**: Replace spatial motion with crossfade. NOT optional — 35% of adults 40+ affected.

**DON'T**: Use bounce or elastic easing — dated and tacky. Real objects decelerate smoothly.

---

### 🎨 Visual Details (Impeccable Anti-Patterns)

**DON'T**: Use glassmorphism everywhere — blur/glass/glow used decoratively, not purposefully.
**DON'T**: Use rounded elements with thick colored border on one side — lazy accent.
**DON'T**: Use sparklines as decoration — tiny charts that convey nothing meaningful.
**DON'T**: Use rounded rectangles with generic drop shadows — safe, forgettable.
**DON'T**: Use modals unless there's truly no better alternative — modals are lazy.

---

### 🧪 The AI Slop Test (Impeccable — CRITICAL)

> If you showed this interface to someone and said "AI made this," would they believe you immediately? **If yes, that's the problem.**

A distinctive interface makes someone ask "how was this made?" not "which AI made this?"

Review the DON'T guidelines above — they are the **fingerprints of AI-generated work** from 2024-2025.

---

### 🛠️ 17 Impeccable Design Commands

| Command | What it does |
|---------|-------------|
| `/teach-impeccable` | One-time setup: gather design context, save to config |
| `/audit` | Run technical quality checks (a11y, performance, responsive) |
| `/critique` | UX design review: hierarchy, clarity, emotional resonance |
| `/normalize` | Align with design system standards |
| `/polish` | Final pass before shipping |
| `/distill` | Strip to essence |
| `/clarify` | Improve unclear UX copy |
| `/optimize` | Performance improvements |
| `/harden` | Error handling, i18n, edge cases |
| `/animate` | Add purposeful motion |
| `/colorize` | Introduce strategic color |
| `/bolder` | Amplify boring designs |
| `/quieter` | Tone down overly bold designs |
| `/delight` | Add moments of joy |
| `/extract` | Pull into reusable components |
| `/adapt` | Adapt for different devices |
| `/onboard` | Design onboarding flows |

---

### Design System — frontend-design-system (supercent-io, 8K)

**Token Workflow:**
1. Define Design Tokens (OKLCH colors, fluid typography, 4pt spacing, radius, shadows, breakpoints)
2. Define Layout + UX Goals (page type, info architecture, responsive)
3. Apply tokens consistently
4. Run the AI Slop Test
5. Validate accessibility
6. Handoff as DESIGN_SPEC.md

### UI Quality — ui-ux-pro-max (56K)

**Accessibility Requirements (CRITICAL):**
- Color contrast ≥ 4.5:1 normal, ≥ 3:1 large text
- Touch targets ≥ 44×44px
- Keyboard navigation with visible focus rings (2-4px)
- Sequential heading hierarchy
- `prefers-reduced-motion` support
- Color is never the only indicator

### Additional Skills
| Skill | Source | Installs | Use |
|-------|--------|----------|-----|
| `web-design-guidelines` | vercel-labs | 155K | Compliance with Vercel standards |
| `sleek-design-mobile-apps` | sleekdotdesign | 124K | Mobile app design patterns |
| `interface-design` | dammyjay93 | 8K | Interface design principles |
| `canvas-design` | anthropics | 17K | Visual design canvas |
| `brand-guidelines` | anthropics | 12K | Brand consistency |

---

## Pre-Delivery Checklist for Specs

- [ ] Aesthetic direction documented (tone, differentiation, AI Slop Test passed)
- [ ] Design tokens defined (OKLCH colors with tinted neutrals, fluid type, 4pt spacing)
- [ ] Typography: distinctive fonts chosen (no Inter/Roboto/Arial), modular scale applied
- [ ] Color: 60-30-10 rule applied, no pure black/white, neutrals tinted
- [ ] Light AND dark mode specified (NOT just inverted)
- [ ] Responsive behavior for 3 breakpoints (mobile/tablet/desktop)
- [ ] Animation specs with exponential easing (no bounce/elastic)
- [ ] `prefers-reduced-motion` alternative specified
- [ ] Accessibility requirements listed (contrast, touch targets, focus)
- [ ] Component list with ALL states (default, hover, active, disabled, error, loading)
- [ ] No AI slop patterns (no card-in-card, no gradient text, no glassmorphism-everywhere)

---

## DESIGN_SPEC.md Template

```markdown
# DESIGN_SPEC: [Feature Name]

## Aesthetic Direction
- **Tone**: [e.g., Editorial/magazine — clean, spacious, typographic]
- **Reference**: [e.g., "Like Stripe's pricing page meets Linear's dashboard"]
- **Differentiation**: [what makes it memorable — the ONE thing]
- **AI Slop Test**: [what specifically avoids generic AI aesthetics]

## Design Tokens

### Colors (OKLCH)
\`\`\`css
/* Primary */
--color-primary: oklch(60% 0.15 250);
--color-primary-light: oklch(85% 0.08 250);
--color-primary-dark: oklch(35% 0.12 250);

/* Tinted Neutrals (brand-hinted, NOT pure gray) */
--gray-50: oklch(97% 0.005 250);
--gray-100: oklch(95% 0.01 250);
--gray-200: oklch(90% 0.01 250);
--gray-300: oklch(82% 0.01 250);
--gray-400: oklch(70% 0.01 250);
--gray-500: oklch(55% 0.01 250);
--gray-600: oklch(45% 0.01 250);
--gray-700: oklch(35% 0.01 250);
--gray-800: oklch(25% 0.01 250);
--gray-900: oklch(15% 0.01 250);

/* Semantic */
--color-success: oklch(65% 0.15 145);
--color-error: oklch(55% 0.2 25);
--color-warning: oklch(75% 0.15 85);
--color-info: oklch(65% 0.12 250);

/* Dark Mode (NOT just inverted — reduce chroma, adjust contrast) */
--dark-bg: oklch(15% 0.01 250);
--dark-surface: oklch(22% 0.01 250);
--dark-text: oklch(92% 0.005 250);
--dark-muted: oklch(65% 0.01 250);
\`\`\`

### Typography (Fluid, Modular Scale)
| Role | Font | Size (clamp) | Weight | Line Height |
|------|------|-------------|--------|-------------|
| Display | [distinctive font] | clamp(2.5rem, 5vw + 1rem, 4.5rem) | 700 | 1.1 |
| H1 | [display font] | clamp(2rem, 3vw + 1rem, 3rem) | 700 | 1.2 |
| H2 | [display font] | clamp(1.5rem, 2vw + 0.5rem, 2rem) | 600 | 1.3 |
| Body | [body font] | clamp(1rem, 0.5vw + 0.875rem, 1.125rem) | 400 | 1.5 |
| Small | [body font] | 0.875rem | 400 | 1.5 |
| Caption | [body font] | 0.75rem | 500 | 1.4 |

### Spacing (4pt grid)
4, 8, 12, 16, 24, 32, 48, 64, 96px
Named: `--space-xs` (4), `--space-sm` (8), `--space-md` (16), `--space-lg` (24), `--space-xl` (32), `--space-2xl` (48), `--space-3xl` (64)

### Shadows (subtle — if you can clearly see it, it's too strong)
| Level | Value |
|-------|-------|
| sm | 0 1px 2px oklch(0% 0 0 / 0.04) |
| md | 0 4px 6px -1px oklch(0% 0 0 / 0.08) |
| lg | 0 10px 15px -3px oklch(0% 0 0 / 0.08) |

### Motion (100/300/500 rule)
| Trigger | Duration | Easing | Description |
|---------|----------|--------|-------------|
| Page load | 500-800ms | `--ease-out-expo` | Staggered reveals, 50ms delay between elements |
| Hover | 100-150ms | `--ease-out-quart` | Scale 1.02, shadow elevation |
| Modal open | 200-300ms | `--ease-out-expo` | Fade + scale from 0.95 |
| Accordion | 300ms | `--ease-out-quart` | grid-template-rows transition |
| Exit (any) | 75% of enter | `--ease-in` | Faster out than in |

\`\`\`css
/* Easing tokens */
--ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);
--ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
--ease-in: cubic-bezier(0.7, 0, 0.84, 0);

/* Reduced motion */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
\`\`\`

## Layout
### Desktop (≥1024px)
[describe layout, grid, sidebar — use named grid areas]

### Tablet (768-1023px)
[describe layout — use container queries for components]

### Mobile (<768px)
[describe mobile — adapt, don't amputate functionality]

## Components
### [Component Name]
- **States**: default, hover, active, focus, disabled, loading, error
- **Variants**: primary, secondary, outline, ghost
- **Sizes**: sm (32px), md (40px), lg (48px)
- **Accessibility**: `aria-label`, keyboard shortcut, focus ring (2px solid primary, 2px offset)

## AI Slop Avoidance Checklist
- [ ] No Inter/Roboto/Arial fonts
- [ ] No pure black/white — tinted neutrals only
- [ ] No gray text on colored backgrounds
- [ ] No card-in-card nesting
- [ ] No identical card grids
- [ ] No glassmorphism-everywhere
- [ ] No bounce/elastic easing
- [ ] No gradient text for "impact"
- [ ] No cyan-on-dark AI color palette
- [ ] Passes the "could an AI have made this?" test with NO
```
