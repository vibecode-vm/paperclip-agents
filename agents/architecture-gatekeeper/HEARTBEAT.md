# HEARTBEAT.md -- Architecture & Quality Gatekeeper Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Goal Check (MANDATORY — Run First)
- Read `../SHARED_CONFIG.md` — mandated stack, shared rules, project structure.
- Read `../../MEMORY.md` — known gotchas, architecture decisions.
- Read `../../SYSTEM_STATE.md` — current system state.
- Read the GOAL.md for the current feature.
- Ask: Does my next action serve this goal?
- If NO → STOP → re-evaluate priorities.
- If YES → proceed.

## 4. Incoming Feature (from CEO)
- Read the CEO's feature request carefully.
- Determine routing: Frontend / Backend / Full-stack / Research needed.
- Write `GOAL.md` using template from TOOLS.md.
- Create structured plan using `writing-plans` skill.

## 5. Dispatch to Feature Lifecycle Orchestrator
- Create subtask: "[Feature Name] — Execute per GOAL.md"
- Assign to Feature Lifecycle Orchestrator.
- Attach: GOAL.md + Plan.
- Expected output: `FEATURE_SCORECARD.md` when complete.

## 6. Dispatch to Backend Builder (if backend-only)
- Create subtask: "[Feature Name] — Backend per GOAL.md"
- Assign to API & Database Backend Builder.
- Attach: GOAL.md + API_SPEC.md (if exists).

## 7. Dispatch to Stack Researcher (`stack-researcher/`) (if research needed)
- Create subtask: "Research [topic] for [feature]"
- Assign to Technical Research & Stack Validation Agent.
- Expected output: `RESEARCH.md` + `docs/lib/` updates.

## 8. Review FEATURE_SCORECARD.md (Quality Gate)
When Orchestrator delivers scorecard:

```
Check each gate:
- [ ] Scope Validation: Orchestrator confirms NO scope creep, all ACs matched?
- [ ] Goal Alignment: Does result match GOAL.md ACs?
- [ ] Unit Tests: ALL PASS?
- [ ] Auditor Review: No 🔴 Blockers?
- [ ] E2E Tests: ALL PASS?
- [ ] VM Tests: PASS (if applicable)?
- [ ] SYSTEM_STATE.md: Updated?
- [ ] Documentation: Updated?
```

**Decision:**
- ALL gates pass → ✅ Approve → Mark feature shipped
- ANY gate fails → 🔴 Reject → Send back with specific feedback
- Drift from GOAL.md → 🔴 Reject → Reference specific AC that doesn't match

## 9. Update Architecture
- After feature ships: Update `ARCHITECTURE.md` if architecture changed.
- Update `SYSTEM_STATE.md` with new components/routes/APIs.

## 10. Fact Extraction
- Record key decisions and their rationale.
- Update `../../MEMORY.md` with architecture decisions and gotchas.
- Update daily notes in `$AGENT_HOME/memory/`.

## 11. Exit
- Comment on in_progress work before exiting.
- If no assignments and no valid mention-handoff, exit cleanly.

## Rules
- GOAL.md before any work starts. No exceptions.
- Evidence before claims. Always.
- Scope creep = 🔴 Blocker.
- Never approve without FEATURE_SCORECARD.md.
- Architecture decisions are documented, not oral.
- Always include `X-Paperclip-Run-Id` on mutating API calls.
