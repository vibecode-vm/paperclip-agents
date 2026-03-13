# HEARTBEAT.md — Event-System Expert Checklist

## 1. Identity
- `GET /api/agents/me`

## 2. Assignments
- Fetch tasks. Prioritize `in_progress` → `todo`.

## 3. Environment Check
- Read `../SHARED_CONFIG.md` — mandated stack.
- Read `../../MEMORY.md` — known event patterns.
- Check existing event infrastructure: `ls src/lib/events/`
- Check Supabase Realtime config

## 4. Read System State (MANDATORY)

**Before starting work:**
1. Read `../../SYSTEM_STATE.md` — existing event channels, subscriptions.
2. Read `docs/lib/` — Supabase Realtime docs if available.
3. Check existing events: `grep -r "emit\|subscribe\|channel" src/`

## 5. Pattern Selection Decision Tree

```
Requirement Analysis:
├── Real-time UI updates needed?
│   ├── Bidirectional → WebSocket
│   └── Server → Client only → SSE (Server-Sent Events)
├── Database change notifications?
│   └── Supabase Realtime (postgres_changes)
├── Async background processing?
│   └── Custom Event Bus + Queue
├── Cross-service communication?
│   └── Pub/Sub Pattern (EventEmitter or Redis Pub/Sub)
└── Simple in-app notifications?
    └── Supabase Realtime (broadcast channel)
```

## 6. Event Schema Design (Zod)
- [ ] Event name follows convention: `domain.action` (e.g., `user.created`)
- [ ] Payload schema defined with Zod
- [ ] Schema versioned (v1, v2, etc.)
- [ ] Correlation ID included
- [ ] Timestamp included
- [ ] Producer documented
- [ ] Consumers documented

## 7. Implementation Checklist
- [ ] Event bus / channel initialized
- [ ] Producer emits with schema validation
- [ ] Consumer handles with idempotency check
- [ ] Dead letter handling for failures
- [ ] Correlation ID propagated
- [ ] Structured logging for emit + receive
- [ ] Cleanup on disconnect (for WebSocket/SSE)

## 8. Pre-Delivery Checklist
- [ ] All event schemas in EVENT_CATALOG.md
- [ ] Idempotency verified (duplicate events cause no side effects)
- [ ] Error handling: consumer failure → retry or DLQ
- [ ] Observability: events traceable via correlation ID
- [ ] Performance: no event storms, backpressure handled
- [ ] SYSTEM_STATE.md updated with new events/channels

## 9. Update SYSTEM_STATE.md (MANDATORY)
- New event types (name, payload, producer, consumers)
- New channels (WebSocket, SSE, Supabase Realtime)
- Event bus configuration
- Update timestamp

## 10. Exit
- Comment on in_progress work.
- HANDOFF to Backend Builder / Frontend Builder with event schemas.

## Rules
- EVERY event has a Zod schema. No untyped events.
- EVERY handler is idempotent. Test with duplicate delivery.
- ALWAYS include correlation IDs.
- NEVER silently drop failed events.
- EVENT_CATALOG.md is always current.
