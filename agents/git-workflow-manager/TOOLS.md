# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Git Worktrees — using-git-worktrees (obra/superpowers, 14K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

```bash
# Create worktree
git worktree add ../feature-<name> -b feature/<name>
cd ../feature-<name>

# Setup
npm ci
npx vitest run  # verify clean baseline

# List worktrees
git worktree list

# Remove worktree (after merge)
git worktree remove ../feature-<name>
git branch -d feature/<name>
```

### 2. Finishing Branches — finishing-a-development-branch (obra/superpowers, 14K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Before finishing:**
```bash
# Verify everything is green
npx vitest run
npx tsc --noEmit
npm run lint

# Check for conflicts
git fetch origin main
git diff origin/main...HEAD --stat
```

**Completion options:**
```bash
# Option 1: Direct merge
git checkout main && git merge feature/<name> --no-ff

# Option 2: PR
git push -u origin feature/<name>
# → Create PR on GitHub

# Option 3: Keep for later
# No action, branch stays

# Option 4: Discard
git worktree remove ../feature-<name>
git branch -D feature/<name>
```

### 3. Verification — verification-before-completion (obra/superpowers, 16K)
Evidence before claiming "merged":
- Show merge commit hash
- Show test results in main after merge
- Show clean `git status`

---

## Templates

### BRANCH_STATUS.md
```markdown
# BRANCH_STATUS: feature/<name>
> Manager: Git Workflow Manager
> Date: [date]
> Feature: [link to GOAL.md]

## Branch Info
| Field | Value |
|-------|-------|
| Branch | `feature/<name>` |
| Worktree | `../feature-<name>` |
| Base | `main` at `[sha]` |
| Commits | [N] commits |
| Files changed | [N] files |
| Lines +/- | +[N] / -[N] |

## Verification
- [ ] Tests: [N]/[N] PASS
- [ ] TypeScript: clean
- [ ] Lint: clean
- [ ] Conflicts with main: None / [list]

## Recommendation
[Merge / PR / Keep / Discard] because [reason]
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Shared rules
- Project folder structure
