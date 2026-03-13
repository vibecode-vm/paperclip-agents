# REPORTING_PROTOCOL.md — Agent Progress & Status Reporting

> **PFLICHT für JEDEN Agent**: Bevor du eine Aufgabe startest oder abschließt, aktualisiere die relevanten Status-Dateien.
> **Warum**: Wenn ein Agent mid-task abstürzt, muss der nächste Agent sofort wissen was passiert ist.
> Letzte Aktualisierung: 2026-03-13 (30 Agents, neue Deliverables)

---

## 1. Feature-Ordner (pro Feature)

Jedes Feature bekommt einen eigenen Ordner unter `docs/features/`:

```
docs/features/<feature-name>/
├── GOAL.md                ← Acceptance Criteria (vom Gatekeeper)
├── STATUS.md              ← LIVE Status: wer arbeitet, was läuft, was fertig
├── SPEC_REVIEW.md         ← Spec Review Ergebnis (vom Spec Reviewer) 🆕
├── PRD.md                 ← Product Requirements (vom Orchestrator)
├── UX_SPEC.md             ← User Flows (vom UX Researcher)
├── DESIGN_SPEC.md         ← Design Tokens + Layout (vom Design Architect)
├── BRANCH_STATUS.md       ← Git Branch Lifecycle (vom Git Workflow Manager) 🆕
├── ROOT_CAUSE.md          ← Bug Root Cause (vom Systematic Debugger) 🆕
├── SECURITY_AUDIT.md      ← Security Findings (vom Security Auditor) 🆕
├── PERFORMANCE_REPORT.md  ← Performance Metrics (vom Performance Optimizer) 🆕
├── A11Y_AUDIT.md          ← Accessibility Findings (vom Accessibility Expert) 🆕
├── TEST_PLAN.md           ← Welche Tests, welche ACs abgedeckt (vom QA Orchestrator)
├── QA_REPORT.md           ← Test-Ergebnisse aller Layers (vom QA Orchestrator)
└── FEATURE_SCORECARD.md   ← Finale Bewertung (vom Orchestrator → Gatekeeper)
```

---

## 2. STATUS.md Template (pro Feature)

```markdown
# STATUS: [Feature Name]

> Erstellt: [timestamp]
> Zuletzt aktualisiert: [timestamp]
> Aktualisiert von: [Agent Name]

## Aktueller Schritt

| Feld | Wert |
|------|------|
| Phase | [Planning / Spec Review / UX / Design / Branch Setup / Build / Debug / Security / Performance / A11y / QA / Docs / Deploy / Review] |
| Verantwortlicher Agent | [Agent Name + Ordner] |
| Gestartet um | [timestamp] |
| Status | [🟢 Läuft / 🟡 Wartet / 🔴 Blockiert / ✅ Fertig] |
| Blocker | [keiner / Beschreibung] |

## Fortschritt

- [ ] GOAL.md geschrieben (Gatekeeper)
- [ ] SPEC_REVIEW.md — Spec approved (Spec Reviewer) 🆕
- [ ] PRD geschrieben (Feature Orchestrator)
- [ ] UX_SPEC.md geliefert (UX Researcher)
- [ ] DESIGN_SPEC.md geliefert (Design Architect)
- [ ] Branch erstellt + BRANCH_STATUS.md (Git Workflow Manager) 🆕
- [ ] Code gebaut + SYSTEM_STATE.md aktualisiert (Builder)
- [ ] ROOT_CAUSE.md bei Bugs (Systematic Debugger) 🆕
- [ ] SECURITY_AUDIT.md — keine CRITICALs (Security Auditor) 🆕
- [ ] PERFORMANCE_REPORT.md — Vitals im grünen Bereich (Performance Optimizer) 🆕
- [ ] A11Y_AUDIT.md — WCAG 2.1 AA bestanden (Accessibility Expert) 🆕
- [ ] Unit Tests PASS (Unit Test Writer)
- [ ] Design Audit bestanden (Design Auditor)
- [ ] E2E Tests PASS (E2E Tester)
- [ ] UX Review bestanden (UX Researcher)
- [ ] Dokumentation aktualisiert (Docs Manager)
- [ ] Branch merged + Cleanup (Git Workflow Manager) 🆕
- [ ] FEATURE_SCORECARD.md geliefert (Orchestrator)
- [ ] Gatekeeper Approval

## Änderungsprotokoll

| Zeit | Agent | Aktion | Ergebnis |
|------|-------|--------|----------|
| [timestamp] | Gatekeeper | GOAL.md erstellt | ✅ |
| [timestamp] | Spec Reviewer | Spec reviewed: APPROVED | ✅ |
| [timestamp] | Orchestrator | PRD geschrieben, an UX Researcher delegiert | 🟢 Läuft |
| [timestamp] | Git Workflow Manager | Branch feat/<name> erstellt | ✅ |
| | | | |

## Letzte Übergabe (HANDOFF)

Von: [Agent Name]
An: [Agent Name]
Deliverables: [was übergeben wurde]
Kontext: [was der Empfänger wissen muss]
```

---

## 3. TASK_LOG.md (Zentral im Projekt-Root)

Zentrale Übersicht ALLER aktiven, blockierten und abgeschlossenen Features:

