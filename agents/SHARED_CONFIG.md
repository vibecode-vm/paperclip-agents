# SHARED_CONFIG.md — Zentrale Konfiguration für alle Agents

> Dieses File wird von JEDEM Agent gelesen. Änderungen hier betreffen alle.
> Letzte Aktualisierung: 2026-03-13 (Skill Registry, Advanced Reasoning, Error Recovery, 30 Agents)

---

## Runtime
- **OpenCode** ist die Agent-Runtime.
- Alle Agents haben Shell-Zugriff, Internet-Zugriff, und MCP-Server Zugriff.

## Paperclip API

| Endpoint | Method | Zweck |
|----------|--------|-------|
| `/api/agents/me` | GET | Agent-Identität |
| `/api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` | GET | Aufgaben-Inbox |
| `/api/issues/{issueId}` | PATCH | Status-Updates (inkl. `X-Paperclip-Run-Id` Header) |
| `/api/companies/{companyId}/issues` | POST | Subtasks erstellen (mit `parentId` + `goalId`) |

---

## Mandated Stack

| Layer | Technologie | Referenz |
|-------|-------------|----------|
| Framework | Next.js 15 (App Router) | `/vercel/next.js` |
| UI Library | React 19 | `/facebook/react` |
| UI Components | shadcn/ui | `/shadcn-ui/ui` |
| Styling | Tailwind CSS 4 | `/tailwindlabs/tailwindcss` |
| State (Client) | Zustand (mit persist + devtools) | `/pmndrs/zustand` |
| State (Server) | TanStack React Query | `/TanStack/query` |
| Validation | Zod | `/colinhacks/zod` |
| Forms | React Hook Form + Zod Resolver | `/react-hook-form/react-hook-form` |
| Icons | lucide-react | — |
| Animation | Framer Motion | — |
| HTTP Client | ky | — |
| Toast | Sonner | — |

> Die Referenz-Spalte sind Context7 Library IDs — für aktuelle Docs.
> Dieser Stack ist **NICHT verhandelbar**. Optimize WITHIN it.

### Backend Stack

| Layer | Technologie | Referenz |
|-------|-------------|----------|
| ORM | Drizzle ORM | `/drizzle-team/drizzle-orm` |
| Migrations | drizzle-kit | — |
| Database | Supabase (PostgreSQL) | `/supabase/supabase` |
| Auth | Supabase Auth / better-auth | `/supabase/supabase` |
| SSR Auth | @supabase/ssr | — |
| Logging | pino | — |
| IDs | nanoid | — |

> Backend Stack gilt für alle API/DB Arbeiten. Wird vom Backend Builder durchgesetzt.

### Test Coverage Targets (Quality Gate)

| Metrik | Minimum | Empfohlen |
|--------|---------|----------|
| Statements | 80% | 90% |
| Branches | 75% | 85% |
| Functions | 80% | 90% |
| Lines | 80% | 90% |

> Coverage wird mit `npx vitest run --coverage` gemessen.
> Features dürfen NICHT shippen wenn Coverage unter Minimum fällt.
> QA Orchestrator prüft Coverage als Teil des QA_REPORT.md.

---

## MCP Server

### Context7 — Aktuelle Library-Dokumentation
```json
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp"]
  }
}
```
**Tools:** `resolve-library-id`, `get-library-docs`
**Wann:** Vor jeder Implementierung aktuelle Docs fetchen → speichern in `docs/lib/`

### Firecrawl — Web Scraping & Crawling
```json
{
  "firecrawl": {
    "command": "npx",
    "args": ["-y", "firecrawl-mcp"],
    "env": { "FIRECRAWL_API_KEY": "<key>" }
  }
}
```
**Auto-Install:** `which firecrawl || npm install -g firecrawl`

---

## Shared Rules (für ALLE Agents)

### 0. 🔍 Context Discovery (ALLERERSTER Schritt — vor JEDER Arbeit)

