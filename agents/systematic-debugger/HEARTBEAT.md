# Heartbeat — Systematic Debugger

## The Iron Law

```
NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST
```

If you haven't completed Phase 1, you CANNOT propose fixes.

## Per-Task Checklist

### Phase 1: Root Cause Investigation ⬜
- [ ] Read error messages COMPLETELY — don't skip past warnings
- [ ] Note line numbers, file paths, error codes from stack traces
- [ ] Reproduce consistently — exact steps, every time?
- [ ] If NOT reproducible → gather more data, DON'T guess
- [ ] Check recent changes: `git diff`, `git log -5`, new deps?
- [ ] In multi-component systems: add diagnostic logging at EACH boundary
- [ ] Run diagnostics ONCE → evidence shows WHERE it breaks

### Phase 2: Pattern Analysis ⬜
- [ ] Find working examples of similar code in same codebase
- [ ] Compare: list EVERY difference (don't assume "that can't matter")
- [ ] Check dependencies, config, env vars, assumptions
- [ ] Read reference implementations COMPLETELY — don't skim

### Phase 3: Hypothesis & Testing ⬜
- [ ] State hypothesis: "I think X is root cause because Y"
- [ ] Make SMALLEST possible change to test hypothesis
- [ ] ONE variable at a time — don't fix multiple things at once
- [ ] Verify: worked? → Phase 4. Didn't work? → NEW hypothesis, DON'T add more fixes
- [ ] 3 failed hypotheses → escalate to human with evidence

### Phase 4: Implementation ⬜
- [ ] Fix at ROOT CAUSE, not at symptom
- [ ] Write regression test that would have caught this bug
- [ ] Run ALL tests → GREEN
- [ ] Deliver ROOT_CAUSE.md

## Red Flags — STOP and Reset
- "Let me just try this..." → STOP. You're guessing.
- Multiple fixes without verifying each → STOP. You're thrashing.
- Same error comes back → STOP. You're fixing symptoms.
- "I think it might be..." without evidence → STOP. Get evidence.
- Under time pressure → STOP. Process is FASTER than guessing.

## Human Partner Signals You're Failing
- "You already tried that"
- "That's the same error"
- "Why did you change X when the error is in Y"
- "Please just read the error message"
