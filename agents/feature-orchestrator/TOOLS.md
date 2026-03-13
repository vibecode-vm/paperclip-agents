# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.
- `POST /api/companies/{companyId}/issues` — create subtasks. Set `parentId` + `goalId`.

## Skills

### Specification & Planning

> Source: [obra/superpowers](https://github.com/obra/superpowers)

- **brainstorming** (obra/superpowers, 50K) — Feature ideation. **HARD-GATE: No implementation until design approved. Every project.**
- **prd** (skills.sh, 9K) — Write Product Requirements Documents.
- **content-strategy** (coreyhaines31, 21K) — Content planning.
- **launch-strategy** (coreyhaines31, 17K) — Launch planning and GTM.

### Orchestration

> Source: [obra/superpowers](https://github.com/obra/superpowers)

- **writing-plans** (obra/superpowers, 26K) — Bite-sized tasks (2-5min each), exact file paths, complete code, YAGNI/DRY/TDD.
- **subagent-driven-development** (obra/superpowers, 34K) — **The standard mode:** fresh subagent/task, two-stage review (spec → quality), 4 implementer statuses (DONE/DONE_WITH_CONCERNS/NEEDS_CONTEXT/BLOCKED). Cost-optimize model selection.
- **executing-plans** (obra/superpowers, 22K) — Batch execution with checkpoint verification.
- **dispatching-parallel-agents** (obra/superpowers, 15K) — Independent tasks in parallel.
- **using-git-worktrees** (obra/superpowers, 14K) — Isolate feature work on clean branch.
- **finishing-a-development-branch** (obra/superpowers, 14K) — Verify tests, merge/PR/keep/discard, clean up.

### Quality Gates
- **verification-before-completion** (obra/superpowers, 16K) — Nothing "done" without evidence.
- **requesting-code-review** (obra/superpowers, 21K) — Pre-review checklist, send to Auditor after build.

---

## PRD Template

```markdown
# PRD: [Feature Name]
> Goal: [link to GOAL.md]
> Author: Feature Lifecycle Orchestrator
> Date: [date]

## Problem
What problem does this solve? Who has this problem?

## User Stories
1. As a [user], I want [action] so that [benefit]
   - AC: [acceptance criterion 1]
   - AC: [acceptance criterion 2]

## Design Requirements
- [ ] Aesthetic direction: [bold/minimal/editorial/...]
- [ ] Responsive: Mobile-first (375px → 768px → 1440px)
- [ ] Dark mode: Yes
- [ ] Animations: [describe]
- [ ] Accessibility: WCAG 2.1 AA

## Technical Requirements
- [ ] Components needed: [list]
- [ ] State: [zustand store / react-query / nuqs]
- [ ] API endpoints: [list]
- [ ] Auth required: Yes/No

## Success Metrics
- [ ] [metric 1]
- [ ] [metric 2]

## Out of Scope
- [what this feature does NOT include]

## Routing
- [ ] Frontend → UX Expert → Designer → Builder → Tests → Auditor → E2E → Docs → Ship
- [ ] Backend → API & Database Backend Builder (via Gatekeeper)
- [ ] Full-Stack → Split FE + BE
```

---

## HANDOFF Template

Every agent-to-agent transition uses this format:

```markdown
# HANDOFF: [From Agent] → [To Agent]
> Feature: [Feature Name]
> Step: [N] of 12
> Date: [date]

## Was wurde gemacht
[1-3 Sätze: was der vorherige Agent geliefert hat]

## Ergebnis
[Link zu Deliverable: DESIGN_SPEC.md / REVIEW.md / TEST_REPORT.md etc.]

## Nächste Aufgabe
[Klare Anweisung: was der nächste Agent tun soll]

## Kontext (komprimiert)
[Max 500 Wörter: nur was der nächste Agent wissen muss]
- GOAL.md: [link]
- PRD: [link]
- Relevante Specs: [links]

## Offene Fragen / Risiken
- [falls vorhanden]

## Iteration Count
[N]/3 — [welcher Gate-Durchlauf ist das?]
```

---

## FEATURE_SCORECARD Template

```markdown
# FEATURE_SCORECARD: [Feature Name]
> Orchestrator: Feature Lifecycle Orchestrator
> Date: [date]
> Goal: [GOAL.md link]

## Gate Results

| # | Gate | Agent | Status | Iterations | Evidence |
|---|------|-------|--------|------------|----------|
| 1 | PRD | Orchestrator | ✅ | 1 | PRD.md |
| 2 | UX Spec | UX Research & Usability Expert | ✅ | 1 | UX_SPEC.md |
| 3 | Design Spec | Visual Design Architect | ✅ | 1 | DESIGN_SPEC.md |
| 4 | Build | React & Next.js Frontend Builder | ✅ | 2 | Code + SYSTEM_STATE.md |
| 5 | Unit Tests | Component & Integration Test Writer | ✅ | 1 | 47/47 PASS |
| 6 | Auditor Review | Design & Accessibility Auditor | ✅ | 2 | REVIEW.md (0🔴 2🟡 1🟢) |
| 7 | E2E Tests | End-to-End Test Automation Agent | ✅ | 1 | TEST_REPORT.md (12/12 PASS) |
| 8 | UX Review | UX Research & Usability Expert | ✅ | 1 | USABILITY_REVIEW.md |
| 9 | VM Test | VM Integration Test Runtime | ⏭️ Skip | — | Web-only |
| 10 | Docs | Documentation & Knowledge Manager | ✅ | 1 | Updated |

## Summary
- Total Iterations: [N]
- Escalations: [0/N]
- Goal Alignment: ✅ All ACs met

## Confidence Score
[8/10] — [reason]

## Verdict: SHIP IT ✅ / NEEDS WORK 🔄
```

---

## Agent Dispatch Quick Reference

| Step | Agent | Dispatch Command | Expected Output |
|------|-------|-----------------|-----------------|
| 6 | UX Research & Usability Expert | "Define UX per PRD" | UX_SPEC.md |
| 7 | Visual Design Architect | "Design per PRD + UX_SPEC" | DESIGN_SPEC.md |
| 8 | React & Next.js Frontend Builder | "Build per DESIGN_SPEC + UX_SPEC" | Code |
| 9 | Component & Integration Test Writer | "Write tests for components" | Test files + PASS/FAIL |
| 10 | Design & Accessibility Auditor | "Review ultra-critical" | REVIEW.md |
| 11 | End-to-End Test Automation Agent | "E2E test with agent-browser" | TEST_REPORT.md |
| 12 | UX Research & Usability Expert | "Usability review post-build" | USABILITY_REVIEW.md |
| 13 | VM Integration Test Runtime | "VM test (if needed)" | VM_TEST_REPORT.md |
| 14 | Documentation & Knowledge Manager | "Update docs" | SYSTEM_STATE + CHANGELOG |
