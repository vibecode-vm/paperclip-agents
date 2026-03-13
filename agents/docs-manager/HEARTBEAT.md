# HEARTBEAT.md -- Documentation & Knowledge Manager Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch documentation tasks.
- Triggered after: feature ships, API changes, architecture changes.

## 3. Goal Check (MANDATORY)
- Read the GOAL.md for the feature that shipped.
- Understand what was built and what documentation needs updating.

## 4. Read Feature Artifacts
- Read `FEATURE_SCORECARD.md` — understand what was delivered.
- Read `DESIGN_SPEC.md` — understand the design intent.
- Read `TEST_REPORT.md` — understand test coverage.
- Read code changes — understand what components/APIs were added or modified.

## 5. Update SYSTEM_STATE.md
- Add new routes.
- Add new components.
- Add new API endpoints.
- Add new state stores.
- Update versions if changed.
- Remove deprecated items.

## 6. Update CHANGELOG.md
```markdown
## [date] — [Feature Name]

### Added
- [New component/feature]

### Changed
- [Modified behavior]

### Fixed
- [Bug fix from this feature]
```
- Explain WHY, not just WHAT.

## 7. Update Component Documentation
- For each new component: create doc page using template.
- For each modified component: update existing doc.
- Include: usage example, props table, variants, accessibility info.

## 8. Update API Documentation
- For each new endpoint: create doc page using template.
- For each modified endpoint: update existing doc.
- Include: request/response examples, auth requirements.

## 9. Update README.md (if needed)
- New setup steps?
- New environment variables?
- New dependencies?

## 10. Verify Documentation
- All file paths in docs point to existing files.
- All component names match actual code.
- All API endpoints are correct.
- No broken links.

## 11. Exit
- Comment on issue: "Docs updated: [list of files]."
- Commit documentation changes.

## Rules
- Update surgically — don't rewrite unchanged sections.
- Every doc must have a "Last updated" timestamp.
- Examples must be copy-pasteable and runnable.
- Cross-reference: components ↔ usage, APIs ↔ consumers.
- Flag stale docs — don't silently ignore them.
- SYSTEM_STATE.md is ALWAYS updated after every feature.
