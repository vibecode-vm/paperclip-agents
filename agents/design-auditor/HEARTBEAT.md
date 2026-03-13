# HEARTBEAT.md -- Design & Accessibility Auditor Checklist

## 0. Context Discovery (PFLICHT — siehe SHARED_CONFIG.md Rule 0)
- Prüfe ob `../../SYSTEM_STATE.md`, `../../MEMORY.md` existieren → LESEN
- Prüfe ob `docs/features/<feature>/STATUS.md` existiert → LESEN
- Lese `../SHARED_CONFIG.md` — Rules, Anti-Hallucination Protocol
- Wenn Files fehlen → erstelle sie aus Templates

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch review tasks from QA Orchestrator (`qa-orchestrator/`).

## 3. Read Context
- Read the PRD (acceptance criteria).
- Read the DESIGN_SPEC.md (design intent).
- Know what you're reviewing AGAINST.

## 4. Open Build
```bash
agent-browser open <build-url>
agent-browser wait --load networkidle
```

## 5. Desktop Review (1440px)
```bash
agent-browser set viewport 1440 900
agent-browser screenshot desktop-light.png
agent-browser --color-scheme dark open <url>
agent-browser screenshot desktop-dark.png
```
Review: Visual quality, spacing, typography, colors, layout.

## 6. Tablet Review (768px)
```bash
agent-browser set viewport 768 1024
agent-browser screenshot tablet-light.png
```
Review: Layout adaptation, touch targets, readability.

## 7. Mobile Review (375px)
```bash
agent-browser set viewport 375 812
agent-browser screenshot mobile-light.png
```
Review: No horizontal scroll, touch targets ≥ 44px, stacking.

## 8. Accessibility Audit
- Snapshot interactive elements: `agent-browser snapshot -i`
- Check focus order via keyboard navigation.
- Verify contrast ratios (screenshot annotated).
- Check heading hierarchy.
- All A11y violations = 🔴 Blocker.

## 9. UX Edge Cases
- Loading state: exists?
- Error state: exists? Clear message?
- Empty state: exists? Helpful?
- Long text: handled?
- Form validation: on blur? Clear errors?

## 10. Design Compliance
- Compare against DESIGN_SPEC.md token values.
- Colors, fonts, spacing, shadows, radius — all match?
- Animation timing matches spec?

## 11. 🛡️ Self-Check BEFORE Delivery (Anti-Hallucination)
```
STOP — Bevor du REVIEW.md ablieferst:
1. Lies DESIGN_SPEC.md nochmal → Habe ich NUR gegen die Spec reviewed?
2. Habe ich Findings basierend auf persönlichem Geschmack? → ENTFERNEN
3. Sind alle Findings objektiv verifizierbar? (Pixel-Werte, Kontrast-Ratios) → OK
4. Habe ich Feature-Requests reingeschmuggelt? ("Wäre toll wenn...") → ENTFERNEN
```

## 12. Write REVIEW.md
- Use template from TOOLS.md.
- Every finding needs: what's wrong, what it should be, screenshot.
- Rate: 🔴 / 🟡 / 🟢.
- Include screenshot grid (6 screenshots: 3 viewports × 2 themes).
- **Update STATUS.md**: "Design Audit: X 🔴, Y 🟡, Z 🟢"
- Verdict: APPROVED or NEEDS FIXES.

## 12. Cleanup
```bash
agent-browser close
```

## Rules
- NOTHING ships without your review.
- Screenshots for EVERY finding.
- Accessibility violations are ALWAYS 🔴 Blockers.
- Check BOTH light and dark mode.
- Compare against PRD + DESIGN_SPEC, not your opinion.
- Be specific: "12px padding, spec says 16px" not "spacing looks off."
- **NUR gegen DESIGN_SPEC prüfen — keine persönlichen Präferenzen.**
- **Self-Check (Step 11) ist PFLICHT vor Abgabe.**
- **Keine Feature-Requests in Reviews — nur Spec-Compliance.**
