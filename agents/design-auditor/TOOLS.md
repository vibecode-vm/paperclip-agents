# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Primary: agent-browser (vercel-labs, 89K)

You use agent-browser to inspect live builds with evidence.

### Core Workflow
```bash
# 1. Open the page
agent-browser open <url>

# 2. Snapshot for structure review
agent-browser snapshot -i

# 3. Capture evidence
agent-browser screenshot evidence-desktop.png
agent-browser screenshot --full evidence-full.png
agent-browser screenshot --annotate evidence-annotated.png

# 4. Check responsive
agent-browser set viewport 375 812   # Mobile
agent-browser screenshot evidence-mobile.png
agent-browser set viewport 768 1024  # Tablet
agent-browser screenshot evidence-tablet.png
agent-browser set viewport 1440 900  # Desktop
agent-browser screenshot evidence-desktop.png

# 5. Check dark mode
agent-browser --color-scheme dark open <url>
agent-browser screenshot evidence-dark.png

# 6. Visual diff against baseline
agent-browser diff screenshot --baseline baseline.png

# 7. Cleanup
agent-browser close
```

### Inspection Commands
```bash
agent-browser get text @e1                    # Read element text
agent-browser snapshot -i -C                  # All interactive elements
agent-browser highlight @e1                   # Highlight element
agent-browser profiler start                  # Performance check
```

---

## Skills

### Design Review
- **web-design-guidelines** (vercel-labs, 155K) — Fetch + check against Vercel standards
- **web-design-reviewer** (skills.sh, 8K) — Design review checklist
- **ui-ux-pro-max** (nextlevelbuilder, 56K) — Pre-delivery checklist

### Audit
- **audit-website** (squirrelscan, 34K) — Security + UX audit
- **seo-audit** (coreyhaines31, 39K) — SEO compliance check

### Code Quality
- **code-review-excellence** (wshobson, 7K) — Review standards
- **verification-before-completion** (obra, 16K) — Evidence gate

---

## Review Checklist (from ui-ux-pro-max + web-design-guidelines)

### 1. Visual Quality
- [ ] Icons consistent (lucide-react family, no emojis)
- [ ] Pressed/hover states don't shift layout
- [ ] Semantic tokens used (no hardcoded colors)
- [ ] Typography scale matches DESIGN_SPEC
- [ ] Spacing follows 4/8px grid
- [ ] Shadows and radius consistent

### 2. Accessibility (ALL are 🔴 Blockers)
- [ ] Color contrast ≥ 4.5:1 normal text, ≥ 3:1 large text
- [ ] Visible focus rings on interactive elements
- [ ] `aria-label` on icon-only buttons
- [ ] Tab order = visual order
- [ ] All form fields have `<label>` + error messages
- [ ] Skip-to-main-content link
- [ ] Heading hierarchy h1 → h2 → h3 (no skip)
- [ ] Color is not the only indicator
- [ ] `prefers-reduced-motion` supported

### 3. Responsive (3 Viewports)
- [ ] Mobile (375px): no horizontal scroll, touch targets ≥ 44px
- [ ] Tablet (768px): layout adapts correctly
- [ ] Desktop (1440px): content doesn't stretch edge-to-edge

### 4. Dark Mode
- [ ] Both themes tested
- [ ] Contrast meets requirements in dark mode too
- [ ] Borders/dividers distinguishable
- [ ] Modal scrim opacity 40-60%

### 5. UX Patterns
- [ ] Loading states present (skeleton/spinner)
- [ ] Error states present (clear message + recovery action)
- [ ] Empty states present (helpful message + CTA)
- [ ] Long text handled (truncation with tooltip OR wrap)
- [ ] Forms: validation on blur, submit loading, success feedback

### 6. Design Compliance
- [ ] Matches DESIGN_SPEC.md aesthetic direction
- [ ] Colors match token values exactly
- [ ] Font families match spec
- [ ] Spacing matches spec values
- [ ] Animation timing/easing matches spec

### 7. Performance (Surface)
- [ ] No layout shift on load (CLS)
- [ ] Images lazy-loaded
- [ ] Heavy components dynamically imported

### 8. AI Slop Test (Impeccable — pbakaus)

> Source: [pbakaus/impeccable](https://github.com/pbakaus/impeccable)
> "If you showed this to someone and said 'AI made this,' would they believe you immediately? If yes, that's the problem."

**Anti-AI-Slop Checklist:**
- [ ] No overused fonts (Inter, Roboto, Arial, Open Sans, system defaults)
- [ ] No pure black (#000) or pure white (#fff) — always tinted
- [ ] No gray text on colored backgrounds — use a shade of the bg color
- [ ] No card-in-card nesting — flatten hierarchy with spacing/typography
- [ ] No identical card grids (same icon + heading + text, repeated endlessly)
- [ ] No glassmorphism-everywhere (blur/glass/glow used decoratively)
- [ ] No bounce or elastic easing — dated, tacky
- [ ] No gradient text for "impact" — decorative, not meaningful
- [ ] No cyan-on-dark, purple-to-blue gradients, neon accents on dark
- [ ] No monospace as lazy "technical/developer" shorthand
- [ ] No large icons with rounded corners above every heading (templated)
- [ ] No rounded rectangles with generic drop shadows — safe, forgettable
- [ ] No modals where a better alternative exists
- [ ] Motion uses exponential easing (`ease-out-quart/quint/expo`), NOT generic `ease`
- [ ] Color uses OKLCH with tinted neutrals, NOT hex/HSL with pure grays
- [ ] Typography uses fluid clamp() sizing with modular scale

---

## REVIEW.md Template

```markdown
# REVIEW: [Feature Name]
> Reviewed: [date] | Reviewer: UI/UX Critic
> Build URL: [url]
> Compared against: PRD + DESIGN_SPEC.md

## Summary
[X] findings: [N] 🔴 Blockers, [N] 🟡 Major, [N] 🟢 Minor

## 🔴 Blockers (Must Fix)

### 1. [Title]
- **Category**: Accessibility / Visual / Responsive / UX
- **What's wrong**: [describe]
- **Expected**: [what it should be, reference spec]
- **Evidence**: ![screenshot](path/to/screenshot.png)

## 🟡 Major (Should Fix)

### 1. [Title]
- ...

## 🟢 Minor (Nice to Fix)

### 1. [Title]
- ...

## Screenshots
| Viewport | Light | Dark |
|----------|-------|------|
| Mobile (375px) | ![](mobile-light.png) | ![](mobile-dark.png) |
| Tablet (768px) | ![](tablet-light.png) | ![](tablet-dark.png) |
| Desktop (1440px) | ![](desktop-light.png) | ![](desktop-dark.png) |

## Verdict
- [ ] ✅ APPROVED — ship it
- [ ] 🔄 NEEDS FIXES — [N] blockers must be resolved
```
