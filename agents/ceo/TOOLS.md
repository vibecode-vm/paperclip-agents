# Tools

## Paperclip API

- `GET /api/agents/me` — confirm current identity, config, and budget.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox for heartbeat work.
- `GET /api/approvals/{approvalId}` and `GET /api/approvals/{approvalId}/issues` — approval follow-up path after board decisions.
- `PATCH /api/issues/{issueId}` — use for status updates and concise markdown comments. Always include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Strategic Brainstorming — brainstorming (obra/superpowers, 50K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**HARD-GATE: No resource allocation without strategic alignment. Every project.**

**Decision Process:**
1. Is this aligned with company goals? If not → reject.
2. Do we have the right agents/resources? If not → identify gaps.
3. What's the risk? → Classify: low / medium / high.
4. What's the timeline? → Realistic estimate, not aspirational.
5. Decision: GO / NO-GO / NEED MORE INFO.

### 2. Verification — verification-before-completion (obra/superpowers, 16K)

**Before approving any delivery:**
- Evidence of functional completion (test results, screenshots, reports)
- Evidence of quality (audit reports, review verdicts)
- Evidence of alignment (does it match the original GOAL.md?)

### 3. Subagent-Driven Development — subagent-driven-development (obra/superpowers, 34K)

**Delegation principles:**
- Clear scope per agent (never "figure it out")
- Specific success criteria per task
- Two-stage review: spec compliance → quality
- Escalation path defined before work starts

---

## Local Verification

- Use local file reads to verify agent-home docs and memory files.
- Use Paperclip API responses as the source of truth for issue, approval, and agent status.

## Memory Tooling

- `qmd` is expected by the memory workflow, but it is not currently installed in this runtime (`qmd: command not found`). Fall back to direct file reads/writes until it is available.

## Decision Framework

| Priority | When | Action |
|----------|------|--------|
| 🔴 P0 — Critical | Production down, data loss | Drop everything, all hands |
| 🟠 P1 — High | Feature blocker, security issue | Resolve this sprint |
| 🟡 P2 — Medium | Enhancement, tech debt | Schedule next sprint |
| 🟢 P3 — Low | Nice to have, optimization | Backlog |
