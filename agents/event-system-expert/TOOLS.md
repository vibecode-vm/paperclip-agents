# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Skills

### 1. Context7 — context7 (am-will, 12K)
Fetch latest Supabase Realtime docs before implementing channels.

### 2. Database Best Practices — supabase-postgres-best-practices (32K)
Supabase Realtime works with postgres_changes — understand RLS implications.

### 3. Security — security-best-practices (11K)
Event channels must be authenticated. No public broadcast of sensitive data.

### 4. TDD for Events — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: Write event handler tests BEFORE implementing handlers.**

**Event TDD Cycle:**
1. 🔴 Define event schema (Zod) → write test: emit event → handler receives it with correct shape
2. 🔴 Run → MUST FAIL (handler doesn't exist)
3. 🟢 Implement handler with MINIMAL code
4. Test: invalid event → handler rejects gracefully
5. Test: missing fields → handler logs error, doesn't crash

**Defensive Event Patterns:**
- **Idempotency**: Handlers MUST be safe to call twice with same event
- **Ordering**: Don't assume events arrive in order — use timestamps or sequence numbers
- **Dead Letter**: Events that fail 3 times → dead letter queue for manual review
- **Schema versioning**: `v1`, `v2` — never break consumers

### 5. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

When events don't arrive or arrive late:
1. **Root Cause**: Is the event emitted? (check logs) Is the channel subscribed? (check Supabase dashboard) Is RLS blocking? (check policies)
2. **Pattern Analysis**: Compare working vs broken channel — what's different?
3. **Hypothesis Testing**: ONE change — different channel name? Different event type? Auth token expired?
4. **Fix**: At root cause, not symptom. Don't just "resubscribe on error".

---

## Patterns

### Custom Event Bus (TypeScript)
```typescript
// src/lib/events/event-bus.ts
import { z } from 'zod'
import { nanoid } from 'nanoid'

const baseEventSchema = z.object({
  id: z.string(),
  type: z.string(),
  timestamp: z.string().datetime(),
  correlationId: z.string(),
  payload: z.unknown(),
})

type EventHandler<T> = (event: T & { correlationId: string }) => Promise<void>

class EventBus {
  private handlers = new Map<string, EventHandler<any>[]>()

  on<T>(eventType: string, handler: EventHandler<T>) {
    const existing = this.handlers.get(eventType) || []
    this.handlers.set(eventType, [...existing, handler])
    return () => {
      const updated = (this.handlers.get(eventType) || []).filter(h => h !== handler)
      this.handlers.set(eventType, updated)
    }
  }

  async emit<T>(eventType: string, payload: T, correlationId?: string) {
    const cid = correlationId || nanoid()
    const event = { ...payload, correlationId: cid }
    console.log(`[EVENT] ${eventType}`, { correlationId: cid, payload })

    const handlers = this.handlers.get(eventType) || []
    await Promise.allSettled(handlers.map(h => h(event)))
  }
}

export const eventBus = new EventBus()
```

### Supabase Realtime Channel
```typescript
// src/lib/events/realtime.ts
import { createClient } from '@/lib/supabase/client'

export function subscribeToTable(
  table: string,
  event: 'INSERT' | 'UPDATE' | 'DELETE' | '*',
  callback: (payload: any) => void
) {
  const supabase = createClient()

  return supabase
    .channel(`${table}-changes`)
    .on('postgres_changes', { event, schema: 'public', table }, callback)
    .subscribe()
}
```

### Server-Sent Events (SSE)
```typescript
// src/app/api/events/stream/route.ts
export async function GET() {
  const encoder = new TextEncoder()

  const stream = new ReadableStream({
    start(controller) {
      const send = (event: string, data: unknown) => {
        controller.enqueue(encoder.encode(`event: ${event}\ndata: ${JSON.stringify(data)}\n\n`))
      }

      // Subscribe to events and forward
      const unsub = eventBus.on('notification', (e) => {
        send('notification', e)
      })

      // Cleanup on disconnect
      return () => unsub()
    },
  })

  return new Response(stream, {
    headers: {
      'Content-Type': 'text/event-stream',
      'Cache-Control': 'no-cache',
      'Connection': 'keep-alive',
    },
  })
}
```

---

## Templates

### EVENT_CATALOG.md
```markdown
# Event Catalog

> Last updated: [timestamp]

## Events

| Event Type | Version | Payload Schema | Producers | Consumers | Pattern |
|-----------|---------|---------------|-----------|-----------|---------|
| `user.created` | v1 | `UserCreatedPayload` | Auth Service | Email, Analytics, Onboarding | Pub/Sub |
| `order.completed` | v1 | `OrderCompletedPayload` | Checkout | Inventory, Notification, Analytics | Pub/Sub |

## Channels

| Channel | Type | Auth Required | Purpose |
|---------|------|--------------|---------|
| `users-changes` | Supabase Realtime | Yes | User table changes |
| `notifications` | SSE | Yes | Real-time user notifications |

## Event Schemas

### user.created (v1)
\`\`\`typescript
const userCreatedSchema = z.object({
  userId: z.string().uuid(),
  email: z.string().email(),
  createdAt: z.string().datetime(),
})
\`\`\`
```

---

## Shared Config

Read `../SHARED_CONFIG.md` for:
- Mandated stack (Supabase, Zod)
- Shared rules
