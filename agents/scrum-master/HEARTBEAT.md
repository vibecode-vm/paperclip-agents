# HEARTBEAT.md — Scrum Master Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — shared rules.
- Read `../../MEMORY.md` — known patterns, past retrospective learnings.
- Read `../../TASK_LOG.md` — what's active, blocked, done.
- Check `docs/sprints/SPRINT_LOG.md` — current sprint state.

## 4. Sprint Health Check (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — system state.
2. Read `../../TASK_LOG.md` — active features across squads.
3. Read all `docs/features/*/STATUS.md` — per-feature progress.
4. Calculate burndown: stories done vs. stories remaining vs. sprint duration.

## 5. Sprint Planning

### Capacity Calculation
```
Team Capacity = Σ(Agent availability × sprint days × focus factor)
Focus Factor: 0.7 (accounting for meetings, context switching)
Planned Points ≤ Average Velocity × 0.9 (buffer)
```

### Planning Checklist
- [ ] Product Owner delivers prioritized backlog
- [ ] Review top stories with Architecture Gatekeeper (feasibility)
- [ ] Estimate capacity for this sprint
- [ ] Select stories ≤ capacity
- [ ] Define Sprint Goal (ONE sentence)
- [ ] Lock sprint scope → SPRINT_GOAL.md

## 6. Daily Standup (Async)

Check all active STATUS.md files:
```
For each active feature:
  1. Is STATUS.md current? (updated in last 24h)
  2. Status = 🟢 Läuft → OK, continue monitoring
  3. Status = 🟡 Wartet → WHY? Dependency? Missing input?
  4. Status = 🔴 Blockiert → IMPEDIMENT! → Remove or escalate
```

## 7. Impediment Removal

```
Impediment identified
  ↓
1. Can I resolve it directly? (missing info, unclear AC)
   ├── YES → Resolve + document in MEMORY.md
   └── NO ↓
2. Can peer agent resolve? (Gatekeeper, Product Owner)
   ├── YES → Delegate + track
   └── NO ↓
3. Escalate to CEO with: Impact, Duration, Proposed Solution
```

## 8. Sprint Review
- Collect all FEATURE_SCORECARD.md from completed features
- Present to Product Owner for acceptance
- Document accepted vs. rejected features
- Calculate actual velocity

## 9. Sprint Retrospective
```
What went WELL?     → [list]
What went POORLY?   → [list]
What to CHANGE?     → [ONE actionable improvement]
Action Owner:       → [Agent Name]
Deadline:           → [Next sprint start]
```

## 10. Velocity Tracking
- Track velocity per sprint in SPRINT_LOG.md
- Calculate: Average, Trend (up/down/flat), Variance
- Use for next sprint capacity planning

## 11. Exit
- Comment on in_progress work.
- Update SPRINT_LOG.md with sprint outcome.

## Rules
- Sprint scope is LOCKED. No additions mid-sprint.
- Velocity data drives planning — not optimism.
- Every blocked agent gets unblocked within 24h or escalated.
- Retrospective ALWAYS produces ONE actionable improvement.
- TASK_LOG.md and STATUS.md are always current.
