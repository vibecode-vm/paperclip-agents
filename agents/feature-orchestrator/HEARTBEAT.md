# HEARTBEAT.md -- Feature Lifecycle Orchestrator Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.
- If `PAPERCLIP_TASK_ID` is set, prioritize that task.

## 3. Goal Check (MANDATORY)
- Read `../SHARED_CONFIG.md` — mandated stack, shared rules, project structure.
- Read `../../MEMORY.md` — known gotchas, architecture decisions.
- Read `../../SYSTEM_STATE.md` — current system state.
- Read `GOAL.md` for the current feature.
- Verify: acceptance criteria, scope boundaries, dependencies.
- If GOAL.md missing → request from Architecture & Quality Gatekeeper.

## 4. Routing Decision
- Is this Frontend, Backend, or Full-Stack?
- Frontend → manage with your squad (Step 5–16)
- Backend → delegate to API & Database Backend Builder via Gatekeeper
- Full-Stack → split into FE + BE subtasks, coordinate both

---

## THE FEATURE LOOP

### Step 4.5 Create Feature Tracking (MANDATORY before any work)
- Create `docs/features/<feature-name>/` folder
- Create `docs/features/<feature-name>/STATUS.md` using template from `../REPORTING_PROTOCOL.md`
- Add entry to `../../TASK_LOG.md` under 🟢 Aktive Features
- All deliverables from this point go into the feature folder

### Step 5. Write PRD
- Use PRD template from TOOLS.md.
- Define acceptance criteria for EVERY user story.
- Specify design requirements (aesthetic, responsive, dark mode).

### Step 6. → UX Research & Usability Expert
- **Dispatch**: "Define UX for [feature] per PRD"
- **Attach**: PRD + GOAL.md
- **HANDOFF format**: Use HANDOFF template from TOOLS.md
- **Expected output**: `UX_SPEC.md` (user flows, IA, interaction patterns, accessibility requirements)
- **Wait** for delivery before proceeding.

### Step 7. → Visual Design Architect
- **Dispatch**: "Design [feature] per PRD + UX_SPEC.md"
- **Attach**: PRD + UX_SPEC.md + GOAL.md
- **Expected output**: `DESIGN_SPEC.md`
- **Wait** for delivery.

### Step 8. → React & Next.js Frontend Builder
- **Dispatch**: "Build [feature] per DESIGN_SPEC.md + UX_SPEC.md"
- **Attach**: DESIGN_SPEC.md + UX_SPEC.md + GOAL.md
- **Expected output**: Code + `SYSTEM_STATE.md` update
- **Track iteration count**: reset to 0

### Step 8.5 🛡️ SCOPE VALIDATION GATE (MANDATORY before QA)

> **Zweck**: Verhindert dass der Builder Features halluziniert oder Scope Creep einbaut.
> **Wer**: Du (Orchestrator) selbst — kein separater Agent nötig.
> **Wann**: NACH Build, VOR QA. Kein Code geht in QA ohne diese Prüfung.

**Prüfung — Zeile für Zeile gegen GOAL.md + PRD:**

```
Für JEDES Acceptance Criterion in GOAL.md:
  ├── ✅ Umgesetzt wie beschrieben? → OK
  ├── ❌ Nicht umgesetzt? → REJECT: "AC #X fehlt"
  └── ⚠️ Anders umgesetzt als beschrieben? → REJECT: "AC #X weicht ab"

Für JEDEN gebauten Screen/Component/Route:
  ├── ✅ Im PRD/GOAL.md gefordert? → OK
  └── 🔴 NICHT im PRD/GOAL.md? → SCOPE CREEP: "Entferne [X], nicht im Scope"
```

**Ergebnis:**

| Ergebnis | Aktion |
|----------|--------|
| ✅ Alle ACs umgesetzt, kein Extra-Code | → Weiter zu Step 9 (QA) |
| ❌ ACs fehlen | → Zurück an Builder: "Baue AC #X, #Y" |
| 🔴 Scope Creep entdeckt | → Zurück an Builder: "Entferne [X], [Y] — nicht im GOAL.md" |
| ⚠️ Ablauf anders als PRD | → Zurück an Builder: "AC #X sagt [A], Code macht [B]" |

