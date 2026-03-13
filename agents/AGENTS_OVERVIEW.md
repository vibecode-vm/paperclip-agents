# AGENTS_OVERVIEW.md — Zentrale Agent-Übersicht

> Letzte Aktualisierung: 2026-03-13
> Insgesamt: **30 Agents** (24 Kern-Agents + 6 Skill-basierte Spezialisten)
> Methodik: [obra/superpowers](https://github.com/obra/superpowers) + [pbakaus/impeccable](https://github.com/pbakaus/impeccable)

---

## Agent-Hierarchie

```
                         ┌──────────────────┐
                         │       CEO        │
                         │ ceo/             │
                         └────────┬─────────┘
                                  │
                    ┌─────────────┴─────────────┐
                    │                           │
          ┌─────────┴──────────┐    ┌───────────┴───────────┐
          │   Product Owner    │    │   Architecture &      │
          │   product-owner/   │    │   Quality Gatekeeper  │
          └─────────┬──────────┘    │   architecture-       │
                    │               │   gatekeeper/          │
          ┌─────────┴──────────┐    └───────────┬───────────┘
          │   Scrum Master     │                │
          │   scrum-master/    │    ┌───────────┼───────────────────────────┐
          └────────────────────┘    │           │           │               │
                                   │           │           │               │
              ┌────────────────────┤     ┌─────┴──────┐   │    ┌──────────┴──────────┐
              │                    │     │ Spec       │   │    │ Security           │
              │     ┌──────────────┤     │ Reviewer   │   │    │ Auditor            │
              │     │              │     │ spec-      │   │    │ security-          │
              │     │              │     │ reviewer/  │   │    │ auditor/           │
              │     │              │     └────────────┘   │    └─────────────────────┘
              │     │              │                       │
    ┌─────────┴┐  ┌─┴──────────┐  ┌──────────┐  ┌────────┴──────────┐
    │ Stack    │  │ Docs       │  │ Code     │  │ DevOps           │
    │ Research.│  │ Manager    │  │ Quality  │  │ Expert           │
    │ stack-   │  │ docs-      │  │ Expert   │  │ devops-expert/   │
    │ research.│  │ manager/   │  │ c-q-e/   │  └────────┬─────────┘
    └──────────┘  └────────────┘  └──────────┘           │
                                              ┌──────────┼──────────┐
                                              │          │          │
                                        ┌─────┴────┐ ┌──┴───────┐ ┌┴─────────────┐
                                        │Kubernetes│ │Git Work- │ │Performance  │
                                        │Expert    │ │flow Mgr  │ │Optimizer    │
                                        │k8s-exp/  │ │git-wf/   │ │perf-opt/    │
                                        └──────────┘ └──────────┘ └──────────────┘

    ┌──────────────────────────────────────────────────────────────────────────┐
    │                    Feature Orchestrator (feature-orchestrator/)          │
    └──────────────────────────┬───────────────────────────────────────────────┘
                               │
        ┌──────────┬───────────┼──────────────┬───────────────┐
        │          │           │              │               │
  ┌─────┴───┐ ┌───┴──────┐ ┌──┴────────┐ ┌──┴──────────┐   │
  │ UX      │ │ Design   │ │Frontend   │ │ Backend     │   │
  │ Research│ │ Architect│ │Builder    │ │ Builder     │   │
  │ux-res/ │ │design-   │ │frontend-  │ │backend-     │   │
  └────┬────┘ │archit./ │ │builder/   │ │builder/     │   │
       │      └──────────┘ └───────────┘ └─────────────┘   │
       └── Post-Build Usability Review                     │
                                                           │
                                        ┌──────────────────┘
                                        │
                              ┌─────────┴──────────┐
                              │  QA Orchestrator   │
                              │  qa-orchestrator/  │
                              └──────────┬─────────┘
                                         │
         ┌──────────┬───────────┬────────┴──┬──────────┬──────────┐
         │          │           │           │          │          │
   ┌─────┴───┐ ┌───┴──────┐ ┌─┴────────┐ ┌┴────────┐ │  ┌──────┴───────┐
   │Unit     │ │Design   │ │E2E      │ │Testab. │ │  │Validation   │
   │Test     │ │Auditor  │ │Tester   │ │Expert  │ │  │Expert       │
   │Writer   │ │design-  │ │e2e-     │ │testab/ │ │  │validation-  │
   │unit-t-w/│ │auditor/ │ │tester/  │ └────────┘ │  │expert/      │
   └─────────┘ └─────────┘ └─────────┘       ┌────┴─────┐ └─────────────┘
                                              │VM Tester │
                                              │vm-tester/│
                                              └──────────┘

  Cross-Cutting Specialists:
    ├── Systematic Debugger (systematic-debugger/) — called by any agent
    ├── Accessibility Expert (accessibility-expert/) — called by Frontend/QA
    ├── Browser Automation (browser-automation/) — infrastructure service
    └── Event-System Expert (event-system-expert/) — called for Realtime features
```

---

## Alle 30 Agents

| # | Ordner | Codename | Squad | Berichtet an |
|---|--------|----------|-------|-------------|
| 1 | `ceo/` | Chief Strategy Officer | Leadership | Board |
| 2 | `product-owner/` | Product Owner | Leadership | CEO |
| 3 | `scrum-master/` | Sprint Commander | Leadership | Product Owner |
| 4 | `architecture-gatekeeper/` | Quality Gatekeeper | Leadership | CEO |
| 5 | `feature-orchestrator/` | Feature Orchestrator | Frontend Squad | Gatekeeper |
| 6 | `stack-researcher/` | Stack Researcher | Research | Gatekeeper |
| 7 | `ux-researcher/` | UX Researcher | Design Squad | Orchestrator |
| 8 | `design-architect/` | Visual Architect | Design Squad | Orchestrator |
| 9 | `frontend-builder/` | Frontend Builder | Build Squad | Orchestrator |
| 10 | `backend-builder/` | Backend Builder | Backend Squad | Gatekeeper |
| 11 | `qa-orchestrator/` | QA Manager | QA Squad | Orchestrator |
| 12 | `unit-test-writer/` | Test Writer | QA Squad | QA Manager |
| 13 | `design-auditor/` | Design Auditor | QA Squad | QA Manager |
| 14 | `e2e-tester/` | E2E Tester | QA Squad | QA Manager |
| 15 | `vm-tester/` | VM Tester | QA Squad | QA Manager |
| 16 | `docs-manager/` | Docs Manager | Docs | Gatekeeper |
| 17 | `browser-automation/` | Browser Runtime | Infrastructure | Service |
| 18 | `devops-expert/` | DevOps Expert | Infrastructure | Gatekeeper |
| 19 | `kubernetes-expert/` | K8s Expert | Infrastructure | DevOps Expert |
| 20 | `event-system-expert/` | Event Expert | Architecture | Gatekeeper |
| 21 | `api-schema-expert/` | API Expert | Architecture | Gatekeeper |
| 22 | `testability-expert/` | Testability Expert | QA Squad | QA Manager |
| 23 | `code-quality-expert/` | Code Quality Expert | Quality | Gatekeeper |
| 24 | `validation-expert/` | Validation Expert | QA Squad | QA Manager |
| **25** | **`systematic-debugger/`** | **Root Cause Hunter** 🆕 | Cross-Cutting | Any Agent |
| **26** | **`spec-reviewer/`** | **Quality Gate Keeper** 🆕 | Quality | Gatekeeper |
| **27** | **`git-workflow-manager/`** | **Branch Shepherd** 🆕 | Infrastructure | Gatekeeper |
| **28** | **`security-auditor/`** | **Red Team** 🆕 | Security | Gatekeeper |
| **29** | **`performance-optimizer/`** | **Speed Demon** 🆕 | Performance | Gatekeeper |
| **30** | **`accessibility-expert/`** | **A11y Guardian** 🆕 | Accessibility | QA Manager |

---

## Feature Lifecycle (Der Loop)

```
 1. BACKLOG.md    ← Product Owner (priorisiert)
 2. SPRINT_GOAL   ← Scrum Master + Product Owner
 3. GOAL.md       ← Gatekeeper
 4. SPEC REVIEW   ← Spec Reviewer (APPROVE oder REQUEST_CHANGES) 🆕
 5. PRD           ← Orchestrator
 6. API_SPEC      ← API Schema Expert (Contract-First)
 7. RESEARCH.md   ← Stack Researcher (Context7 + npm audit)
 8. UX_SPEC.md    ← UX Researcher
 9. DESIGN_SPEC   ← Design Architect
10. VALIDATION    ← Validation Expert (Zod Schemas + Security)
11. BRANCH SETUP  ← Git Workflow Manager (Worktree isolieren) 🆕
12. Code          ← Frontend Builder + Backend Builder
    └── DEBUG     ← Systematic Debugger (bei Bugs) 🆕
13. SECURITY SCAN ← Security Auditor (Pre-Deploy Gate) 🆕
14. PERF AUDIT    ← Performance Optimizer (Bundle + Vitals) 🆕
15. A11Y AUDIT    ← Accessibility Expert (WCAG 2.1 AA) 🆕
16. QA_REPORT     ← QA Orchestrator
    ├── Test Strategy (testability-expert)
    ├── Unit Tests (unit-test-writer)
    ├── Audit (design-auditor)
    ├── E2E (e2e-tester)
    ├── UX Review (ux-researcher)
    ├── Validation Review (validation-expert)
    └── VM Test (vm-tester, optional)
17. Quality Gate  ← Code Quality Expert
18. Docs Update   ← Docs Manager
19. BRANCH FINISH ← Git Workflow Manager (Merge/Cleanup) 🆕
20. Deploy        ← DevOps Expert → Kubernetes Expert
21. SCORECARD     ← Orchestrator → Gatekeeper → Product Owner (Acceptance)
```

---

## Squad-Übersicht

| Squad | Agents | Fokus |
|-------|--------|-------|
| **Leadership** (4) | CEO, Product Owner, Scrum Master, Architecture Gatekeeper | Vision, Backlog, Sprint, Qualitätsgates |
| **Architecture** (3) | Event-System Expert, API Schema Expert, Spec Reviewer 🆕 | Systemdesign, Contracts, Spec Review |
| **Design** (2) | UX Researcher, Design Architect | User Flows, Visual Design |
| **Build** (2) | Frontend Builder, Backend Builder | Code-Implementierung |
| **QA** (7) | QA Orchestrator, Unit Test Writer, Design Auditor, E2E Tester, VM Tester, Testability Expert, Validation Expert | Qualitätssicherung |
| **Quality** (2) | Code Quality Expert, Spec Reviewer 🆕 | Code-Metriken, Spec-Qualität |
| **Infrastructure** (4) | DevOps Expert, Kubernetes Expert, Browser Automation, Git Workflow Manager 🆕 | CI/CD, K8s, Git, Browser |
| **Security** (1) | Security Auditor 🆕 | OWASP, Pen Testing, Audit |
| **Performance** (1) | Performance Optimizer 🆕 | Core Web Vitals, Bundle Analysis |
| **Accessibility** (1) | Accessibility Expert 🆕 | WCAG 2.1, POUR Principles |
| **Cross-Cutting** (1) | Systematic Debugger 🆕 | 4-Phase Root Cause Analysis |
| **Research** (1) | Stack Researcher | Library-Evaluation, Context7 |
| **Docs** (1) | Docs Manager | Dokumentation, Knowledge Base |

---

## Skill-Quellen (17 Repositories)

| Repository | Skills | Installs |
|-----------|--------|----------|
| [obra/superpowers](https://github.com/obra/superpowers) | TDD, Debugging, Brainstorming, Plans, Verification, Subagent, Git, Code Review | 14K–50K |
| [vercel-labs/agent-skills](https://github.com/vercel-labs) | React Best Practices, Design Guidelines, Browser | 32K–199K |
| [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | OKLCH, 4pt Grid, Motion, AI Slop Test | — |
| [anthropics/skills](https://github.com/anthropics/skills) | Frontend Design, Canvas, Webapp Testing | 17K–145K |
| [supercent-io/agent-skills](https://github.com/supercent-io/agent-skills) | Security, Code Review, Design System, State | 8K–11K |
| [google-labs-code/stitch-skills](https://github.com/google-labs-code/stitch-skills) | React Components, Design | 12K–14K |
| [coreyhaines31/marketingskills](https://github.com/coreyhaines31/marketingskills) | SEO, Content, Launch | 15K–40K |
| [sleekdotdesign/agent-skills](https://github.com/sleekdotdesign/agent-skills) | Mobile App Design | 128K |
| [supabase/agent-skills](https://github.com/supabase/agent-skills) | Postgres Best Practices | 32K |
| [nextlevelbuilder/ui-ux-pro-max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | UI/UX Pro Max | 57K |
| [wshobson/agents](https://github.com/wshobson/agents) | TypeScript, Tailwind | 13K–17K |
| [better-auth/skills](https://github.com/better-auth/skills) | Auth Best Practices | 21K |
| [browser-use/browser-use](https://github.com/browser-use/browser-use) | Browser Automation | 48K |
| [currents-dev/playwright](https://github.com/currents-dev/playwright-best-practices-skill) | Playwright | 8K |
| [am-will/codex-skills](https://github.com/am-will/codex-skills) | Context7, Swarm Planner | 12K |
| [intopia.digital](https://intopia.digital) | Web Accessibility (WCAG) | — |
| [trailofbits.com](https://www.trailofbits.com) | Security Auditing | — |

> Vollständige Skill Registry mit Download-URLs: siehe `SHARED_CONFIG.md` → "Skill Registry — Download URLs"

---

## Agent Config Dateien

Jeder Agent hat 4 Dateien:
- `SOUL.md` — Persona, Anti-Patterns, Reasoning-Protokoll (CoT/ReAct/Self-Refinement)
- `AGENTS.md` — Rolle, Reporting Chain, Scope, Workflow
- `TOOLS.md` — Skills (inline), Templates, Commands, Download-URLs
- `HEARTBEAT.md` — Checkliste, Red Flags, Human Partner Signals

---

## Schlüssel-Prinzipien

### Kern-Prinzipien (alle 30 Agents)
1. **Goal Check (Step 0)** — Jeder Agent liest GOAL.md als erstes
2. **Evidence before claims** — Screenshots/Reports bevor "fertig" gesagt wird
3. **Circuit Breaker** — Max 3 Iterationen pro Gate, dann Eskalation
4. **HANDOFF Protocol** — Standardformat für Agent-zu-Agent Übergaben
5. **Mandated Stack** — Next.js 15, React 19, shadcn, Tailwind 4, Zustand, Zod = fix

### Superpowers-Methodik (obra/superpowers)
6. **TDD Iron Law** — Kein Produktionscode ohne failing test zuerst
7. **4-Phase Debugging** — Root Cause → Pattern → Hypothesis → Implementation
8. **HARD-GATE** — Kein Code ohne vorheriges Design-Approval
9. **Subagent-Driven Development** — Fresh Subagent per Task, Two-Stage Review
10. **Verification Evidence** — Command-Output beweist Behauptung

### Impeccable Design (pbakaus/impeccable)
11. **AI Slop Test** — 16-Punkte Anti-AI-Slop Checklist bei jedem Design Audit
12. **OKLCH + Tinted Neutrals** — Keine HEX/HSL, kein reines Schwarz/Weiß

### Advanced Reasoning (alle Agents)
13. **Chain-of-Thought** — Denkschritte zeigen bei komplexen Entscheidungen
14. **ReAct** — Abwechselnd Denken + Handeln bei explorativen Aufgaben
15. **Self-Refinement** — Eigene Arbeit kritisch prüfen vor Abgabe
16. **Error Recovery** — 3-Stufen-Eskalation (Selbst → Peer → Gatekeeper)

### Spezialisiert (neue Agents)
17. **Contract-First** — API Schema Expert definiert Contracts VOR Implementation
18. **Security Gate** — Security Auditor prüft OWASP Top 10 vor Deploy
19. **Performance Budget** — LCP < 2.5s, Bundle < 200KB, INP < 200ms
20. **WCAG 2.1 AA** — Accessibility Expert prüft 50+ POUR-Kriterien
