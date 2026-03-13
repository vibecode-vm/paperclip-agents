# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Dispatching Parallel Agents — dispatching-parallel-agents (obra/superpowers, 15K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Coordinate multiple squads simultaneously:**
1. Identify independent work streams
2. Dispatch to parallel agents with clear scope
3. Monitor progress via STATUS.md
4. Integrate results, resolve conflicts

### 2. Subagent-Driven Development — subagent-driven-development (obra/superpowers, 34K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**The fastest iteration mode — fresh subagent per task with two-stage review:**

1. Read plan → extract all tasks → create task tracking
2. Per task:
   - Dispatch implementer subagent with full task text + context
   - Implementer: implements, tests, commits, self-reviews
   - Dispatch spec reviewer → confirms code matches spec
   - Dispatch code quality reviewer → approves code quality
   - Mark task complete
3. After all tasks → dispatch final code reviewer → finish branch

**Implementer Status Handling:**
| Status | Action |
|--------|--------|
| DONE | Proceed to spec compliance review |
| DONE_WITH_CONCERNS | Read concerns. Correctness/scope → address before review. Observations → note and proceed. |
| NEEDS_CONTEXT | Provide missing context, re-dispatch |
| BLOCKED | Context problem → add context. Too complex → upgrade model. Task too large → split. Plan wrong → escalate to human. |

**Model Selection (cost-optimize):**
- Touches 1-2 files with clear spec → cheap/fast model
- Multi-file integration → standard model
- Architecture/design/review → most capable model

### 3. Executing Plans — executing-plans (obra/superpowers, 22K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Sprint execution (batch mode with checkpoints):**
1. Load sprint plan
2. Track each story: `todo` → `in_progress` → `done`
3. Verify completion evidence
4. Update velocity data

### 4. Swarm Planner — swarm-planner (am-will, 12K)
**Dependency-Aware Sprint Planning:**
```
T1: [depends_on: []] Design phase
T2: [depends_on: []] Research phase
T3: [depends_on: [T1, T2]] Build phase
T4: [depends_on: [T3]] Test phase
```

---

## Templates

### SPRINT_LOG.md
```markdown
# Sprint Log

> Updated: [timestamp]

## Current Sprint: Sprint [N]

| Field | Value |
|-------|-------|
| Start | [date] |
| End | [date] |
| Goal | [one sentence] |
| Planned Points | [number] |
| Completed Points | [number] |
| Velocity | [number] |

## Sprint History

| Sprint | Goal | Planned | Completed | Velocity | Retro Action |
|--------|------|---------|-----------|----------|-------------|
| N-1 | [goal] | 34 | 31 | 31 | [action] |
| N-2 | [goal] | 30 | 32 | 32 | [action] |

## Velocity Trend
- Average (last 3): [number]
- Trend: [↑ improving / → stable / ↓ declining]
```

### IMPEDIMENT_LOG.md
```markdown
# Impediment Log

| # | Date | Agent | Impediment | Impact | Resolution | Resolved |
|---|------|-------|-----------|--------|------------|----------|
| 1 | [date] | [agent] | [description] | [high/medium/low] | [how resolved] | ✅ / 🔴 |
```

### RETROSPECTIVE.md
```markdown
# Sprint [N] Retrospective

> Date: [date]
> Velocity: [actual] / [planned]

## What went WELL ✅
- [item]

## What went POORLY ❌
- [item]

## Actionable Improvement 🔄
**What**: [specific change]
**Owner**: [Agent Name]
**Deadline**: [Sprint N+1 start]
**Success Metric**: [how we know it worked]
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Shared rules (Goal Check, Evidence, Circuit Breaker, HANDOFF)
- Anti-Hallucination Protocol
- Project folder structure
