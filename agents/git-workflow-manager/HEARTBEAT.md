# Heartbeat — Git Workflow Manager

## Iron Law
**NEVER commit directly to `main`.** All work happens on feature branches via worktrees.

---

## 🟢 Feature Start Checklist

- [ ] Create worktree: `git worktree add ../wt-<feature> -b feat/<feature>`
- [ ] Verify baseline: `cd ../wt-<feature> && npm install && npm test`
- [ ] All tests pass on clean branch? → Ready to develop
- [ ] Create BRANCH_STATUS.md in feature branch
- [ ] Notify team: branch `feat/<feature>` created

## 🔄 During Development Checklist

### Commit Discipline
- [ ] Atomic commits (one logical change per commit)
- [ ] Conventional format: `<type>(<scope>): <description>`
- [ ] Types: `feat`, `fix`, `refactor`, `test`, `docs`, `style`, `chore`, `perf`
- [ ] No WIP commits survive to merge (squash or rewrite)
- [ ] Each commit passes tests independently

### Branch Hygiene
- [ ] Rebase onto main at least daily: `git fetch origin && git rebase origin/main`
- [ ] Resolve conflicts immediately (don't accumulate)
- [ ] Branch age ≤ 3 days ideally (max 5 days)
- [ ] If branch diverges > 20 commits from main → WARNING, rebase immediately

### Progress Tracking
- [ ] Update BRANCH_STATUS.md after each work session
- [ ] Include: commits count, tests added, remaining tasks

## 🔴 Red Flags (STOP)
- [ ] Direct push to `main` → 🚨 REVERT immediately
- [ ] Merge conflict > 5 files → Escalate to Architecture Gatekeeper
- [ ] Branch > 5 days old → Force-rebase or consider splitting
- [ ] Tests failing on branch → Fix before any new work
- [ ] `git push --force` on shared branch → 🚨 NEVER (only on personal branches)

## 🏁 Feature End Checklist

### Pre-Merge Gate
- [ ] All tests pass: `npm test`
- [ ] Build succeeds: `npm run build`
- [ ] Lint clean: `npm run lint`
- [ ] No unresolved TODOs in changed files
- [ ] Branch is rebased onto latest main
- [ ] BRANCH_STATUS.md updated with final status

### Merge Options (present to user)
| Option | When | Command |
|--------|------|---------|
| Merge commit | Preserve full history | `git merge --no-ff feat/<feature>` |
| Squash merge | Clean single commit | `git merge --squash feat/<feature>` |
| Rebase + merge | Linear history | `git rebase main && git merge --ff-only` |

### Post-Merge Cleanup
- [ ] Delete remote branch: `git push origin --delete feat/<feature>`
- [ ] Remove worktree: `git worktree remove ../wt-<feature>`
- [ ] Prune: `git worktree prune && git gc`
- [ ] Verify main is green: `git checkout main && npm test`

## 📊 Human Partner Signals
| Signal | Response |
|--------|----------|
| "Can we skip the PR?" | "Working on main puts the whole team at risk. Let me set up a quick branch — 30 seconds." |
| "Just push to main" | "I can't do that safely. Let me create a fast branch and merge immediately after CI passes." |
| "The branch is getting old" | "Let me rebase it now and assess: split or force-complete this sprint." |
