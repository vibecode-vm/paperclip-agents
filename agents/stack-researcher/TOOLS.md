# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Find Skills — find-skills (vercel-labs, #1 most installed)

Find new skills for specific problems:
```bash
skills search "date picker"
skills search "form validation"
skills search "authentication"
```

### 2. Competitor Analysis — competitor-alternatives (coreyhaines31, 16K)

Compare packages and alternatives:
- Feature comparison matrix
- Bundle size comparison
- Community health check

### 3. Web Research — firecrawl (firecrawl/cli, 11K)

Crawl documentation and comparison pages.

**Auto-Install: Check if Firecrawl is available, install if not:**
```bash
# Check if firecrawl CLI exists
which firecrawl || npx -y firecrawl --help

# If not installed → install globally
npm install -g firecrawl

# OR use via npx (no install needed)
npx -y firecrawl scrape <url>

# OR install as MCP server (recommended for OpenCode)
# Add to MCP config:
# {
#   "firecrawl": {
#     "command": "npx",
#     "args": ["-y", "firecrawl-mcp"],
#     "env": { "FIRECRAWL_API_KEY": "<key>" }
#   }
# }
```

**Usage:**
```bash
firecrawl scrape <docs-url>         # Scrape a single page
firecrawl crawl <site-url>          # Crawl entire site
firecrawl scrape <url> --format md  # Get markdown output
```

### 4. Brainstorming — brainstorming (obra, 50K)

Structured ideation for finding approaches:
- What are the possible solutions?
- What are the trade-offs?
- Which fits our stack best?

### 5. Agent Browser — agent-browser (vercel-labs, 89K)

For researching live documentation and package pages:
```bash
agent-browser open https://bundlephobia.com/package/<pkg>
agent-browser screenshot bundle-size.png
```

### 6. Context7 Documentation Fetcher — context7 (am-will/codex-skills, 12K)

Fetch **latest, version-specific documentation** for any library. This prevents outdated API usage and hallucinated code.

**When to use:**
- Before implementing any library-dependent feature
- When unsure about current API signatures or patterns
- For version-specific behavior (e.g., Next.js 15 vs 14)
- To verify best practices and current patterns

### 7. Security Best Practices — security-best-practices (supercent-io, 11K)

Security guidelines for web applications:
- Auth patterns, CSRF protection, XSS prevention
- Dependency vulnerability assessment
- Secure coding patterns

### 8. Code Review — code-review (supercent-io, 11K)

Structured code review patterns to evaluate recommended libraries.

---

## Context7 — Latest Documentation Fetching

### Setup
Context7 is available as MCP server or as skills.sh script. Both work.

**Option A: MCP Server (recommended for OpenCode)**
```json
// In MCP config:
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp"]
  }
}
```

**Option B: Skills.sh Script**
```bash
# Install
npx skills add https://github.com/am-will/codex-skills --skill context7

# API Key stored in: ~/.agents/skills/context7/.env
# CONTEXT7_API_KEY=<your-key>
```

### Workflow

**Step 1: Search for the library**
```bash
# Via MCP: resolve-library-id
mcp context7 resolve-library-id "next.js"
mcp context7 resolve-library-id "react"
mcp context7 resolve-library-id "zustand"
mcp context7 resolve-library-id "tailwindcss"

# Via script:
python3 ~/.agents/skills/context7/scripts/context7.py search "next.js"
```
Returns library ID (e.g., `/vercel/next.js`).

**Step 2: Fetch documentation for specific topic**
```bash
# Via MCP: get-library-docs
mcp context7 get-library-docs "/vercel/next.js" "app router middleware"
mcp context7 get-library-docs "/facebook/react" "useEffect cleanup"
mcp context7 get-library-docs "/pmndrs/zustand" "persist middleware"
mcp context7 get-library-docs "/tailwindlabs/tailwindcss" "v4 migration"

# Via script:
python3 ~/.agents/skills/context7/scripts/context7.py context "/vercel/next.js" "app router middleware"
python3 ~/.agents/skills/context7/scripts/context7.py context "/vercel/next.js" "server actions" --type md
```

### Quick Reference — Mandated Stack Docs

| Library | Context7 ID | Example Queries |
|---------|-------------|------------------|
| Next.js 15 | `/vercel/next.js` | "app router", "server actions", "middleware", "image optimization" |
| React 19 | `/facebook/react` | "useEffect", "server components", "suspense", "transitions" |
| Tailwind CSS 4 | `/tailwindlabs/tailwindcss` | "v4 config", "custom properties", "dark mode" |
| Zustand | `/pmndrs/zustand` | "persist middleware", "devtools", "slices pattern" |
| Zod | `/colinhacks/zod` | "schema validation", "transform", "discriminated unions" |
| shadcn/ui | `/shadcn-ui/ui` | "dialog", "form", "data-table", "command" |
| React Hook Form | `/react-hook-form/react-hook-form` | "zod resolver", "controlled inputs" |
| TanStack Query | `/TanStack/query` | "mutations", "infinite queries", "prefetching" |
| Supabase | `/supabase/supabase` | "auth", "row level security", "realtime" |
| Drizzle ORM | `/drizzle-team/drizzle-orm` | "schema", "migrations", "relations" |
| Vitest | `/vitest-dev/vitest` | "mocking", "coverage", "workspace" |
| MSW | `/mswjs/msw` | "http handlers", "request matching" |
| Playwright | `/microsoft/playwright` | "locators", "assertions", "fixtures" |

### Documentation Output