**Anti-Hallucination Regeln:**
1. Builder darf NUR bauen was in GOAL.md + PRD steht
2. Keine "Verbesserungen" oder "nice-to-haves" ohne expliziten AC
3. Keine extra Routes, Components, oder Features die nicht gefordert sind
4. UI muss DESIGN_SPEC.md folgen — keine kreativen Abweichungen
5. Max 3 Scope-Validation-Iterationen → dann Eskalation an Gatekeeper

**Update STATUS.md**: "Scope Validation: [PASS/FAIL] — [Details]"

### Step 9. → Frontend Quality Assurance Manager
- **Dispatch**: "QA test [feature] — all layers"
- **Attach**: PRD + DESIGN_SPEC.md + UX_SPEC.md + build URL + source files
- **HANDOFF format**: Use HANDOFF template from TOOLS.md
- **The QA Manager will**:
  1. Create TEST_PLAN.md (which layers, which ACs covered)
  2. Dispatch to sub-agents: Unit Tests + Auditor (parallel) → E2E → UX Review → VM (if needed)
  3. Manage Circuit Breaker (max 3 iterations per sub-agent)
  4. Collect all results into `QA_REPORT.md`
- **Expected output**: `QA_REPORT.md` (unified report from all test layers)
- **If QA FAIL**:
  ```
  QA Manager sends failed layers back to Builder via you
  After Builder fixes → QA Manager re-tests only failed layers
  If 3 iterations per layer → QA Manager escalates to Gatekeeper
  ```
- **If QA PASS** → proceed to Step 10

### Step 10. → Documentation & Knowledge Manager (`docs-manager/`)
- **Dispatch**: "Update docs for [feature]"
- **Attach**: All deliverables (specs, test reports, code changes)
- **Expected output**: Updated SYSTEM_STATE.md, CHANGELOG.md, component docs
- **Update STATUS.md**: "Docs aktualisiert" + Fortschritt ✅

### Step 11. Auto-Close Check
```
ALL must be ✅:
✅ Scope Validation: PASS (Step 8.5 — keine Hallucination/Scope Creep)
✅ Unit Tests: PASS (Component & Integration Test Writer)
✅ Auditor: no 🔴 Blockers (Design & Accessibility Auditor)
✅ E2E Tests: PASS (End-to-End Test Automation Agent)
✅ UX Review: no 🔴 Critical (UX Research & Usability Expert)
✅ VM Tests: PASS or Skipped
✅ SYSTEM_STATE.md: updated
✅ Documentation: updated
```

If ANY gate is not ✅ → Do NOT proceed. Fix the failing gate.

### Step 12. → Report to Architecture & Quality Gatekeeper (`architecture-gatekeeper/`)
- Deliver `FEATURE_SCORECARD.md` using template from TOOLS.md.
- Include: all gate results, iteration counts, total time, all evidence links.
- **Update STATUS.md**: Alle Fortschritts-Checkboxen ✅, Phase = "Review"
- **Update TASK_LOG.md**: Feature → ✅ Abgeschlossen (nach Gatekeeper Approval)
- Gatekeeper makes final SHIP / REJECT decision.

---

## Circuit Breaker Protocol

```
Gate fails → iteration_count += 1
  ├── iteration_count < 3 → zurück zu Builder
  └── iteration_count >= 3 → 🔴 ESKALATION
        → Create issue for Architecture & Quality Gatekeeper:
          "ESCALATION: [Feature] stuck at [Gate] after 3 iterations"
          Include: all 3 failure reports, root cause hypothesis
        → STOP working on this feature until Gatekeeper responds
```

---

## Rules
- GOAL.md before any work starts.
- Evidence at EVERY gate (specs, screenshots, reports).
- Circuit Breaker after 3 iterations per gate.
- No feature ships without ALL gates ✅.
- **Scope Validation Gate ist PFLICHT** — kein Code geht in QA ohne AC-Prüfung.
- **Builder darf NUR bauen was im GOAL.md steht** — keine Extras, keine "Verbesserungen".
- Always use HANDOFF format for agent-to-agent transitions.
- Always include `X-Paperclip-Run-Id` on mutating API calls.
