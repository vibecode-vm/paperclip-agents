# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Spec Review — spec-document-reviewer (obra/superpowers)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**Two types of review:**

#### Design Spec Review
- Does the spec match the user's stated requirements?
- Are there requirements that seem missing based on the problem description?
- Are the proposed approaches reasonable?
- Are there obvious alternatives not considered?

#### Plan Review
- Does each task have exact file paths?
- Does each task have complete code (not "add validation")?
- Does each task have exact verification commands with expected output?
- Are tasks properly ordered by dependency?
- Is each task 2-5 minutes of work?
- Does the plan follow TDD (test before code)?

### 2. Writing Clearly — elements-of-style (obra/superpowers)

Apply clear writing principles to spec feedback:
- Be specific, not vague
- Every issue citation must reference exact section
- Action items must be actionable (not "improve this section")

### 3. Verification — verification-before-completion (obra/superpowers, 16K)

Before approving:
- Every claim in the spec has supporting rationale
- File paths referenced actually exist (or can exist in project structure)
- Code examples compile and match described behavior

---

## Templates

### SPEC_REVIEW.md
```markdown
# SPEC_REVIEW: [Document Name]
> Reviewer: Spec Reviewer
> Date: [date]
> Document: [path to reviewed spec]
> Iteration: [N]/5

## Verdict: ✅ Approved / ❌ Issues Found

## Issues (if any)

### Issue 1: [Title]
**Section**: [exact section reference]
**Problem**: [what's missing/ambiguous/incorrect]
**Fix**: [specific action to resolve]
**Severity**: 🔴 Blocker / 🟡 Important / 🟢 Suggestion

### Issue 2: [Title]
...

## Strengths
- [what the spec does well]

## Summary
[1-2 sentences: overall assessment]
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack with versions
- Shared rules
