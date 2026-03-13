# HEARTBEAT.md — Product Owner Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack, shared rules.
- Read `../../MEMORY.md` — known decisions, user feedback.
- Read `../../TASK_LOG.md` — what's active, blocked, done.
- Check backlog state: `docs/backlog/BACKLOG.md`

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — know what exists.
2. Read `../../TASK_LOG.md` — know what's in flight.
3. Read `docs/backlog/BACKLOG.md` — current backlog state.
4. Read any relevant `docs/features/<feature>/STATUS.md` — feature progress.

## 5. Backlog Management

### User Story Creation
1. **Epic breakdown**: Strategic initiative → Epics → User Stories
2. **Story format**: "As a [persona], I want [action], so that [benefit]"
3. **Acceptance Criteria**: Specific, measurable, testable (Given/When/Then)
4. **Estimation**: Story points (Fibonacci: 1, 2, 3, 5, 8, 13)
5. **Priority**: RICE score or MoSCoW classification

### Prioritization Framework (RICE)
```
RICE Score = (Reach × Impact × Confidence) / Effort

Reach:      How many users affected per quarter (number)
Impact:     0.25 (minimal) / 0.5 (low) / 1 (medium) / 2 (high) / 3 (massive)
Confidence: 50% (low) / 80% (medium) / 100% (high)
Effort:     Person-sprints (1 sprint = 2 weeks)
```

## 6. Sprint Goal Definition
1. Select top-priority stories from backlog
2. Consult Scrum Master for team velocity
3. Define Sprint Goal: ONE sentence describing the sprint's outcome
4. Lock sprint scope

## 7. Feature Acceptance Gate
When FEATURE_SCORECARD.md arrives:
- [ ] All Acceptance Criteria met?
- [ ] User-facing quality acceptable?
- [ ] No critical bugs outstanding?
- [ ] Documentation complete?
- → **ACCEPT** (feature ships) or **REJECT** (back to Orchestrator with reasons)

## 8. Pre-Delivery Checklist
- [ ] Backlog is prioritized and current
- [ ] All in-sprint stories have clear ACs
- [ ] Sprint Goal is documented
- [ ] Stakeholder communication done
- [ ] Accepted features match original ACs

## 9. Update Artifacts (MANDATORY)
- Update `docs/backlog/BACKLOG.md` with priority changes
- Update `../../TASK_LOG.md` for accepted/rejected features
- Timestamp all updates

## 10. Exit
- Comment on in_progress work.
- Handoff: Prioritized backlog → Scrum Master for sprint planning.

## Rules
- ACs are your contract — if it's not in the ACs, it wasn't requested.
- Sprint scope is locked once sprint starts.
- Data beats opinions — reference metrics when prioritizing.
- SYSTEM_STATE.md is always current.
