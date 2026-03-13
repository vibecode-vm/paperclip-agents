# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

**4-Phase Process for Deployment Failures:**
1. **Root Cause Investigation** — Which CI step failed? Read logs COMPLETELY. Check Docker build, env vars, health endpoint. Add diagnostic instrumentation at EACH component boundary.
2. **Pattern Analysis** — Find a WORKING deployment. Compare differences (env, config, deps).
3. **Hypothesis Testing** — SINGLE hypothesis, SMALLEST change, verify. Don't stack fixes.
4. **Implementation** — Fix at root cause, not symptom. Verify with GREEN pipeline.

**Multi-Component Systems (Superpowers):**
```
For EACH component boundary:
  - Log what data enters component
  - Log what data exits component
  - Verify environment/config propagation
  - Check state at each layer
Run once → evidence shows WHERE it breaks → investigate THAT component
```

### 2. Verification — verification-before-completion (obra/superpowers, 16K)

**Before claiming "deployed":**
1. Pipeline ran to completion? Show GREEN status.
2. Health check returns 200? Show `curl` output.
3. Staging tests pass? Show test results.
4. No critical vulnerabilities? Show `npm audit` / container scan.
5. Evidence before claims — ALWAYS.

### 3. Git Worktrees — using-git-worktrees (obra/superpowers, 14K)

Isolate deployment work on a clean branch:
```bash
git worktree add ../deploy-<feature> -b deploy/<feature>
```

### 4. Branch Finishing — finishing-a-development-branch (obra/superpowers, 14K)

After deployment verification:
1. Verify ALL tests pass (not just "it seems fine")
2. Present options: merge / PR / keep / discard
3. Clean up worktree after merge

---

## Patterns

### GitHub Actions CI/CD
```yaml
name: CI/CD
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      - run: npm ci
      - run: npm run lint
      - run: npx tsc --noEmit
      - run: npx vitest run --coverage

  build:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: docker/build-push-action@v5
        with:
          context: .
          push: ${{ github.ref == 'refs/heads/main' }}
          tags: |
            ghcr.io/${{ github.repository }}:${{ github.sha }}
            ghcr.io/${{ github.repository }}:latest
```

### Multi-Stage Dockerfile
```dockerfile
# Stage 1: Dependencies
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: Builder
FROM node:20-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .
RUN npm run build

# Stage 3: Runner
FROM node:20-alpine AS runner
WORKDIR /app
ENV NODE_ENV=production
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs
COPY --from=builder /app/public ./public
COPY --from=builder --chown=nextjs:nodejs /app/.next/standalone ./
COPY --from=builder --chown=nextjs:nodejs /app/.next/static ./.next/static
USER nextjs
EXPOSE 3000
ENV PORT=3000
HEALTHCHECK --interval=30s --timeout=3s CMD wget -qO- http://localhost:3000/api/health || exit 1
CMD ["node", "server.js"]
```

### Docker Compose (Production)
```yaml
version: '3.8'
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    env_file: .env.production
    restart: unless-stopped
    healthcheck:
      test: ["CMD", "wget", "-qO-", "http://localhost:3000/api/health"]
      interval: 30s
      timeout: 5s
      retries: 3
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:16-alpine
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: ${DB_NAME}
      POSTGRES_USER: ${DB_USER}
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${DB_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

volumes:
  pgdata:
```

### Health Check Endpoint
```typescript
// src/app/api/health/route.ts
import { NextResponse } from 'next/server'

export async function GET() {
  return NextResponse.json({
    status: 'healthy',
    version: process.env.APP_VERSION || 'dev',
    uptime: process.uptime(),
    timestamp: new Date().toISOString(),
  })
}
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack with versions
- Shared rules
- Project folder structure
