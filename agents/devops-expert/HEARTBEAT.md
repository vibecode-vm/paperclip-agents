# HEARTBEAT.md — DevOps Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack, shared rules.
- Read `../../MEMORY.md` — known infrastructure gotchas.
- Check existing infrastructure: `ls docker-compose*.yml Dockerfile* .github/workflows/`
- Verify environment variables are documented

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — know what services exist.
2. Check existing CI/CD: `ls .github/workflows/`
3. Check existing Docker: `ls Dockerfile* docker-compose*`
4. Check deploy scripts: `ls scripts/deploy*`

**After finishing work:**
Update `../../SYSTEM_STATE.md` with infrastructure changes.

## 5. CI/CD Pipeline (GitHub Actions)

### Pipeline Stages
```yaml
# .github/workflows/ci.yml
stages:
  1. Checkout + Cache (node_modules, .next/cache)
  2. Install dependencies (npm ci)
  3. Lint (npm run lint)
  4. Type check (npx tsc --noEmit)
  5. Unit tests (npx vitest run --coverage)
  6. Build (npm run build)
  7. Security audit (npm audit --audit-level=high)
  8. Docker build + push (if main branch)
  9. Deploy (if main branch, staging first)
```

### Pipeline Checklist
- [ ] Actions run on push to main + PRs
- [ ] Cache strategy reduces install time
- [ ] Tests run before build
- [ ] Build artifacts are versioned
- [ ] Security scan passes
- [ ] Deploy only on main branch

## 6. Docker Configuration

### Dockerfile Checklist
- [ ] Multi-stage build (builder → runner)
- [ ] Node Alpine base image (slim)
- [ ] Non-root user in production stage
- [ ] `.dockerignore` excludes `node_modules`, `.git`, docs
- [ ] Health check defined (`HEALTHCHECK CMD`)
- [ ] Specific Node version pinned (not `latest`)

### Docker Compose Checklist
- [ ] All services defined (app, db, redis if needed)
- [ ] Volumes for persistent data
- [ ] Environment variables via `.env` file
- [ ] Network isolation between services
- [ ] Restart policies defined (`unless-stopped`)

## 7. Environment Management
```
Development:  docker-compose.dev.yml   (hot reload, debug)
Staging:      docker-compose.staging.yml (production-like)
Production:   docker-compose.prod.yml   (optimized, monitored)
```

## 8. Monitoring & Health Checks
- [ ] `/api/health` endpoint returns 200 + service info
- [ ] Structured logging via pino (JSON format)
- [ ] Alert on: 5xx errors > threshold, response time > 2s, disk > 80%
- [ ] Log rotation configured
- [ ] Uptime monitoring active

## 9. Pre-Delivery Checklist
- [ ] CI pipeline runs green on main
- [ ] Docker builds successfully
- [ ] Health checks pass in staging
- [ ] Secrets are NOT in code/Dockerfiles
- [ ] Rollback procedure documented
- [ ] Deploy time under 5 minutes
- [ ] SYSTEM_STATE.md updated

## 10. Exit
- Comment on in_progress work.
- Document any infrastructure changes in SYSTEM_STATE.md.

## Rules
- NEVER hardcode secrets in Dockerfiles, CI configs, or code.
- ALWAYS use multi-stage Docker builds for production.
- ALWAYS pin versions (Node, npm packages, Docker base images).
- CI/CD must run ALL tests before deploy.
- Rollback must be possible within 2 minutes.
