# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. UI/UX Pro Max — ui-ux-pro-max (nextlevelbuilder, 56K)

**Comprehensive UX quality checklist:**
- User flow completeness (entry → goal → exit)
- Error handling UX (clear messages, recovery paths)
- Loading state design (skeleton, spinner, progress)
- Empty state design (helpful, actionable)
- Form UX (validation, auto-save, smart defaults)
- Navigation patterns (breadcrumbs, back, search)
- Micro-interactions (feedback, transitions)

### 2. Web Design Guidelines — web-design-guidelines (vercel-labs, 155K)

**Vercel's standards for web UX:**
- Mobile-first design principles
- Touch target sizing (min 44×44px)
- Readable typography (min 16px body, 1.5 line-height)
- Contrast ratios (WCAG AA: 4.5:1 text, 3:1 large text)
- Responsive breakpoints and behavior
- Performance perception (optimistic UI, skeleton screens)

### 3. Agent Browser — agent-browser (vercel-labs, 89K)

**For usability testing of live builds:**
```bash
# Test user flows
agent-browser open <build-url>
agent-browser snapshot -i     # List all interactive elements
agent-browser snapshot -i -C  # Check focus/tab order

# Test on mobile
agent-browser set viewport 375 812
agent-browser screenshot mobile-flow.png

# Check accessibility
agent-browser a11y audit
agent-browser a11y tree
```

### 4. Audit Website — audit-website (squirrelscan, 34K)

**Structured website audit covering:**
- Navigation and information architecture
- User flow friction points
- Accessibility compliance
- Mobile usability
- Performance perception

### 5. Frontend Design — frontend-design (vercel-labs, 144K)

**Design system awareness:**
- Component composition patterns
- Spacing and layout systems
- Color semantics (success, warning, error, info)
- Typography hierarchy
- Icon usage guidelines

### 6. Impeccable UX Patterns — impeccable/frontend-design (pbakaus)