```markdown
# TASK_LOG.md — Zentrale Feature-Übersicht

> Letzte Aktualisierung: [timestamp]

## 🟢 Aktive Features

| Feature | Phase | Agent | Gestartet | Letzte Aktivität | Docs |
|---------|-------|-------|-----------|-----------------|------|
| Auth Login | Build | Frontend Builder | 2026-03-12 09:00 | 2026-03-12 14:30 | docs/features/auth-login/ |
| Dashboard | QA | QA Orchestrator | 2026-03-11 | 2026-03-12 12:00 | docs/features/dashboard/ |

## 🔴 Blockierte Features

| Feature | Blockiert seit | Blocker | Verantwortlich | Docs |
|---------|---------------|---------|---------------|------|
| | | | | |

## ✅ Abgeschlossene Features

| Feature | Shipped | Dauer | Iterationen | Docs |
|---------|---------|-------|-------------|------|
| | | | | |
```

---

## 4. Reporting-Regeln pro Agent (alle 30)

### Wann updaten?

| Event | Was updaten | Wer |
|-------|-------------|-----|
| Feature gestartet | TASK_LOG.md + STATUS.md erstellen | Orchestrator |
| Arbeit an Feature beginnen | STATUS.md → "Aktueller Schritt" | Jeder Agent |
| Deliverable fertig | STATUS.md → Fortschritt + Änderungsprotokoll | Jeder Agent |
| Übergabe an nächsten Agent | STATUS.md → "Letzte Übergabe" | Absender |
| Feature blockiert | STATUS.md → Blocker + TASK_LOG.md → 🔴 | Blockierter Agent |
| Feature shipped | TASK_LOG.md → ✅ Abgeschlossen | Orchestrator |

### Was jeder Agent schreiben MUSS:

| Agent | Schreibt in STATUS.md | Liefert in Feature-Ordner |
|-------|----------------------|--------------------------|
| Gatekeeper | "GOAL.md erstellt" | `GOAL.md` |
| Spec Reviewer 🆕 | "SPEC_REVIEW: APPROVED / REQUEST_CHANGES" | `SPEC_REVIEW.md` |
| Orchestrator | "PRD geschrieben, delegiert an [Agent]" | `PRD.md` |
| Stack Researcher | "Research abgeschlossen" | `RESEARCH.md` |
| UX Researcher | "UX_SPEC geliefert" | `UX_SPEC.md` |
| Design Architect | "DESIGN_SPEC geliefert" | `DESIGN_SPEC.md` |
| Git Workflow Manager 🆕 | "Branch feat/<name> erstellt / merged" | `BRANCH_STATUS.md` |
| Frontend Builder | "Code gebaut, SYSTEM_STATE aktualisiert" | — (Code in src/) |
| Backend Builder | "API gebaut, DB migriert, SYSTEM_STATE aktualisiert" | `API_SPEC.md` |
| Systematic Debugger 🆕 | "Root Cause: [Beschreibung], Fix: [Beschreibung]" | `ROOT_CAUSE.md` |
| Security Auditor 🆕 | "Audit: X CRITICAL, Y HIGH, Z MEDIUM, W LOW" | `SECURITY_AUDIT.md` |
| Performance Optimizer 🆕 | "LCP: Xs, Bundle: XKB, CLS: X.XX" | `PERFORMANCE_REPORT.md` |
| Accessibility Expert 🆕 | "A11y: X Blocker, Y Major, Z Minor" | `A11Y_AUDIT.md` |
| QA Orchestrator | "TEST_PLAN erstellt, Tests dispatcht" | `TEST_PLAN.md`, `QA_REPORT.md` |
| Unit Test Writer | "Unit Tests: X PASS, Y FAIL" | — (Tests in src/) |
| Design Auditor | "Audit: X 🔴, Y 🟡, Z 🟢" | `REVIEW.md` |
| E2E Tester | "E2E: X PASS, Y FAIL" | `TEST_REPORT.md` |
| Docs Manager | "Docs aktualisiert" | — (Updates SYSTEM_STATE) |

---

## 5. Crash-Recovery

Wenn ein Agent abstürzt oder nicht antwortet:

1. **Orchestrator prüft STATUS.md** → "Wer hat zuletzt was gemacht?"
2. **Letzter HANDOFF** → Wo wurde das Feature übergeben?
3. **Fortschritts-Checkboxen** → Was ist schon fertig?
4. **Änderungsprotokoll** → Chronologische History
5. → Orchestrator re-dispatcht das Feature ab dem letzten ✅ Punkt

### Recovery-Entscheidungsbaum:

```
STATUS.md lesen → Letzter ✅ Schritt finden
  ├── Deliverable im Feature-Ordner? → Weiter ab nächstem Schritt
  ├── Kein Deliverable? → Schritt wiederholen
  └── Agent meldet Blocker? → Eskalation an Gatekeeper
```

---

## 6. Error Recovery (3-Stufen-Eskalation)

> Siehe auch: SHARED_CONFIG.md → Rule 8: Error Recovery Protocol

```
Stufe 1: SELBST LÖSEN (max 3 Versuche)
  └── Agent nutzt Systematic Debugger Skill (4-Phase)

Stufe 2: PEER FRAGEN (nach 3 gescheiterten Versuchen)
  └── Systematic Debugger Agent aufrufen → ROOT_CAUSE.md

Stufe 3: ESKALIEREN (nach 5 Minuten ohne Fortschritt)
  └── Status=BLOCKED → Gatekeeper/Orchestrator entscheidet
```
