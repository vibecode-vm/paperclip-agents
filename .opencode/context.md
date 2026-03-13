# Project Context

## Environment
- Language: Markdown + Paperclip API workflow
- Runtime: Local Paperclip heartbeat
- Build: None
- Test: API verification + filesystem verification
- Package Manager: None

## Project Type
- [ ] Library/Package
- [ ] Application (CLI/Web/Mobile/Desktop)
- [ ] Microservice
- [ ] Monorepo
- [x] Other: Paperclip agent workspace bootstrap

## Infrastructure
- Container: None detected
- Orchestration: Paperclip heartbeat runner
- CI/CD: None in workspace root
- Cloud: Paperclip control plane at `http://localhost:3112`

## Structure
- Source: `agents/ceo/`
- Tests: none
- Docs: `.opencode/`, `agents/ceo/*.md`
- Entry: Assigned issue `SER-1`

## Conventions
- Naming: uppercase agent docs (`AGENTS.md`, `HEARTBEAT.md`, `SOUL.md`, `TOOLS.md`)
- Imports: n/a
- Error handling: explicit Paperclip issue status updates
- Testing: verify via API responses and local file reads

## Notes
- Agent identity: CEO (`6cc2c1b0-781f-48f0-bb85-6e2cdc9fd727`)
- Initial bootstrap run id: `2b3be0a0-5c74-4767-835d-b279e8ff43ef`
- Approval follow-up run id: `8b4423a8-30da-432e-bc58-31ad0bb335a9`
- Assigned task: `SER-1` create CEO home docs, set instructions path, hire Founding Engineer.
- Founding Engineer hire approval `a597f423-fa3a-4f90-87e6-31ceabd6fb9c` was approved on 2026-03-11.
- Founding Engineer agent id: `3dee5113-fbbd-4024-973b-ecbefa09121a` (`founding-engineer`), status `idle`.