> Source: [pbakaus/impeccable](https://github.com/pbakaus/impeccable)

**Interaction Design (Impeccable):**
- **Optimistic UI**: Update immediately, sync later. Instagram likes work offline — UI updates instantly.
- **Progressive disclosure**: Start simple, reveal sophistication through interaction. Basic options first, advanced behind expandable sections.
- **Empty states teach**: Design empty states that teach the interface, not just say "nothing here."
- **Button hierarchy**: Not every button is primary — use ghost, text links, secondary styles. Hierarchy matters.
- **DON'T**: Repeat information — redundant headers, intros that restate the heading.

**UX Writing (Impeccable):**
- Make every word earn its place.
- Buttons: verb + noun ("Create Project", not "Submit").
- Error messages: what went wrong + how to fix it.
- Empty states: guiding, not dead-ends.
- DON'T: Repeat information users can already see.

**Responsive Design (Impeccable):**
- **Container queries** (`@container`) for component-level responsiveness — components adapt to their container, not the viewport.
- Adapt the interface for different contexts — DON'T just shrink it.
- DON'T hide critical functionality on mobile — adapt, don't amputate.

### 7. Verification — verification-before-completion (obra, 16K)

- Evidence before claims. Screenshot every usability issue found.
- Reference specific heuristic violations with number.

---

## Nielsen's 10 Heuristics — Evaluation Framework

Use this framework for every `USABILITY_REVIEW.md`:

| # | Heuristic | What to Check | Severity |
|---|-----------|---------------|----------|
| H1 | Visibility of system status | Loading indicators, progress bars, save confirmations, button states, real-time feedback | 🔴 if missing |
| H2 | Match between system & real world | User-friendly language, familiar icons, logical flow matching mental models, no jargon | 🟡 if violated |
| H3 | User control & freedom | Undo, back button, cancel actions, close modals, clear form, dismiss notifications | 🔴 if missing |
| H4 | Consistency & standards | Same components same behavior, platform conventions, consistent terminology | 🟡 if violated |
| H5 | Error prevention | Confirmation for destructive actions, smart defaults, input constraints, disabled invalid options | 🔴 for destructive |
| H6 | Recognition over recall | Visible labels, recent items, search suggestions, breadcrumbs, contextual hints | 🟡 if missing |
| H7 | Flexibility & efficiency | Keyboard shortcuts, search, filters, sorting, bulk actions, customization | 🟢 nice-to-have |
| H8 | Aesthetic & minimalist design | Content hierarchy, no clutter, whitespace usage, focused CTAs, visual noise | 🟡 if violated |
| H9 | Error recovery | Clear error messages (what + how to fix), inline validation, preserve form data on error | 🔴 if missing |
| H10 | Help & documentation | Tooltips, onboarding, empty state guidance, contextual help, FAQ | 🟡 if missing |

### Severity Ratings
- 🔴 **Critical** — Users cannot complete their task. Must fix before shipping.
- 🟡 **Major** — Users can complete task but with significant friction. Should fix.
- 🟢 **Minor** — Optimization opportunity. Can fix later.

---

## WCAG 2.1 AA Accessibility Checklist

| Principle | Requirement | How to Test |
|-----------|-------------|-------------|
| **Perceivable** | Color contrast ≥ 4.5:1 (text), ≥ 3:1 (large text) | DevTools contrast checker |
| | Non-text contrast ≥ 3:1 (buttons, inputs, icons) | Visual inspection |
| | Alt text on all meaningful images | Code review |
| | Captions for video/audio | Content review |
| **Operable** | All functionality keyboard-accessible | Tab through entire page |
| | Focus visible on all interactive elements | Tab + visual check |
| | No keyboard traps | Tab in AND out of every component |
| | Touch targets ≥ 44×44px | Measure in DevTools |
| | Skip navigation link | Tab from page start |
| **Understandable** | Language set on `<html>` | Code review |
| | Form labels linked to inputs | `getByLabelText` in tests |
| | Error messages explain the problem | Test with invalid input |
| | Consistent navigation across pages | Cross-page comparison |
| **Robust** | Valid HTML | HTML validator |
| | ARIA roles used correctly | Accessibility tree |
| | Works in screen readers | VoiceOver/NVDA test |

---

## UX_SPEC.md Template

```markdown
# UX_SPEC: [Feature Name]
> Author: UX Research & Usability Expert
> Date: [date]
> Based on: PRD [link]

## 1. User Research

### Target Users
| Persona | Goals | Pain Points | Tech Level |
|---------|-------|-------------|------------|
| [Name] | [What they want to achieve] | [Current frustrations] | Beginner/Intermediate/Advanced |

### User Journey Map
\`\`\`mermaid
journey
    title [Feature] User Journey
    section Discovery
      Find feature: 3: User
      Understand purpose: 4: User
    section Engagement
      First interaction: 5: User
      Complete task: 4: User
    section Completion
      See result: 5: User
      Share/export: 3: User
\`\`\`

## 2. Information Architecture

### Navigation Structure
- Where does this feature live in the app hierarchy?
- How do users discover it?
- What breadcrumb path leads here?

### Content Hierarchy
1. Primary content/action (what users came for)
2. Supporting content (context, details)
3. Secondary actions (settings, sharing)
4. Meta information (timestamps, status)

## 3. User Flows

### Happy Path
\`\`\`mermaid
flowchart TD
    A[Entry Point] --> B[Step 1]
    B --> C[Step 2]
    C --> D{Decision}
    D -->|Option A| E[Success]
    D -->|Option B| F[Alternative]
\`\`\`

### Error Paths
- What if [input is invalid]? → [show inline error with fix suggestion]
- What if [API fails]? → [show retry option with clear message]
- What if [no data]? → [show empty state with guidance]

## 4. Interaction Patterns

### States
| Element | Default | Hover | Active | Focus | Disabled | Loading | Error | Success |
|---------|---------|-------|--------|-------|----------|---------|-------|---------|
| [Button] | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| [Form] | ✅ | — | — | ✅ | ✅ | ✅ | ✅ | ✅ |

### Micro-interactions
- Form submission → button loading state → success/error feedback
- Delete action → confirmation dialog → undo toast (3s)
- Toggle → instant visual feedback + optimistic UI

## 5. Accessibility Requirements

- Keyboard navigation: full tab order documented
- Screen reader: all content has semantic meaning
- Color: no information conveyed by color alone
- Motion: `prefers-reduced-motion` respected
- Touch: all targets ≥ 44×44px

## 6. Responsive Behavior

| Breakpoint | Layout | Navigation | Key Differences |
|------------|--------|------------|-----------------|
| Mobile (375px) | Single column | Bottom nav / hamburger | Simplified, touch-first |
| Tablet (768px) | 2-column | Side nav | Expanded content |
| Desktop (1440px) | Multi-column | Top nav + sidebar | Full feature set |

## 7. Error Messages (Specific)

| Error Scenario | Message | Heuristic |
|----------------|---------|-----------|
| Empty required field | "[Field name] is required" | H9 |
| Invalid email | "Please enter a valid email (e.g., name@company.com)" | H9, H2 |
| API timeout | "Couldn't connect. [Try again] or check your connection." | H9, H3 |
| No results | "No results for '[query]'. Try different keywords or [browse all]." | H9, H6 |
```

---

## USABILITY_REVIEW.md Template

```markdown
# USABILITY_REVIEW: [Feature Name]
> Reviewer: UX Research & Usability Expert
> Date: [date]
> Build: [url]

## Summary
[X] issues found: [N] 🔴 Critical, [N] 🟡 Major, [N] 🟢 Minor

## Heuristic Evaluation

### 🔴 Critical Issues

#### H[N] — [Heuristic Name]: [Issue Title]
- **Where**: [page/component]
- **What**: [description of the problem]
- **Why it matters**: [impact on user]
- **Fix**: [specific recommendation]
- **Evidence**: ![screenshot](path/to/screenshot.png)

### 🟡 Major Issues
[same format]

### 🟢 Minor Issues
[same format]

## Accessibility Audit
| Check | Status | Notes |
|-------|--------|-------|
| Keyboard navigation | ✅/❌ | [details] |
| Focus visibility | ✅/❌ | [details] |
| Color contrast | ✅/❌ | [ratios] |
| Screen reader compatibility | ✅/❌ | [details] |
| Touch targets | ✅/❌ | [sizes] |

## Verdict
- [ ] ✅ USABLE — No critical issues, ship with minor fixes
- [ ] 🟡 NEEDS WORK — Major issues require attention before shipping
- [ ] 🔴 UNUSABLE — Critical usability problems, must redesign
```
