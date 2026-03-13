# SKILLS.md — Paperclip Skill Discovery & Knowledge Base

> Dieses File wird von Paperclip zur Runtime geladen.
> Es sagt Agents, wie sie Skills finden und enthält extrahiertes Wissen der besten Skills.
> Letzte Aktualisierung: 2026-03-13 (Skill Loading Methoden, 30 Agents, 17 Skill-Quellen)

---

## Skill Discovery & Loading

Wenn ein Agent eine Aufgabe bekommt die neue Skills erfordert:

1. **Prüfe zuerst**: Ist der Skill schon in deiner `TOOLS.md` gelistet? → Benutze ihn direkt (Methodik ist inline).
2. **Suche nach Skills**: `skills search "<thema>"` (via [skills.sh](https://skills.sh))
3. **Installiere Skill**: `skills install <skill-name>` (lädt SKILL.md in deinen Kontext)
4. **Context7 für Docs**: `resolve-library-id` + `get-library-docs` — aktuelle Library-Docs
5. **Paperclip API**: Prüfe ob ein anderer Agent im Org-Chart den Skill bereits hat

### Skill laden — 3 Methoden

```bash
# Methode 1: skills.sh CLI (empfohlen)
skills search "systematic-debugging"
skills install systematic-debugging

# Methode 2: GitHub Raw Download (wenn CLI nicht verfügbar)
curl -sL https://raw.githubusercontent.com/obra/superpowers/main/skills/systematic-debugging/SKILL.md

# Methode 3: Volle Skill-Registry → siehe ../SHARED_CONFIG.md Abschnitt "Skill Registry"
```

> **Vollständige Download-URLs und Install-Befehle für ALLE Skills** → siehe `SHARED_CONFIG.md` → Abschnitt "Skill Registry — Download URLs"

---

## Top Skills — Extrahiertes Wissen

### 🏆 #1 vercel-react-best-practices (199K installs)

**8 Regel-Kategorien nach Priorität:**

| Priorität | Kategorie | Wichtigste Regeln |
|-----------|----------|-------------------|
| 🔴 CRITICAL | Waterfalls eliminieren | `Promise.all()` für unabhängige Ops, await erst in Branches wo nötig, Suspense für Streaming |
| 🔴 CRITICAL | Bundle Size | Direkte Imports (keine Barrel Files), `next/dynamic` für Heavy Components, Third-Party nach Hydration |
| 🟠 HIGH | Server Performance | `React.cache()` für Per-Request Dedup, LRU-Cache cross-request, Minimize Client-Serialization, `after()` non-blocking |
| 🟡 MEDIUM-HIGH | Client Data | SWR für Request-Dedup, Passive Event Listeners für Scroll, localStorage versionieren |
| 🟢 MEDIUM | Re-render | Derived State during render (nicht Effects), `startTransition` für non-urgent, Refs für transiente Werte |
| 🟢 MEDIUM | Rendering | `content-visibility` für lange Listen, Ternary statt `&&`, `useTransition` für Loading |
| 🔵 LOW-MEDIUM | JS Performance | Map/Set für O(1) Lookups, Early Return, `toSorted()` für Immutability, Combine filter/map |
| 🔵 LOW | Advanced | Event Handler in Refs, Init-once Pattern, `useLatest` Hook |

**Agent**: Frontend Builder (`frontend-builder/`)

---

### 🏆 #2 web-design-guidelines (155K installs)

**Wie es funktioniert:**
1. Fetch aktuelle Guidelines von: `https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md`
2. Lese die zu prüfenden Files
3. Prüfe gegen **alle** Rules aus den gefetchten Guidelines
4. Output in `file:line` Format

**Agent**: Design Auditor (`design-auditor/`) — nutze als Audit-Referenz

---

### 🏆 #3 frontend-design (145K installs, Anthropic)

**Design Direction Workflow:**
1. Purpose → Tone → Differentiation definieren
2. Typography: NIE System Fonts. Distinctive Fonts + Pairing
3. Color: Dominant + scharfe Akzente > timide Paletten. CSS Variables
4. Motion: Staggered Page Load > verstreute Micro-Interactions
5. Spatial: Asymmetrie, Overlap, Grid-Breaking, großzügiger Negativraum
6. Backgrounds: Gradients, Noise, Textures, Patterns. NIE plain solid

**Agent**: Design Architect (`design-architect/`)

---

### 🏆 #4 agent-browser (91K installs)

**Kernbefehle:**
```bash
agent-browser open <url>               # Seite öffnen
agent-browser wait --load networkidle   # Warten auf Netzwerk
agent-browser click @element-id         # Klicken
agent-browser type @input-id "text"     # Tippen
agent-browser screenshot <name>.png     # Screenshot
agent-browser set viewport 375 812     # Viewport ändern
agent-browser snapshot -i               # Interactive Elements listen
```

**Agents**: E2E Tester, Design Auditor, Browser Automation

---

### 🏆 #5 ui-ux-pro-max (57K installs)

**Accessibility PFLICHT-Regeln:**
- Kontrast: ≥ 4.5:1 normal, ≥ 3:1 large text
- Touch Targets: ≥ 44×44px
- Fokus Ring: 2-4px, sichtbar, sequentiell
- Heading Hierarchy: h1 → h2 → h3 (nie überspringen)
- `prefers-reduced-motion`: Alle Animations außer Opacity deaktivieren
- Farbe NIE als einziger Indikator

**Agents**: Design Architect, Design Auditor, Frontend Builder

---

### 🏆 #6 systematic-debugging (28K installs, obra)

**Das eiserne Gesetz: KEINE FIXES OHNE ROOT CAUSE INVESTIGATION**

**4 Phasen (MANDATORY — jede Phase VOR der nächsten):**
1. **Root Cause Investigation** — Layer für Layer durchgraben
2. **Pattern Analysis** — Symptome gruppieren, Gemeinsamkeiten finden
3. **Hypothesis & Testing** — Hypothese formulieren, gezielt testen
4. **Implementation** — Erst jetzt fixen

**Red Flags (STOP):**
- Du sagst "done" ohne Root Cause
- Du rätst statt systematisch zu suchen
- Du fixst Symptome statt Ursachen
- Unter Zeitdruck überspringst du Phasen

**Agents**: Alle Test-Agents, Frontend Builder

---

### 🏆 #7 test-driven-development (23K installs, obra)

**Red-Green-Refactor (MANDATORY jeder Schritt):**

| Phase | Was | PFLICHT |
|-------|-----|---------|
| 🔴 RED | EIN minimaler Test der fehlschlägt | Nur EIN Behavior pro Test |
| 👁️ VERIFY RED | `npm test` — Test MUSS feilen | NIE überspringen |
| 🟢 GREEN | Einfachster Code der den Test passiert | Keine Features hinzufügen |
| 👁️ VERIFY GREEN | `npm test` — Test MUSS passen | Andere Tests auch grün? |
| 🔄 REFACTOR | Duplikation entfernen, Names verbessern | Tests bleiben grün |

**Gute Tests:** Klarer Name, testet echtes Verhalten, EIN Ding pro Test
**Schlechte Tests:** Vager Name, testet Mocks statt Code

**Agents**: Unit Test Writer (`unit-test-writer/`), Frontend Builder

---

### 🏆 #8 webapp-testing (22K installs, Anthropic)

**Decision Tree:** Server vs. Browser Testing
- CLI/API Tests → `curl` oder Script
- Visual/Interactive → `agent-browser`
- **Recon-Then-Action Pattern**: Erst Seite erkunden, dann handeln

**Agents**: E2E Tester, QA Orchestrator

---

### 🏆 #9 swarm-planner (12K installs, am-will)

**Dependency-Aware Plans:**
```
T1: [depends_on: []] Create database schema
T2: [depends_on: []] Install packages
T3: [depends_on: [T1]] Create repository layer
T4: [depends_on: [T3]] Implement business logic
T5: [depends_on: [T2, T4]] Add API endpoints
```
- Tasks mit leerem `depends_on` → PARALLEL
- PFLICHT: Context7 Docs VOR Planung
- Nach Plan: Sub-Agent Review spawnen

**Agents**: Gatekeeper, Orchestrator

---

### 🏆 #10 next-best-practices (32K installs, vercel-labs)

**Next.js Spezifisch:**
- App Router > Pages Router
- Server Components default, `"use client"` nur wenn nötig
- Route Handlers für API, Server Actions für Mutationen
- `loading.tsx` + `error.tsx` + `not-found.tsx` für jeden Route
- Metadata API für SEO
- `generateStaticParams` für Static Generation

**Agent**: Frontend Builder

---

### 🏆 #11 supabase-postgres-best-practices (32K installs)

**Datenbank-Patterns:**
- Row Level Security (RLS) IMMER aktivieren
- Indexes für alle Foreign Keys
- Enums statt Magic Strings
- Prepared Statements gegen SQL Injection
- Connection Pooling mit PgBouncer

**Agent**: Stack Researcher (Referenz für Backend-Entscheidungen), Backend Builder (`backend-builder/`)

### 🏆 #12 writing-plans (26K installs, obra)

**Plan-Writing für KI-Agents — Zero-Context Approach:**

1. **Scope Check**: Mehrere Subsysteme? → In separate Pläne aufteilen
2. **File Structure ZUERST**: Welche Files werden erstellt/geändert? Decomposition-Entscheidungen hier
3. **Bite-Sized Tasks** (2-5min pro Step):
   ```
   - "Write the failing test" — step
   - "Run it to make sure it fails" — step
   - "Implement the minimal code" — step
   - "Run tests and verify" — step
   - "Commit" — step
   ```
4. **Checkbox Syntax**: `- [ ]` für Tracking
5. **Header**: Goal (1 Satz), Architecture (2-3 Sätze), Tech Stack

**Prinzipien**: DRY, YAGNI, TDD, Frequent Commits
**Annahme**: Implementer hat NULL Context → ALLES dokumentieren

**Agents**: Gatekeeper, Orchestrator

---

### 🏆 #13 executing-plans (22K installs, obra)

**Plan ausführen — 3 Steps:**

1. **Load & Review** — Plan lesen, kritisch prüfen, Fragen VORHER stellen
2. **Execute Tasks** — Jede Task: `in_progress` → Steps exakt folgen → Verify → `completed`
3. **Complete** — Alle Tasks fertig → `finishing-a-development-branch` nutzen

**STOP & Fragen bei:**
- Blocker (fehlende Dependency, Test fail, Instruction unklar)
- Plan hat kritische Lücken
- Instruction nicht verstanden
- Verification repeatedly fails

**NIEMALS**: Auf main/master Branch starten ohne User-Consent!

**Agents**: Frontend Builder, alle Implementer

---

### 🏆 #14 verification-before-completion (17K installs, obra)

**Eisernes Gesetz: KEINE COMPLETION CLAIMS OHNE FRESH VERIFICATION**

**5-Step Gate Function (PFLICHT vor JEDER Abgabe):**
```
1. IDENTIFY: Welcher Command beweist deine Behauptung?
2. RUN: Führe den VOLLSTÄNDIGEN Command aus (frisch, komplett)
3. READ: Gesamten Output lesen, Exit-Code prüfen, Fehler zählen
4. VERIFY: Bestätigt Output deine Behauptung?
5. ERST DANN: Behauptung machen MIT Beweis
```

**Red Flags (SOFORT STOPPEN):**
- "Should", "probably", "seems to" verwenden
- "Great!", "Perfect!", "Done!" OHNE Verification
- Agent-Reports vertrauen ohne VCS-Diff zu prüfen
- "Looks correct" statt tatsächlich zu testen

**Agents**: ALLE Agents — besonders Builder, Test Writer, E2E Tester

---

### 🏆 #15 subagent-driven-development (18K installs, obra)

**Per-Task Flow mit 3 Review-Runden:**

```
Für JEDE Task:
  1. Dispatch Implementer → baut + testet + committed + self-reviewed
  2. Dispatch Spec-Reviewer → bestätigt Code matched Spec
     ├── NEIN → Implementer fixt Gaps → Spec-Review nochmal
     └── JA → weiter
  3. Dispatch Code-Quality-Reviewer → prüft Code-Qualität
     ├── NEIN → Implementer fixt Quality → Review nochmal
     └── JA → Task complete

Nach ALLEN Tasks:
  → Final Code Reviewer für gesamte Implementation
  → finishing-a-development-branch
```

**Agents**: Orchestrator, QA Orchestrator (pattern für Sub-Agent Dispatch)

---

### 🏆 #16 dispatching-parallel-agents (15K installs, obra)

**4-Step Pattern für parallele Ausführung:**

1. **Identify Independent Domains** — Gruppiere nach was unabhängig ist
2. **Create Focused Agent Tasks** — Pro Agent: Scope + Goal + Constraints + Expected Output
3. **Dispatch in Parallel** — Alle unabhängigen Tasks gleichzeitig
4. **Review & Integrate** — Summaries lesen, Konflikte prüfen, Full Test Suite

**Wann NICHT parallel:** Tasks die sich gegenseitig blockieren oder gleiche Files ändern

**Agents**: Orchestrator, QA Orchestrator (parallel: Tests + Audit)

---

### 🏆 #17 finishing-a-development-branch (14K installs, obra)

**5 Steps zum sauberen Abschluss:**
1. **Verify Tests** — Alle Tests grün? Build OK?
2. **Determine Base Branch** — Wohin mergen?
3. **Present Options** — User entscheidet: Merge, Squash, Rebase
4. **Execute Choice** — Merge durchführen
5. **Cleanup Worktree** — Aufräumen

**Agents**: Frontend Builder (nach Feature-Delivery)

---

### 🏆 #18 typescript-advanced-types (13K installs, wshobson)

**Key Patterns für Frontend Builder:**
- **Discriminated Unions** für Type-Safe State: `{ status: 'loading' } | { status: 'success'; data: T } | { status: 'error'; error: Error }`
- **Generics** für wiederverwendbare Components/Hooks
- **Mapped Types** für Type-Safe Forms: `Record<keyof FormData, FieldConfig>`
- **Template Literal Types** für Type-Safe API Endpoints
- **Type Guards** (`is` keyword) für Runtime-Type-Checks
- **Utility Types**: `Partial<T>`, `Required<T>`, `Pick<T, K>`, `Omit<T, K>`
- **`satisfies`** Operator für Type-Checking ohne Narrowing zu verlieren

**Agent**: Frontend Builder

---

### 🏆 #19 git-commit (13K installs, github)

**Commit Message Convention:**
```
<type>(<scope>): <description>

[body — was und warum, nicht wie]

[footer — Breaking Changes, Issue-Referenzen]
```

**Types**: feat, fix, refactor, test, docs, style, chore, perf
**Scope**: Component/Feature Name

**Agents**: Alle Agents die Code committen

---

### 🏆 #20 brainstorming (51K installs, obra)

**Socratic Design Refinement — VOR dem Coden:**

1. **Scope Check ZUERST**: Zu groß? → In Sub-Projekte aufteilen. Jedes bekommt eigenen Spec → Plan → Implementation Cycle
2. **Verstehen**: Eine Frage pro Nachricht, Multiple Choice wenn möglich. Purpose, Constraints, Success Criteria
3. **2-3 Ansätze vorschlagen** mit Trade-offs + Empfehlung + Begründung
4. **Design präsentieren** — Sektion für Sektion, nach jeder Sektion fragen ob es passt
5. **Design for Isolation**: Jede Unit hat EINEN Zweck, klar definierte Interfaces, unabhängig testbar
6. **Spec schreiben** → Spec-Review-Loop (max 5 Iterationen) → User Review Gate → writing-plans

**Anti-Pattern**: "Das ist zu einfach für ein Design" — **NEIN!** Auch einfache Features brauchen Design.

**Agents**: Gatekeeper (Feature-Design), UX Researcher (UX-Design)

---

### 🏆 #21 requesting-code-review (21K installs, obra)

**Code Review nach JEDEM Task — nie überspringen:**

**Wann (MANDATORY):**
- Nach jeder Task in subagent-driven-development
- Nach Major Feature
- Vor Merge zu main

**Wie:**
1. Git SHAs holen (BASE_SHA + HEAD_SHA)
2. Code-Reviewer Subagent dispatchen mit: Was gebaut wurde + Plan/Requirements + SHAs
3. **Auf Feedback reagieren:**
   - Critical → SOFORT fixen
   - Important → Vor Weiterarbeit fixen
   - Minor → Für später notieren
   - Reviewer falsch? → Push-back MIT technischer Begründung

**Red Flags**: "Es ist zu einfach für Review" → **NEIN!** Nie überspringen.

**Agents**: Frontend Builder (nach jedem Build), QA Orchestrator (Review-Dispatch)

---

### 🏆 #22 receiving-code-review (17K installs, obra)

**6-Step Response Pattern:**
```
1. READ: Feedback komplett lesen ohne sofort zu reagieren
2. UNDERSTAND: Anforderung in eigenen Worten wiedergeben (oder fragen)
3. VERIFY: Gegen die tatsächliche Codebase prüfen
4. EVALUATE: Technisch korrekt für DIESE Codebase?
5. RESPOND: Technische Bestätigung ODER begründeter Push-back
6. IMPLEMENT: Ein Item nach dem anderen, jedes testen
```

**VERBOTEN:**
- "You're absolutely right!" / "Great point!" / "Excellent feedback!" → Performativ
- "Let me implement that now" → OHNE vorher zu verifizieren
- Items implementieren die du nicht verstehst → STOP, Klarstellung holen

**YAGNI Check**: Wird "professionelles" Feature wirklich gebraucht? Oder Over-Engineering?

**Agents**: Frontend Builder (wenn Code-Review kommt), alle implementierenden Agents

---

### 🏆 #23 using-superpowers (20K installs, obra)

**Skill Priority Hierarchy (das Framework):**
1. User Instructions → HÖCHSTE
2. Skill-spezifische Rules
3. System Prompt Defaults → NIEDRIGSTE

**Wann welcher Skill?**
- Brainstorming → VOR Design
- Systematic Debugging → BEI jedem Bug
- TDD → WÄHREND Implementation
- Verification → VOR jeder Completion-Behauptung
- Code Review → NACH jeder Task

**Agents**: ALLE Agents — Meta-Framework

---

## 🔄 Superpowers 7-Step Workflow → Agent Mapping

Das obra/superpowers Framework definiert einen 7-Step Workflow der **perfekt** auf unsere Agents mapped:

| Step | Superpowers Skill | Unser Agent | Was passiert |
|------|------------------|-------------|-------------|
| 1 | `brainstorming` | **Gatekeeper** → UX Researcher | Idee → Socratic Refinement → Design-Spec → User Review Gate |
| 2 | `using-git-worktrees` | **Frontend Builder** | Isolierter Worktree → Branch → Setup → Baseline Tests grün |
| 3 | `writing-plans` | **Gatekeeper** → Orchestrator | PRD → Bite-sized Tasks (2-5min) → File Structure → Checkpoint |
| 4 | `subagent-driven-development` | **Orchestrator** → Builder + QA | Pro Task: Implementer → Spec-Reviewer → Code-Quality-Reviewer |
| 5 | `test-driven-development` | **Unit Test Writer** + Builder | RED → Verify RED → GREEN → Verify GREEN → REFACTOR → Commit |
| 6 | `requesting-code-review` | **Orchestrator** → Code-Reviewer | Nach jeder Task: SHA-Range → Review → Fix Critical/Important |
| 7 | `finishing-a-development-branch` | **Builder** → Gatekeeper | Verify Tests → Base Branch → Merge/PR → Cleanup |

**Zusätzlich immer aktiv:**
- `systematic-debugging` → bei JEDEM Bug (alle Agents)
- `verification-before-completion` → vor JEDER Completion-Behauptung (alle Agents)
- `dispatching-parallel-agents` → wenn unabhängige Tasks parallel laufen können (Orchestrator, QA)

---

## Available Skill Repos (Top 15)

| Repository | Top Skills | Installs |
|-----------|-----------|----------|
| `vercel-labs/agent-skills` | react-best-practices, composition-patterns, web-design-guidelines | 80K–200K |
| `vercel-labs/agent-browser` | agent-browser | 92K |
`anthropics/skills` | frontend-design, canvas-design, webapp-testing, skill-creator, pdf | 17K–147K |
| `obra/superpowers` | systematic-debugging, writing-plans, executing-plans, TDD, verification-before-completion, subagent-driven-development, dispatching-parallel-agents, finishing-a-development-branch | 14K–28K |
| `vercel-labs/next-skills` | next-best-practices | 32K |
| `supabase/agent-skills` | supabase-postgres-best-practices | 32K |
| `better-auth/skills` | better-auth-best-practices | 21K |
| `am-will/codex-skills` | swarm-planner, context7, frontend-responsive-design-standards, role-creator, parallel-task-spark | 9K–12K |
| `currents-dev/playwright-best-practices-skill` | playwright-best-practices | 8K |
| `nextlevelbuilder/ui-ux-pro-max-skill` | ui-ux-pro-max | 58K |
| `wshobson/agents` | typescript-advanced-types, tailwind-design-system | 13K–17K |
| `github/awesome-copilot` | git-commit, documentation-writer | 9K–13K |
| `google-labs-code/stitch-skills` | react:components, design-md, stitch-loop, enhance-prompt | 12K–14K |
| `sleekdotdesign/agent-skills` | sleek-design-mobile-apps | 128K |
| `coreyhaines31/marketingskills` | seo-audit, copywriting, content-strategy, analytics-tracking | 15K–40K |

---

## Runtime Context Loading Order

Agents should always check for context in this order:
1. `GOAL.md` — Was ist das Ziel?
2. `../../SYSTEM_STATE.md` — Was existiert aktuell?
3. `../../MEMORY.md` — Was hat die Firma gelernt?
4. `../../TASK_LOG.md` — Was ist aktiv/blockiert/fertig?
5. `docs/features/<feature>/STATUS.md` — Wo steht dieses Feature?
6. `../SHARED_CONFIG.md` — Stack, Rules, MCP, Anti-Hallucination
7. `../SKILLS.md` — DIESES FILE: Skill Discovery + Wissen
8. `../REPORTING_PROTOCOL.md` — Wie Status reportet wird
9. `docs/lib/` — Aktuelle Library-Docs (via Context7)
10. Own `TOOLS.md` — Agent-spezifische Skills
