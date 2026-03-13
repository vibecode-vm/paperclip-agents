# 🤖 Paperclip AI Agent System

> **30 spezialisierte KI-Agents** für die vollständige Software-Entwicklung — von der Idee bis zum Deploy.

[![Agents](https://img.shields.io/badge/Agents-30-blue)]() [![Skills](https://img.shields.io/badge/Skills-60+-green)]() [![Skill_Sources](https://img.shields.io/badge/Skill_Sources-17_Repos-orange)]() [![Lines](https://img.shields.io/badge/Config-12.9K_Lines-purple)]()

---

## ⚡ Was ist das?

Ein Multi-Agent-System in dem **30 KI-Agents** zusammenarbeiten, um Software zu planen, designen, bauen, testen, sichern und deployen. Jeder Agent hat eine eigene Persönlichkeit, Expertise und Arbeitsweise — definiert durch 4 Konfigurationsdateien.

**Kein generischer Chatbot. Jeder Agent ist ein Spezialist.**

```
User gibt Feature-Ziel → 30 Agents liefern produkt-fertige Software
```

---

## 🏗️ Architektur

```
                    ┌─────────┐
                    │   CEO   │ Strategic Vision
                    └────┬────┘
                         │
              ┌──────────┼──────────┐
              │                     │
     ┌────────┴────────┐   ┌───────┴──────────┐
     │ Product Owner   │   │ Architecture &   │
     │ + Scrum Master  │   │ Quality Gatekeeper│
     └────────┬────────┘   └───────┬──────────┘
              │                     │
     ┌────────┴────────┐   ┌───────┴──────────────────────────────────┐
     │ Spec Reviewer   │   │ Stack Researcher · Code Quality Expert  │
     │ (Pre-Code Gate) │   │ Docs Manager · DevOps · K8s Expert      │
     └─────────────────┘   │ Git Workflow Mgr · Security Auditor     │
                           └───────┬──────────────────────────────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    │   Feature Orchestrator      │
                    │   (Feature-Lifecycle-Engine) │
                    └────┬────────────────────┬───┘
                         │                    │
         ┌───────────────┤    ┌───────────────┤
         │               │    │               │
    ┌────┴────┐    ┌─────┴──┐│  ┌────────────┴┐
    │ UX      │    │ Design ││  │ Frontend    │
    │ Research│    │ Archit.││  │ Builder     │
    └─────────┘    └────────┘│  └─────────────┘
                        ┌────┴────────┐
                        │ Backend     │
                        │ Builder     │
                        └─────────────┘
                              │
              ┌───────────────┴───────────────┐
              │       QA Orchestrator         │
              │   (5-Layer Test Pipeline)     │
              └───┬───┬───┬───┬───┬───┬───┬───┘
                  │   │   │   │   │   │   │
                 UT  DA  E2E  VM  TE  VE  AE
```

**Legende**: UT=Unit Test Writer, DA=Design Auditor, E2E=E2E Tester, VM=VM Tester, TE=Testability Expert, VE=Validation Expert, AE=Accessibility Expert

---

## 👥 Alle 30 Agents

### 🔵 Leadership & Strategy (4)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| CEO | Chief Strategy Officer | Strategisches HARD-GATE, P0-P3 Decision Framework |
| Product Owner | Product Owner | Brainstorming, Bite-Sized Writing Plans |
| Scrum Master | Sprint Commander | Subagent-Driven Development, Sprint Cadence |
| Architecture Gatekeeper | Quality Gatekeeper | HARD-GATE, Design-for-Isolation, YAGNI |

### 🟢 Design & Research (3)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| UX Researcher | UX Researcher | Nielsen's 10 Heuristics, Optimistic UI |
| Design Architect | Visual Architect | OKLCH, 4pt Grid, AI Slop Test |
| Stack Researcher | Stack Researcher | Context7, Firecrawl, `skills search` |

### 🟡 Build (3)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| Feature Orchestrator | Feature Orchestrator | 21-Step Feature Lifecycle, Subagent Dispatch |
| Frontend Builder | Frontend Builder | TDD Iron Law, React 19, Impeccable Design |
| Backend Builder | Backend Builder | Contract-First TDD, Drizzle ORM, RLS |

### 🔴 Quality & Testing (8)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| QA Orchestrator | QA Manager | 5-Layer Test Pipeline, Parallel Dispatch |
| Unit Test Writer | Test Writer | RED-GREEN-REFACTOR, Vitest |
| Design Auditor | Design Auditor | 16-Point AI Slop Audit, 3 Viewports |
| E2E Tester | E2E Tester | Playwright, User Journey Testing |
| VM Tester | VM Tester | Native App Testing, Desktop Automation |
| Testability Expert | Testability Expert | 5 Testing Anti-Patterns |
| Validation Expert | Validation Expert | Zod Fortress, OWASP Input Validation |
| Code Quality Expert | Code Quality Expert | Pre-Review Checklist, SOLID |

### 🟣 Infrastructure (4)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| DevOps Expert | DevOps Expert | CI/CD, Docker, GitHub Actions |
| Kubernetes Expert | K8s Expert | Helm, Health Probes, NetworkPolicies |
| Browser Automation | Browser Runtime | agent-browser, Headless Chrome |
| Docs Manager | Docs Manager | Documentation-as-Code, Clear Writing |

### 🔶 Skill-Based Specialists (6) — NEU
| Agent | Codename | Deliverable | Superkraft |
|-------|----------|-------------|-----------|
| 🔍 Systematic Debugger | Root Cause Hunter | `ROOT_CAUSE.md` | 4-Phase Debugging, Chain-of-Thought |
| 📋 Spec Reviewer | Quality Gate Keeper | `SPEC_REVIEW.md` | 5-Category Review, Self-Refinement |
| 🌿 Git Workflow Manager | Branch Shepherd | `BRANCH_STATUS.md` | Worktree Lifecycle, Commit Discipline |
| 🔒 Security Auditor | Red Team | `SECURITY_AUDIT.md` | OWASP Top 10, ReAct Protocol |
| ⚡ Performance Optimizer | Speed Demon | `PERFORMANCE_REPORT.md` | Core Web Vitals, Bundle Analysis |
| ♿ Accessibility Expert | A11y Guardian | `A11Y_AUDIT.md` | WCAG 2.1 POUR (50+ Kriterien) |

### 🏛️ Architecture (2)
| Agent | Codename | Superkraft |
|-------|----------|-----------|
| Event-System Expert | Event Expert | Supabase Realtime, Idempotency |
| API Schema Expert | API Expert | Contract-First TDD, Zod Schemas |

---

## 📁 Dateistruktur

```
workspace/
├── SYSTEM_STATE.md          # Aktueller Systemzustand
├── MEMORY.md                # Gelernte Patterns & Gotchas
├── TASK_LOG.md              # Feature-Übersicht
│
└── agents/                  # ← DIESES VERZEICHNIS
    ├── README.md            # Diese Datei
    ├── AGENTS_OVERVIEW.md   # Alle 30 Agents im Überblick
    ├── SHARED_CONFIG.md     # Globale Regeln, Stack, Skill Registry
    ├── SKILLS.md            # Skill Discovery & extrahiertes Wissen
    ├── REPORTING_PROTOCOL.md # Status-Reporting Templates
    │
    └── [agent-name]/        # 30 Agent-Ordner, je 4 Dateien:
        ├── SOUL.md          # Persona, Anti-Patterns, Reasoning-Protokoll
        ├── AGENTS.md        # Rolle, Scope, Reporting Chain, Workflow
        ├── HEARTBEAT.md     # Checklisten, Red Flags, Human Partner Signals
        └── TOOLS.md         # Skills (inline), Commands, Templates
```

**Statistiken:**
| Metrik | Wert |
|--------|------|
| Agent-Ordner | 30 |
| Markdown-Dateien | 124 |
| Konfigurationszeilen | 12.986 |
| Skill-Quellen | 17 Repositories |
| Eingebettete Skills | 60+ |

---

## 🧠 Methodik

### Superpowers (obra/superpowers)
Die Kern-Methodik für alle 30 Agents:

| Prinzip | Beschreibung | Agents |
|---------|-------------|--------|
| 🔴 TDD Iron Law | Kein Code ohne failing test | 8 Agents |
| 🔍 4-Phase Debugging | Root Cause → Pattern → Hypothesis → Fix | 15 Agents |
| 🚫 HARD-GATE | Kein Code ohne Design-Approval | 4 Agents |
| ✅ Verification Evidence | Beweis vor Behauptung | 20 Agents |
| 👥 Subagent-Driven Dev | Fresh Subagent per Task, Two-Stage Review | 5 Agents |
| 🌿 Git Worktrees | Branch-Isolation via Worktrees | 4 Agents |

### Impeccable Design (pbakaus/impeccable)
Anti-AI-Slop-Design für Design Architect, Design Auditor, Frontend Builder:
- OKLCH Color System mit tinted neutrals
- 4pt Grid Spacing
- Exponential Easing (keine generischen Animationen)
- 16-Point AI Slop Audit

### Advanced Reasoning (alle Agents)
| Technik | Wann | Beispiel |
|---------|------|---------|
| Chain-of-Thought | Komplexe Entscheidungen | Debugger: 8-Schritt CoT |
| ReAct | Explorative Aufgaben | Security Auditor: Reason → Attack → Analyze |
| Self-Refinement | Vor jeder Abgabe | Spec Reviewer: 4-Fragen-Loop |
| Few-Shot | Lehr-Situationen | Accessibility Expert: Bad→Good Diffs |

---

## 🔧 Skill-Quellen

Skills kommen aus **17 Repositories** und werden auf 3 Arten geladen:

```bash
# 1. skills.sh CLI (empfohlen)
skills search "systematic-debugging"
skills install systematic-debugging

# 2. GitHub Raw Download
curl -sL https://github.com/obra/superpowers/tree/main/skills/systematic-debugging/SKILL.md

# 3. Skill Registry → SHARED_CONFIG.md
```

Top Skill-Repositories:
| Repository | Skills | Installs |
|-----------|--------|----------|
| obra/superpowers | 13 Skills (TDD, Debug, Plans, Review...) | 14K–50K |
| vercel-labs | 10 Skills (React, Design, Browser...) | 32K–199K |
| anthropics/skills | 4 Skills (Frontend Design, Testing...) | 17K–145K |
| pbakaus/impeccable | Design Aesthetics | — |
| + 13 weitere Repos | 30+ Skills | 8K–128K |

> Vollständige Skill Registry mit Download-URLs: `SHARED_CONFIG.md` → Abschnitt "Skill Registry"

---

## 🔄 Feature Lifecycle (21 Schritte)

```
 1. BACKLOG      ← Product Owner
 2. SPRINT_GOAL  ← Scrum Master
 3. GOAL.md      ← Gatekeeper
 4. SPEC REVIEW  ← Spec Reviewer          ← 🆕
 5. PRD          ← Feature Orchestrator
 6. API_SPEC     ← API Schema Expert
 7. RESEARCH     ← Stack Researcher
 8. UX_SPEC      ← UX Researcher
 9. DESIGN_SPEC  ← Design Architect
10. VALIDATION   ← Validation Expert
11. BRANCH SETUP ← Git Workflow Manager    ← 🆕
12. CODE         ← Frontend + Backend Builder
    └── DEBUG    ← Systematic Debugger     ← 🆕
13. SECURITY     ← Security Auditor        ← 🆕
14. PERFORMANCE  ← Performance Optimizer   ← 🆕
15. ACCESSIBILITY← Accessibility Expert    ← 🆕
16. QA REPORT    ← QA Orchestrator (5 Layers)
17. QUALITY GATE ← Code Quality Expert
18. DOCS         ← Docs Manager
19. BRANCH MERGE ← Git Workflow Manager    ← 🆕
20. DEPLOY       ← DevOps → Kubernetes Expert
21. SCORECARD    ← Orchestrator → PO (Acceptance)
```

---

## 🛡️ Schlüssel-Prinzipien

1. **Goal Check** — Jeder Agent liest GOAL.md als erstes
2. **Evidence before claims** — Beweise vor Behauptungen
3. **Circuit Breaker** — Max 3 Iterationen, dann Eskalation
4. **Error Recovery** — 3-Stufen: Selbst → Peer → Gatekeeper
5. **Anti-Hallucination** — STOP-CHECK-DELIVER vor jeder Abgabe
6. **Mandated Stack** — Next.js 15 · React 19 · shadcn · Tailwind 4 · Zustand · Zod
7. **Contract-First** — API Schema vor Implementation
8. **Security Gate** — OWASP Top 10 vor Deploy
9. **Performance Budget** — LCP < 2.5s, Bundle < 200KB
10. **WCAG 2.1 AA** — 50+ Accessibility-Kriterien

---

## 📚 Weitere Dokumentation

| Datei | Inhalt |
|-------|--------|
| [AGENTS_OVERVIEW.md](agents/AGENTS_OVERVIEW.md) | Vollständige Agent-Übersicht mit Hierarchie |
| [SHARED_CONFIG.md](agents/SHARED_CONFIG.md) | Globale Regeln, Stack, Skill Registry, Reasoning Techniques |
| [SKILLS.md](agents/SKILLS.md) | Skill Discovery & extrahiertes Wissen der Top 23 Skills |
| [REPORTING_PROTOCOL.md](agents/REPORTING_PROTOCOL.md) | Status-Reporting Templates |

---

## 🏗️ Tech Stack

| Layer | Technologie |
|-------|-------------|
| Framework | Next.js 15 (App Router) |
| UI | React 19 + shadcn/ui |
| Styling | Tailwind CSS 4 |
| State | Zustand + TanStack React Query |
| Validation | Zod |
| ORM | Drizzle ORM |
| Database | Supabase (PostgreSQL) |
| Testing | Vitest + Playwright |
| CI/CD | GitHub Actions + Docker |
| Orchestration | Kubernetes + Helm |
| Agent Runtime | OpenCode (Paperclip) |

---

*Built with [obra/superpowers](https://github.com/obra/superpowers) · [pbakaus/impeccable](https://github.com/pbakaus/impeccable) · [skills.sh](https://skills.sh)*