> **Bevor du irgendetwas tust: Lies was schon da ist.**
> **Wenn eine Datei fehlt: Erstelle sie aus dem Template.**

```
STEP 0 — CONTEXT DISCOVERY (PFLICHT für jeden Agent bei jedem Start)

1. Prüfe: Existiert `../../SYSTEM_STATE.md`?
   ├── JA → LESEN. Du weißt jetzt was im System existiert.
   └── NEIN → Erstelle aus Template (siehe SHARED_CONFIG Projekt-Ordnerstruktur)

2. Prüfe: Existiert `../../MEMORY.md`?
   ├── JA → LESEN. Du kennst jetzt bekannte Gotchas und Patterns.
   └── NEIN → Erstelle aus Template.

3. Prüfe: Existiert `../../TASK_LOG.md`?
   ├── JA → LESEN. Du weißt was aktiv, blockiert oder fertig ist.
   └── NEIN → Erstelle aus Template.

4. Prüfe: Existiert `docs/features/<aktuelles-feature>/STATUS.md`?
   ├── JA → LESEN. Du weißt wo das Feature steht und wer zuletzt was gemacht hat.
   └── NEIN → Feature ist neu, Orchestrator erstellt den Ordner.

5. Prüfe: Existiert `docs/lib/`?
   ├── JA → Prüfe ob Docs für deine Libraries vorhanden sind. LESEN.
   └── NEIN → Ordner erstellen, Stack Researcher um Context7 Docs bitten.

6. Lese `../SHARED_CONFIG.md` — Stack, Rules, MCP Server.
7. Lese `../SKILLS.md` — verfügbare Skills und extrahiertes Wissen.
8. Lese `../REPORTING_PROTOCOL.md` — wie du Status reportest.
```

**Warum das wichtig ist:**
- Agent startet → findet vorherigen State → macht WEITER statt von vorne
- Agent crashed → nächster Agent liest STATUS.md → übernimmt
- Kein Agent arbeitet jemals "blind" ohne Kontext

### 1. Goal Check (MANDATORY Step 0)
Jeder Agent liest als ERSTES die GOAL.md des Features. Alle Aktionen müssen dem Goal dienen.

### 2. Evidence before Claims
Kein Agent darf "fertig" sagen ohne Beweis:
- Builder: Tests müssen PASS zeigen
- Tester: Screenshots + Logs
- Auditor: REVIEW.md mit Befund
- Docs: Alle Links verifiziert

### 3. Circuit Breaker
Max 3 Iterationen pro Gate. Danach → Eskalation an Architecture Gatekeeper.

### 4. HANDOFF Protocol
Standard-Format für Agent-zu-Agent Übergaben:
```markdown
# HANDOFF: [From Agent] → [To Agent]
Feature: [name]
Goal: [GOAL.md link]
Deliverables: [was übergeben wird]
Context: [was der Empfänger wissen muss]
Documentation: [docs/lib/ Referenzen]
```

### 5. Skill Loading & Discovery

> **Jeder Agent hat Skills in seiner TOOLS.md referenziert. Hier steht, WIE man diese Skills lädt.**

#### Methode 1: skills.sh CLI (empfohlen)

```bash
# Skills suchen
skills search "systematic-debugging"
skills search "test-driven-development"
skills search "react best practices"

# Skill installieren (lädt SKILL.md in den Agent-Kontext)
skills install <skill-name>

# Alle verfügbaren Skills listen
skills list
```

#### Methode 2: GitHub Raw Download

Falls `skills` CLI nicht verfügbar — direkt von GitHub laden:

```bash
# Superpowers Skills (obra/superpowers)
curl -sL https://raw.githubusercontent.com/obra/superpowers/main/skills/<skill-name>/SKILL.md

# Beispiel:
curl -sL https://raw.githubusercontent.com/obra/superpowers/main/skills/systematic-debugging/SKILL.md
curl -sL https://raw.githubusercontent.com/obra/superpowers/main/skills/test-driven-development/SKILL.md
curl -sL https://raw.githubusercontent.com/obra/superpowers/main/skills/brainstorming/SKILL.md
```

