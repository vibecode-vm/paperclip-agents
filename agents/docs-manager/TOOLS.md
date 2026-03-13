# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Documentation Writing — documentation-writer (github/awesome-copilot, 8K)

**Good documentation is:**
- **Scannable**: Headers, tables, code blocks. No walls of text.
- **Precise**: Exact file paths, exact component names, exact API endpoints.
- **Current**: Reflects the actual state of the code, not historical.
- **Examples**: Every API endpoint has a request/response example. Every component has a usage example.

**Clear Writing Principles (Superpowers):**
- Make every word earn its place — cut ruthlessly
- One idea per sentence, one topic per paragraph
- Active voice ("User creates account" not "Account is created by user")
- Specific > vague ("reduces load time by 40%" not "improves performance")
- Avoid jargon unless the audience expects it

### 2. Changelog Maintenance — changelog-maintenance (skills.sh, ~5K)

**CHANGELOG.md follows Keep a Changelog format:**
```markdown
## [version] — YYYY-MM-DD

### Added
- New feature description

### Changed
- Changed behavior description

### Fixed
- Bug fix description

### Removed
- Removed feature description
```

### 3. Verification — verification-before-completion (obra/superpowers, 16K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Before reporting docs as updated:**
- Verify ALL links work (dead links = doc rot)
- Verify component/route lists match ACTUAL code — `grep` to confirm
- Cross-reference: every documented component exists in code, every code component is documented
- SYSTEM_STATE.md matches reality — don't just update timestamps

**Spec Document Reviewer Pattern (Superpowers):**
After writing any spec/documentation, self-review against these criteria:
1. Does every claim have supporting code or evidence?
2. Are file paths absolute and correct?
3. Are code examples copy-pasteable and functional?
4. Is anything ambiguous? Rewrite until unambiguous.

### 4. Agent Browser — agent-browser (vercel-labs, 89K)

- Use to verify live documentation pages render correctly.
- Take screenshots of documentation for review.

---

## Templates

### SYSTEM_STATE.md
```markdown
# System State — [Project Name]
> Last updated: [date] by Documentation & Knowledge Manager
> After: [feature that triggered update]

## Routes
| Route | Page | Status |
|-------|------|--------|
| `/` | Home | ✅ Active |
| `/dashboard` | Dashboard | ✅ Active |

## Components
| Component | Location | Used By |
|-----------|----------|---------|
| `Button` | `components/ui/button.tsx` | Throughout |
| `Header` | `components/layout/header.tsx` | All pages |

## API Endpoints
| Method | Endpoint | Purpose | Auth |
|--------|----------|---------|------|
| GET | `/api/users` | List users | Yes |
| POST | `/api/auth/login` | Login | No |

## State Stores (Zustand)
| Store | Location | Purpose |
|-------|----------|---------|
| `useAuthStore` | `stores/auth.ts` | Authentication state |
| `useThemeStore` | `stores/theme.ts` | Theme preference |

## External Dependencies
| Package | Version | Why |
|---------|---------|-----|
| `next` | 15.x | Framework |
| `shadcn/ui` | latest | UI Components |

## Environment Variables
| Variable | Required | Purpose |
|----------|----------|---------|
| `DATABASE_URL` | Yes | Database connection |
| `NEXT_PUBLIC_API_URL` | Yes | API base URL |
```

### Component Documentation
```markdown
# [ComponentName]

## Overview
[1-2 sentence description]

## Usage
\`\`\`tsx
import { ComponentName } from '@/components/feature/ComponentName'

<ComponentName prop="value" />
\`\`\`

## Props
| Prop | Type | Default | Required | Description |
|------|------|---------|----------|-------------|
| `prop` | `string` | — | Yes | Description |

## Variants
[Show each variant with example]

## Accessibility
- Role: `button` / `dialog` / etc.
- Keyboard: Tab, Enter, Escape
- ARIA: `aria-label`, `aria-expanded`

## Related
- Design spec: `DESIGN_SPEC.md#component-name`
- Tests: `ComponentName.test.tsx`
```

### API Documentation
```markdown
# [Endpoint Name]

## `METHOD /api/path`

### Description
[What this endpoint does]

### Request
\`\`\`json
{
  "field": "value"
}
\`\`\`

### Response (200)
\`\`\`json
{
  "data": { ... }
}
\`\`\`

### Response (400)
\`\`\`json
{
  "error": "Validation failed",
  "details": [...]
}
\`\`\`

### Authentication
Required: Yes/No
```
