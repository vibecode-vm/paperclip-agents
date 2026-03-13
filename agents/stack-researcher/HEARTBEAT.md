# HEARTBEAT.md -- Technical Research & Stack Validation Agent Checklist

## 0. Context Discovery (PFLICHT — siehe SHARED_CONFIG.md Rule 0)
- Prüfe ob `../../SYSTEM_STATE.md`, `../../MEMORY.md` existieren → LESEN
- Prüfe ob `docs/lib/` existiert → LESEN (welche Docs sind schon da?)
- Prüfe ob `docs/features/<feature>/STATUS.md` existiert → LESEN
- Lese `../SHARED_CONFIG.md` — mandated Stack, Rules
- Wenn Files fehlen → erstelle sie aus Templates

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch research tasks from Architecture & Quality Gatekeeper or Feature Lifecycle Orchestrator.
- Determine mode: Stack Audit (A) / Library Research (B) / Boilerplate Audit (C) / **Documentation Fetch (D)**

## 3. Goal Check (MANDATORY)
- Read the GOAL.md for the feature (if feature-specific research).
- Understand: What does the Builder need? What problem are we solving?

---

## MODE A: Stack Audit (Before Feature Sprint)

### 4A. Check All Package Versions
```bash
npm list --depth=0
npm outdated
```
- Record: current vs latest for every package.
- Flag: packages > 1 minor version behind.

### 5A. Security Scan
```bash
npm audit
npm audit --production
```
- Flag: any critical or high vulnerabilities.
- Check: are fixes available?

### 6A. Peer Dependency Check
```bash
npm dedupe --dry-run
```
- Identify: conflicting peer dependencies.
- Check: will planned new packages conflict?

### 7A. Boilerplate Scan
- Scan codebase for repeated patterns (fetch wrappers, error handlers, form logic).
- Count occurrences.
- Recommend: abstractions or packages to reduce.

### 8A. Write STACK_AUDIT.md
- Use template from TOOLS.md.
- Categorize updates: safe (patch) / review (minor) / careful (major).
- Include update plan with priority.
- Deliver to Gatekeeper.

---

## MODE B: Library Research (Before Feature Build)

### 4B. Understand the Need
- What capability does the feature need?
- What are the constraints? (bundle size, compatibility, TS support)
- Is there already something in our stack that solves this?

### 5B. Search for Candidates
- Search npm: `npm search <keyword>`
- Search bundlephobia.com for size comparison.
- Search npmtrends.com for popularity comparison.
- Search skills.sh for relevant skills.
- Identify top 3 candidates.

### 6B. Evaluate Each Candidate
For each candidate, check ALL criteria:
```bash
npm info <package> version
npm info <package> time.modified
npm info <package> peerDependencies
npm info <package> license
```
- Bundle size: bundlephobia.com
- GitHub: stars, last commit, open issues
- TypeScript: built-in types?
- shadcn/ui compatible?

### 7B. Compatibility Test
```bash
npm install --dry-run <package>
```
- Does it install without conflicts?
- Does it work with React 19 + Next.js 15?

### 8B. Write RESEARCH.md
- Use template from TOOLS.md.
- Include comparison table.
- Clear recommendation with reasoning.
- Integration guide (install + usage example).
- Deliver to Orchestrator / Builder.

---

## MODE C: Boilerplate Audit (On Request)

### 4C. Scan Codebase
- Look for: repeated fetch patterns, manual form validation, duplicated error handling, repeated state patterns, copy-pasted components.
- Count: How many times does each pattern appear?

### 5C. Find Solutions
For each pattern:
- Is there a package that eliminates this? (e.g., react-query for fetch)
- Is there a custom hook/utility we should extract?
- What's the boilerplate reduction? (lines saved)

### 6C. Write BOILERPLATE_REPORT.md
```markdown
# BOILERPLATE_REPORT: [Project Name]

| Pattern | Files | Lines | Solution | Lines After | Savings |
|---------|-------|-------|----------|-------------|---------|
| Manual fetch + error | 12 | 240 | react-query | 48 | 80% |
| Form validation | 8 | 160 | react-hook-form + zod | 40 | 75% |
| Loading skeletons | 15 | 90 | Shared Skeleton comp | 15 | 83% |
```
- Deliver to Builder / Gatekeeper.

---

## MODE D: Documentation Fetch (Before Feature Build — via Context7)

This is the **critical mode** that ensures programming agents always have the latest library docs.

### 4D. Determine Which Libraries the Feature Needs
- Read PRD + GOAL.md.
- Identify: which stack libraries are relevant for this feature?
- Example: Form feature → react-hook-form, zod, shadcn/ui form

### 5D. Fetch Latest Docs via Context7
```bash
# Step 1: Resolve library IDs (if not in quick reference table)
mcp context7 resolve-library-id "next.js"
mcp context7 resolve-library-id "zustand"

# Step 2: Fetch docs for the specific topics needed
mcp context7 get-library-docs "/vercel/next.js" "server actions forms"
mcp context7 get-library-docs "/colinhacks/zod" "schema validation"
mcp context7 get-library-docs "/react-hook-form/react-hook-form" "zod resolver"
mcp context7 get-library-docs "/shadcn-ui/ui" "form component"
```

### 6D. Save Docs for Programming Agents
```bash
# Save to docs/lib/ so Builder and Test Writer can reference them
mkdir -p docs/lib/

mcp context7 get-library-docs "/vercel/next.js" "server actions" > docs/lib/nextjs-server-actions.md
mcp context7 get-library-docs "/pmndrs/zustand" "persist" > docs/lib/zustand-persist.md
mcp context7 get-library-docs "/colinhacks/zod" "schema" > docs/lib/zod-schemas.md
```

### 7D. Include in HANDOFF to Builder
```markdown
## Documentation References (Latest via Context7)
- Next.js Server Actions: `docs/lib/nextjs-server-actions.md`
- Zustand Persist: `docs/lib/zustand-persist.md`
- Zod Schemas: `docs/lib/zod-schemas.md`

⚠️ USE THESE DOCS — they are fetched from the latest versions.
   Do NOT rely on training data for API signatures.
```

### 8D. Flag Breaking Changes
- If Context7 docs show API changes from what we currently use → flag it.
- Example: "Next.js 15 deprecated `getServerSideProps` → use Server Components"
- Add to STACK_AUDIT.md or create a migration note.

---

## Rules
- EVERY recommendation needs data (bundle size, downloads, last update).
- NEVER recommend packages last updated > 6 months ago.
- NEVER recommend packages without TypeScript support.
- ALWAYS check peer dependency compatibility BEFORE recommending.
- The mandated stack is NOT replaceable. Optimize WITHIN it.
- Recommendation first, details second. Don't write essays.
- If "no change needed" → say that clearly. Don't invent work.
- **ALWAYS use Context7 to verify API signatures before recommending code patterns.**
- **ALWAYS save fetched docs to `docs/lib/` for programming agents to reference.**
- **NUR recherchieren was gefragt ist — keine zusätzlichen Libraries vorschlagen die nicht angefordert wurden.**
- **Update STATUS.md nach jeder Recherche-Lieferung.**