#### Methode 3: MCP Server (für Tool-Integration)

```json
{
  "context7": {
    "command": "npx",
    "args": ["-y", "@upstash/context7-mcp"]
  },
  "firecrawl": {
    "command": "npx",
    "args": ["-y", "firecrawl-mcp"],
    "env": { "FIRECRAWL_API_KEY": "<key>" }
  }
}
```

---

### Skill Registry — Download URLs

> Alle Skills die in Agent TOOLS.md referenziert werden, mit Download-Quelle.

#### obra/superpowers (GitHub)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `systematic-debugging` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/systematic-debugging) | 28K | Systematic Debugger, alle Builder, alle Tester |
| `test-driven-development` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/test-driven-development) | 23K | Unit Test Writer, Backend Builder, Frontend Builder |
| `brainstorming` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/brainstorming) | 50K | Product Owner, Architecture Gatekeeper |
| `writing-plans` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/writing-plans) | 26K | Feature Orchestrator, QA Orchestrator |
| `executing-plans` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/executing-plans) | 22K | Feature Orchestrator, QA Orchestrator |
| `subagent-driven-development` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/subagent-driven-development) | 34K | Scrum Master, Feature Orchestrator, QA Orchestrator |
| `verification-before-completion` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/verification-before-completion) | 16K | ALLE Agents |
| `requesting-code-review` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/requesting-code-review) | 21K | Code Quality Expert, Feature Orchestrator |
| `receiving-code-review` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/receiving-code-review) | 17K | Code Quality Expert, alle Builder |
| `using-git-worktrees` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/using-git-worktrees) | 14K | Git Workflow Manager, Frontend Builder, DevOps Expert |
| `finishing-a-development-branch` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/finishing-a-development-branch) | 14K | Git Workflow Manager, DevOps Expert |
| `dispatching-parallel-agents` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/dispatching-parallel-agents) | 15K | Feature Orchestrator, QA Orchestrator |
| `using-superpowers` | [SKILL.md](https://github.com/obra/superpowers/tree/main/skills/using-superpowers) | 20K | ALLE Agents (Meta-Framework) |

#### skills.sh / Vercel Labs

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `vercel-react-best-practices` | `skills install vercel-react-best-practices` | 197K | Frontend Builder, Performance Optimizer |
| `web-design-guidelines` | `skills install web-design-guidelines` | 155K | Design Auditor |
| `frontend-design` | `skills install frontend-design` | 145K | Design Architect |
| `agent-browser` | `skills install agent-browser` | 89K | E2E Tester, Browser Automation, Design Auditor |
| `next-best-practices` | `skills install next-best-practices` | 32K | Frontend Builder |
| `ui-ux-pro-max` | `skills install ui-ux-pro-max` | 57K | Design Architect, UX Researcher |
| `webapp-testing` | `skills install webapp-testing` | 22K | E2E Tester, QA Orchestrator |
| `find-skills` | `skills install find-skills` | #1 | Stack Researcher, Architecture Gatekeeper |
| `context7` | `skills install context7` | 12K | Stack Researcher, API Schema Expert |
| `playwright-best-practices` | `skills install playwright-best-practices` | 8K | E2E Tester |

#### Anthropic (GitHub: `anthropics/skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `frontend-design` | `skills install frontend-design` / [GitHub](https://github.com/anthropics/skills) | 145K | Design Architect, Frontend Builder |
| `canvas-design` | `skills install canvas-design` | 17K | Design Architect |
| `brand-guidelines` | `skills install brand-guidelines` | 12K | Design Architect |
| `webapp-testing` | `skills install webapp-testing` | 22K | E2E Tester, QA Orchestrator, Unit Test Writer |

#### Supercent-io (GitHub: `supercent-io/agent-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `security-best-practices` | `skills install security-best-practices` / [GitHub](https://github.com/supercent-io/agent-skills) | 11K | Security Auditor, Validation Expert, Stack Researcher |
| `code-review` | `skills install code-review` | 11K | Stack Researcher, Backend Builder |
| `frontend-design-system` | `skills install frontend-design-system` | 8K | Design Architect, Frontend Builder |
| `ui-component-patterns` | `skills install ui-component-patterns` | 10K | Frontend Builder |
| `state-management` | `skills install state-management` | 10K | Frontend Builder |
| `responsive-design` | `skills install responsive-design` | 10K | Frontend Builder |

#### Nextlevelbuilder (GitHub: `nextlevelbuilder/ui-ux-pro-max-skill`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `ui-ux-pro-max` | `skills install ui-ux-pro-max` / [GitHub](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) | 57K | Design Architect, UX Researcher, Design Auditor |

#### Google Labs (GitHub: `google-labs-code/stitch-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `react:components` | `skills install react:components` / [GitHub](https://github.com/google-labs-code/stitch-skills) | 14K | Frontend Builder |
| `design-md` | `skills install design-md` | 12K | Design Architect |
| `stitch-loop` | `skills install stitch-loop` | 12K | Feature Orchestrator |

#### Coreyhaines31 (GitHub: `coreyhaines31/marketingskills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `seo-audit` | `skills install seo-audit` / [GitHub](https://github.com/coreyhaines31/marketingskills) | 39K | Design Auditor |
| `content-strategy` | `skills install content-strategy` | 21K | Feature Orchestrator |
| `launch-strategy` | `skills install launch-strategy` | 17K | Feature Orchestrator |
| `competitor-alternatives` | `skills install competitor-alternatives` | 16K | Stack Researcher |
| `copywriting` | `skills install copywriting` | 15K | Docs Manager |

#### Sleekdotdesign (GitHub: `sleekdotdesign/agent-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `sleek-design-mobile-apps` | `skills install sleek-design-mobile-apps` / [GitHub](https://github.com/sleekdotdesign/agent-skills) | 128K | Design Architect |

#### Supabase (GitHub: `supabase/agent-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `supabase-postgres-best-practices` | `skills install supabase-postgres-best-practices` / [GitHub](https://github.com/supabase/agent-skills) | 32K | Backend Builder, Event System Expert |

#### Better Auth (GitHub: `better-auth/skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `better-auth-best-practices` | `skills install better-auth-best-practices` / [GitHub](https://github.com/better-auth/skills) | 21K | Backend Builder |

#### Browser-Use (GitHub: `browser-use/agent-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `browser-use` | `skills install browser-use` / [GitHub](https://github.com/browser-use/browser-use) | 48K | E2E Tester, Browser Automation |

#### Wshobson (GitHub: `wshobson/agents`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `typescript-advanced-types` | `skills install typescript-advanced-types` / [GitHub](https://github.com/wshobson/agents) | 13K | Validation Expert, Frontend Builder |
| `tailwind-design-system` | `skills install tailwind-design-system` | 17K | Frontend Builder |

#### GitHub Awesome Copilot (GitHub: `github/awesome-copilot`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `git-commit` | `skills install git-commit` / [GitHub](https://github.com/awesome-copilot/awesome-copilot) | 13K | Alle Agents die committen |
| `documentation-writer` | `skills install documentation-writer` | 8K | Docs Manager |
| `chrome-devtools` | `skills install chrome-devtools` | 8K | E2E Tester, VM Tester |

#### Currents-dev (GitHub: `currents-dev/playwright-best-practices-skill`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `playwright-best-practices` | `skills install playwright-best-practices` / [GitHub](https://github.com/currents-dev/playwright-best-practices-skill) | 8K | E2E Tester |

#### Am-Will (GitHub: `am-will/codex-skills`)

| Skill | Download | Installs | Genutzt von |
|-------|----------|----------|-------------|
| `context7` | `skills install context7` / [GitHub](https://github.com/am-will/codex-skills) | 12K | Stack Researcher, API Schema Expert |
| `swarm-planner` | `skills install swarm-planner` | 12K | Architecture Gatekeeper |
| `find-skills` | `skills install find-skills` | #1 | Stack Researcher |

#### Intopia (Web Accessibility)

| Skill | Download | Genutzt von |
|-------|----------|-------------|
| `web-accessibility` | [Intopia Digital](https://intopia.digital) — WCAG 2.1 Guidelines | Accessibility Expert, Frontend Builder, E2E Tester |

#### Trail of Bits (Security)

| Skill | Download | Genutzt von |
|-------|----------|-------------|
| `trail-of-bits-security` | `skills install trail-of-bits-security` / [Trail of Bits](https://www.trailofbits.com) | Security Auditor, Validation Expert |

#### pbakaus/impeccable (GitHub)

| Skill | Download | Genutzt von |
|-------|----------|-------------|
| Design Aesthetics (OKLCH, 4pt Grid, Motion, AI Slop Test) | [GitHub](https://github.com/pbakaus/impeccable) | Design Architect, Design Auditor, Frontend Builder, UX Researcher |

> **Nutzungshinweis**: Die Kernmethodik jedes Skills ist **bereits inline** in der jeweiligen Agent-TOOLS.md eingebettet. Die Download-URLs dienen zum **vollständigen Nachlesen** der Originaldokumentation oder zum **Aktualisieren** auf neuere Versionen.

### Skill Priority (obra/using-superpowers)
1. **User Instructions** (AGENTS.md, direkte Requests) → HÖCHSTE Priorität
2. **Skills** (skills.sh Patterns) → Override System-Defaults
3. **System Prompt** → NIEDRIGSTE Priorität

### 6. State Tracking
- Nach JEDEM Build: `SYSTEM_STATE.md` im Projekt-Root updaten.
- Vor JEDEM Feature: `SYSTEM_STATE.md` lesen um den aktuellen Stand zu kennen.

### 7. Advanced Reasoning Techniques (für ALLE Agents)

> Jeder Agent SOLL diese Techniken anwenden, wenn die Aufgabe Komplexität erfordert.

#### Chain-of-Thought (CoT)
Bei komplexen Entscheidungen: **DENKSCHRITTE ZEIGEN**, nicht nur das Ergebnis.
```
1. Problem verstehen → Was genau soll gelöst werden?
2. Constraints identifizieren → Was sind die Grenzen?
3. Optionen auflisten → Welche Ansätze gibt es?
4. Bewerten → Welche Option passt am besten? WARUM?
5. Entscheiden → Gewählter Ansatz MIT Begründung
```

#### ReAct (Reasoning + Acting)
Für explorative Aufgaben: **Abwechselnd DENKEN und HANDELN**.
```
REASON: Was muss ich herausfinden?
ACT:    Datei lesen / Command ausführen / API aufrufen
REASON: Was sagt das Ergebnis?
ACT:    Nächste Aktion basierend auf dem Ergebnis
```

#### Self-Refinement Loop
Nach JEDER Abgabe: **Eigene Arbeit kritisch prüfen**.
```
1. Ist mein Output vollständig? (alle ACs abgedeckt?)
2. Ist mein Output korrekt? (keine Fehler, keine Lücken?)
3. Ist mein Output minimal? (nichts Überflüssiges?)
4. Würde ICH diesen Output akzeptieren, wenn ich der Empfänger wäre?
```

#### Few-Shot Examples
Wenn möglich: **Beispiele von "Good vs Bad" zeigen**.
```diff
# Bad
-"Handle errors appropriately"

# Good
+"Show toast notification with error.message for 5 seconds, log full error to Sentry, 
+ set form field border to red, display inline error text below the field"
```

### 8. Error Recovery Protocol (für ALLE Agents)

> Wenn ein Agent auf einen Fehler stößt, der seine Arbeit blockiert.

```
ERROR RECOVERY (3-Stufen-Eskalation):

Stufe 1: SELBST LÖSEN (max 3 Versuche)
  ├── Fehler analysieren (4-Phase Debugging aus Superpowers)
  ├── Hypothese testen
  └── Fix verifizieren

Stufe 2: PEER FRAGEN (nach 3 gescheiterten Versuchen)
  ├── Systematic Debugger einschalten
  ├── Oder: Agent mit relevanter Expertise fragen
  └── Kontext übergeben: Fehler, was versucht wurde, Hypothesen

Stufe 3: ESKALIEREN (nach 5 Minuten ohne Fortschritt)
  ├── Status auf BLOCKED setzen
  ├── Blocker-Beschreibung in STATUS.md
  └── Architecture Gatekeeper oder Orchestrator informieren
```

### 9. Progress Reporting (REPORTING_PROTOCOL.md)
- Jeder Agent MUSS `STATUS.md` im Feature-Ordner updaten wenn er Arbeit beginnt/beendet.
- Orchestrator pflegt `TASK_LOG.md` im Projekt-Root.
- Alle Feature-Deliverables (GOAL, PRD, UX_SPEC, DESIGN_SPEC, QA_REPORT) in `docs/features/<name>/`.
- Bei Crash: Nächster Agent liest STATUS.md → setzt ab letztem ✅ fort.

### 10. 🛡️ Anti-Hallucination Protocol (PFLICHT für JEDEN Agent)

> **Eiserne Regel: Kein Agent liefert ab, ohne sich selbst zu prüfen UND vom Empfänger geprüft zu werden.**

#### A. STOP-CHECK-DELIVER (vor jeder Abgabe)

Jeder Agent MUSS vor dem Abliefern diese 3 Fragen beantworten:

```
STOP: Ist mein Output GENAU das was angefordert wurde?
  1. Habe ich GOAL.md / den Auftrag nochmal gelesen? (Pflicht!)
  2. Enthält mein Output NUR was gefordert wurde? (Keine Extras!)
  3. Fehlt etwas das gefordert wurde? (Vollständigkeit!)

Wenn EINE Antwort NEIN → FIX IT before delivering.
```

#### B. Hierarchische Validierung (Empfänger prüft IMMER)

```
Agent liefert ab
    ↓
Empfänger (höhere Hierarchie) prüft:
  ├── Entspricht dem Auftrag? → ✅ Akzeptiert
  ├── Fehlt etwas? → 🔴 REJECT: "AC #X fehlt"
  ├── Zu viel drin? → 🔴 REJECT: "Entferne [X], nicht gefordert"
  └── Anders als gefordert? → 🔴 REJECT: "Soll [A] sein, ist [B]"
```

#### C. Agent-spezifische Anti-Hallucination Regeln

| Agent | Prüft sich gegen | Wird geprüft von |
|-------|------------------|-----------------|
| Stack Researcher | GOAL.md — nur recherchieren was gefragt ist | Gatekeeper / Orchestrator |
| UX Researcher | PRD — nur Flows für geforderte Features | Orchestrator |
| Design Architect | UX_SPEC + PRD — nur Screens designen die gefordert sind | Orchestrator |
| Frontend Builder | DESIGN_SPEC + GOAL.md — nur bauen was designed ist | Orchestrator (Scope Gate) |
| Backend Builder | API_SPEC + GOAL.md — nur API-Endpoints die spezifiziert sind | Gatekeeper |
| Unit Test Writer | Code + PRD ACs — nur testen was existiert und gefordert | QA Orchestrator |
| Design Auditor | DESIGN_SPEC — reviewen gegen Spec, nicht persönliche Meinung | QA Orchestrator |
| E2E Tester | PRD ACs — nur User Journeys testen die im PRD stehen | QA Orchestrator |
| QA Orchestrator | PRD + DESIGN_SPEC — nur relevante Tests dispatchen | Orchestrator |
| Docs Manager | SYSTEM_STATE + Code — nur dokumentieren was existiert | Orchestrator |
| Spec Reviewer | GOAL.md — nur reviewen was im Scope steht | Gatekeeper |
| Systematic Debugger | Bug-Report — nur den gemeldeten Bug untersuchen | Aufrufender Agent |
| Security Auditor | OWASP + Code — nur Schwachstellen die existieren, keine Phantome | Gatekeeper |
| Performance Optimizer | Messdaten — nur optimieren was gemessen langsam ist | Gatekeeper |
| Accessibility Expert | WCAG 2.1 + Code — nur existierende Barrieren melden | QA Orchestrator |
| Git Workflow Manager | Branch-Policy — nur definierte Branch-Operationen ausführen | Gatekeeper |

#### D. Verbotene Patterns (🔴 SOFORT STOPPEN)

1. **"Ich füge noch schnell X hinzu"** → NEIN. Wenn X nicht im GOAL.md steht, nicht bauen.
2. **"Das wäre doch besser wenn..."** → NEIN. Verbesserungsvorschläge als Kommentar, nicht als Code.
3. **"Ich habe auch noch Y gemacht"** → NEIN. Nur den Auftrag erfüllen.
4. **"Vielleicht braucht man auch Z"** → NEIN. Scope ist definiert.
5. **Eigene ACs erfinden** → NEIN. Nur die ACs aus GOAL.md/PRD umsetzen.
6. **Tests für nicht-existierenden Code schreiben** → NEIN. Nur testen was gebaut wurde.
7. **Design-Reviews nach persönlichem Geschmack** → NEIN. Nur gegen DESIGN_SPEC prüfen.

---

## Projekt-Ordnerstruktur

```
project-root/
├── SYSTEM_STATE.md         ← Aktueller Systemzustand (Routes, Components, Stores, APIs)
├── MEMORY.md               ← Gelernte Patterns, Entscheidungen, Gotchas
├── TASK_LOG.md             ← Zentrale Feature-Übersicht (aktiv/blockiert/fertig)
├── docs/
│   ├── lib/                ← Context7 Docs (vom Stack Researcher)
│   ├── architecture/       ← Architektur-Docs (vom Gatekeeper)
│   ├── features/           ← Feature-Docs (pro Feature ein Ordner)
│   │   └── <feature>/
│   │       ├── STATUS.md           ← LIVE Status, Fortschritt, Änderungsprotokoll
│   │       ├── GOAL.md             ← Acceptance Criteria
│   │       ├── SPEC_REVIEW.md      ← Spec Review Ergebnis (Spec Reviewer)
│   │       ├── PRD.md              ← Product Requirements
│   │       ├── UX_SPEC.md          ← User Flows
│   │       ├── DESIGN_SPEC.md      ← Design Tokens + Layout
│   │       ├── BRANCH_STATUS.md    ← Git Branch Lifecycle (Git Workflow Manager)
│   │       ├── ROOT_CAUSE.md       ← Bug Root Cause (Systematic Debugger)
│   │       ├── SECURITY_AUDIT.md   ← Security Findings (Security Auditor)
│   │       ├── PERFORMANCE_REPORT.md ← Performance Metrics (Performance Optimizer)
│   │       ├── A11Y_AUDIT.md       ← Accessibility Findings (Accessibility Expert)
│   │       ├── QA_REPORT.md        ← Test-Ergebnisse (QA Orchestrator)
│   │       └── FEATURE_SCORECARD.md ← Finale Bewertung
│   └── api/                ← API-Docs (vom Docs Manager)
├── agents/                 ← Agent-Konfigurationen (dieses Verzeichnis)
│   ├── README.md           ← Übersicht + Quick Start
│   ├── AGENTS_OVERVIEW.md  ← Alle 30 Agents Detailübersicht
│   ├── SHARED_CONFIG.md    ← DIESES FILE
│   ├── SKILLS.md           ← Skill Discovery + extrahiertes Wissen
│   ├── REPORTING_PROTOCOL.md ← Status-Reporting Regeln + Templates
│   └── [agent-ordner]/     ← 30 Individuelle Agent-Configs (je 4 Dateien)
└── src/                    ← Source Code
```