Save fetched docs to `docs/lib/` so programming agents can reference them:
```bash
# Fetch and save
mcp context7 get-library-docs "/vercel/next.js" "app router" > docs/lib/nextjs-app-router.md
mcp context7 get-library-docs "/pmndrs/zustand" "persist" > docs/lib/zustand-persist.md
```

Then in HANDOFF to Builder:
```markdown
## Documentation References
- Next.js App Router: `docs/lib/nextjs-app-router.md`
- Zustand Persist: `docs/lib/zustand-persist.md`
```

---

## npm Research Commands

### Version Check
```bash
# Check current versions
npm list --depth=0

# Check outdated packages
npm outdated

# Check specific package info
npm info <package> version
npm info <package> time.modified
npm info <package> peerDependencies
npm info <package> license

# Check for vulnerabilities
npm audit
npm audit --production
```

### Package Evaluation
```bash
# Bundle size (use bundlephobia.com)
agent-browser open https://bundlephobia.com/package/<package>@latest
agent-browser screenshot <package>-bundle.png

# Download trends (use npmtrends.com)
agent-browser open https://npmtrends.com/<pkg1>-vs-<pkg2>-vs-<pkg3>
agent-browser screenshot comparison.png

# GitHub health
agent-browser open https://github.com/<owner>/<repo>
# Check: stars, last commit, open issues, contributors
```

### Compatibility Test
```bash
# Dry-run install to check conflicts
npm install --dry-run <package>

# Check peer dependency tree
npm explain <package>

# Check for duplicate packages
npm dedupe --dry-run
```

---

## Templates

### RESEARCH.md (Library Recommendation)
```markdown
# RESEARCH: [Topic — e.g., "Date Picker for Dashboard"]
> Researcher: Technical Research & Stack Validation Agent
> Date: [date]
> Requested by: [Orchestrator / Gatekeeper]

## Question
What is the best [type of library] for [specific use case]?

## Requirements
- Must work with: Next.js 15, React 19, Tailwind 4
- Must support: [specific features]
- Bundle budget: < [X]KB gzipped
- TypeScript: required

## Comparison

| Criterion | Package A | Package B | Package C |
|-----------|-----------|-----------|-----------|
| **Name** | `pkg-a` | `pkg-b` | `pkg-c` |
| Version | 3.2.1 | 2.0.0 | 1.5.0 |
| Last published | 2 weeks ago | 3 months ago | 8 months ago ⚠️ |
| Weekly downloads | 500K | 120K | 30K |
| Bundle size (gzip) | 12KB ✅ | 45KB ⚠️ | 8KB ✅ |
| TypeScript | Built-in ✅ | @types ⚠️ | Built-in ✅ |
| Tree-shakeable | Yes ✅ | No ❌ | Yes ✅ |
| Peer deps conflict | None ✅ | React 18 only ❌ | None ✅ |
| GitHub stars | 15K | 8K | 2K |
| License | MIT ✅ | MIT ✅ | MIT ✅ |
| shadcn/ui compatible | Yes ✅ | No ❌ | Yes ✅ |

## Recommendation
**Use Package A** because:
1. Actively maintained (2 weeks ago)
2. Smallest bundle that meets requirements (12KB)
3. No peer dependency conflicts
4. shadcn/ui compatible

## Integration Guide
\`\`\`bash
npm install pkg-a
\`\`\`

\`\`\`tsx
import { Component } from 'pkg-a'

// Usage example
<Component prop="value" />
\`\`\`

## Risks
- [any migration concerns]
- [any known limitations]
```

### STACK_AUDIT.md (Version & Health Check)
```markdown
# STACK_AUDIT: [Project Name]
> Auditor: Technical Research & Stack Validation Agent
> Date: [date]

## Package Versions

### Core Stack
| Package | Current | Latest | Status | Action |
|---------|---------|--------|--------|--------|
| next | 15.0.3 | 15.1.0 | ⚠️ Minor behind | Update recommended |
| react | 19.0.0 | 19.0.0 | ✅ Latest | — |
| tailwindcss | 4.0.0 | 4.0.1 | ⚠️ Patch behind | Update (safe) |
| zustand | 5.0.0 | 5.0.0 | ✅ Latest | — |

### UI
| Package | Current | Latest | Status | Action |
|---------|---------|--------|--------|--------|
| @radix-ui/react-dialog | 1.0.5 | 1.1.0 | ⚠️ Minor behind | Review changelog |

### Dev Dependencies
| Package | Current | Latest | Status | Action |
|---------|---------|--------|--------|--------|
| vitest | 2.0.0 | 2.1.0 | ⚠️ Minor | Update |
| typescript | 5.5.0 | 5.6.0 | ⚠️ Minor | Update |

## Vulnerabilities
\`\`\`bash
npm audit
# [output]
\`\`\`

- 🔴 Critical: [N]
- 🟡 High: [N]
- 🟢 Low: [N]

## Peer Dependency Conflicts
- [list any conflicts]
- [or "None found ✅"]

## Boilerplate Opportunities
| Pattern | Occurrences | Solution |
|---------|-------------|----------|
| Manual fetch + error handling | 12 files | Use @tanstack/react-query |
| Manual form validation | 8 forms | Use react-hook-form + Zod resolver |
| Repeated loading states | 15 components | Create shared Skeleton component |

## Update Plan
1. **Safe updates** (patches): `npm update`
2. **Minor updates** (review changelogs first): [list]
3. **Major updates** (need testing): [list]
4. **Don't update**: [list with reason]

## Verdict
- [ ] ✅ Stack is healthy — proceed with feature
- [ ] ⚠️ Updates needed before feature — [list critical ones]
- [ ] 🔴 Critical vulnerabilities — must fix first
```
