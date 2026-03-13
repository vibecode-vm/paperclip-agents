# Tools

## Runtime
- **OpenCode** is your runtime.

## Paperclip API
- `GET /api/agents/me` — identity.
- `GET /api/companies/{companyId}/issues?assigneeAgentId={agentId}&status=todo,in_progress,blocked` — inbox.
- `PATCH /api/issues/{issueId}` — status updates. Include `X-Paperclip-Run-Id`.

---

## Test Stack (MANDATORY)

| Package | Purpose | Version |
|---------|---------|---------|
| `vitest` | Test runner | latest |
| `@testing-library/react` | Component testing | latest |
| `@testing-library/jest-dom` | Custom matchers | latest |
| `@testing-library/user-event` | User interaction simulation | latest |
| `msw` | API mocking (Mock Service Worker) | latest |
| `@vitejs/plugin-react` | React support for Vitest | latest |

### Setup Check
```bash
# Verify test stack is installed
npm list vitest @testing-library/react msw --depth=0

# Run tests
npx vitest run

# Run tests with coverage
npx vitest run --coverage

# Run specific test file
npx vitest run components/Button/Button.test.tsx

# Watch mode (for development)
npx vitest watch
```

---

## Skills

### 1. Test-Driven Development — test-driven-development (obra/superpowers, 23K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**THE IRON LAW:**
```
NO PRODUCTION CODE WITHOUT A FAILING TEST FIRST
```
Write code before the test? **Delete it. Start over.** No exceptions — don't keep it as "reference," don't "adapt" it. Delete means delete.

**RED-GREEN-REFACTOR Cycle:**

| Phase | Action | Verify |
|-------|--------|--------|
| 🔴 RED | Write ONE failing test (one behavior, clear name, real code) | Run test → MUST FAIL. If it passes, you're testing existing behavior. Fix test. |
| 🟢 GREEN | Write MINIMAL code to pass (no extras, no "improvements") | Run ALL tests → ALL MUST PASS. Test fails? Fix code, not test. |
| 🔵 REFACTOR | Remove duplication, improve names, extract helpers | Run ALL tests → STILL PASS. Don't add behavior. |
| ↻ REPEAT | Next failing test for next behavior | — |

**Good vs Bad Tests (Superpowers):**
```typescript
// ✅ GOOD: Clear name, tests real behavior, one thing
test('retries failed operations 3 times', async () => {
  let attempts = 0;
  const operation = () => { attempts++; if (attempts < 3) throw new Error('fail'); return 'success'; };
  const result = await retryOperation(operation);
  expect(result).toBe('success');
  expect(attempts).toBe(3);
});

// ❌ BAD: Vague name, tests mock not code
test('retry works', async () => {
  const mock = jest.fn().mockRejectedValueOnce(new Error()).mockResolvedValueOnce('success');
  await retryOperation(mock);
  expect(mock).toHaveBeenCalledTimes(2);
});
```

**Common Rationalizations (ALL wrong):**
- "I'll just quickly write this, then add tests" → Delete it.
- "This is too simple to test" → Simple code breaks too.
- "I know the implementation, so I'll write both" → You'll write tests that pass, not tests that verify.

**Plan before writing tests:**
- List all component props and their expected behavior
- List all user interactions and their expected outcomes
- List all edge cases (empty, error, loading, overflow)
- Write one test per behavior

### 2. Web App Testing — webapp-testing (anthropics, 22K)

**Testing patterns for React/Next.js:**
- Use `render()` to mount components in a test environment.
- Use `screen.getByRole()`, `screen.getByLabelText()` for accessible queries.
- Use `userEvent` for realistic user interactions (clicks, typing).
- Use `waitFor()` for async operations.
- Mock API calls with MSW, not manual fetch mocks.

### 3. Systematic Debugging — systematic-debugging (obra/superpowers, 28K)

> Source: [obra/superpowers](https://github.com/obra/superpowers)

**IRON LAW: NO FIXES WITHOUT ROOT CAUSE INVESTIGATION FIRST.**

**4-Phase Process (when tests fail unexpectedly):**
1. **Root Cause Investigation** — Read error message CAREFULLY (line numbers, stack traces). Reproduce consistently. Check recent changes (git diff). Trace data flow backward.
2. **Pattern Analysis** — Find working examples in same codebase. Compare: what's different? Don't assume "that can't matter."
3. **Hypothesis Testing** — Form SINGLE hypothesis, make SMALLEST possible change, verify. Didn't work? Form NEW hypothesis. DON'T stack fixes.
4. **Implementation** — Fix at ROOT CAUSE, not at symptom. Verify with evidence.

**Red Flags — STOP and follow process:**
- "Let me just try this..." → You're guessing. Stop.
- Multiple fixes without verifying → You're thrashing. Stop.
- Same error keeps coming back → You're fixing symptoms. Stop.

### 4. Verification — verification-before-completion (obra, 16K)

- Run `npx vitest run` before reporting results.
- Read the FULL output (total, passed, failed, skipped).
- Only report PASS if exit code is 0 and all tests pass.

---

## Testing Patterns

### Component Render Test
```tsx
import { render, screen } from '@testing-library/react'
import { Button } from './Button'

describe('Button', () => {
  it('renders with label text', () => {
    render(<Button>Click me</Button>)
    expect(screen.getByRole('button', { name: 'Click me' })).toBeInTheDocument()
  })

  it('renders disabled state', () => {
    render(<Button disabled>Click me</Button>)
    expect(screen.getByRole('button')).toBeDisabled()
  })

  it('applies variant classes', () => {
    render(<Button variant="outline">Click me</Button>)
    expect(screen.getByRole('button')).toHaveClass('outline')
  })
})
```

### User Interaction Test
```tsx
import { render, screen } from '@testing-library/react'
import userEvent from '@testing-library/user-event'

describe('LoginForm', () => {
  it('submits form with valid data', async () => {
    const onSubmit = vi.fn()
    render(<LoginForm onSubmit={onSubmit} />)

    await userEvent.type(screen.getByLabelText('Email'), 'test@example.com')
    await userEvent.type(screen.getByLabelText('Password'), 'password123')
    await userEvent.click(screen.getByRole('button', { name: 'Sign in' }))

    expect(onSubmit).toHaveBeenCalledWith({
      email: 'test@example.com',
      password: 'password123',
    })
  })

  it('shows error for invalid email', async () => {
    render(<LoginForm onSubmit={vi.fn()} />)

    await userEvent.type(screen.getByLabelText('Email'), 'not-an-email')
    await userEvent.click(screen.getByRole('button', { name: 'Sign in' }))

    expect(screen.getByText(/invalid email/i)).toBeInTheDocument()
  })
})
```

### API Mock Test (MSW)
```tsx
import { render, screen, waitFor } from '@testing-library/react'
import { http, HttpResponse } from 'msw'
import { setupServer } from 'msw/node'
import { UserList } from './UserList'

const server = setupServer(
  http.get('/api/users', () => {
    return HttpResponse.json([
      { id: 1, name: 'Alice' },
      { id: 2, name: 'Bob' },
    ])
  })
)

beforeAll(() => server.listen())
afterEach(() => server.resetHandlers())
afterAll(() => server.close())

describe('UserList', () => {
  it('displays users from API', async () => {
    render(<UserList />)
    await waitFor(() => {
      expect(screen.getByText('Alice')).toBeInTheDocument()
      expect(screen.getByText('Bob')).toBeInTheDocument()
    })
  })

  it('shows error when API fails', async () => {
    server.use(
      http.get('/api/users', () => HttpResponse.error())
    )
    render(<UserList />)
    await waitFor(() => {
      expect(screen.getByText(/error/i)).toBeInTheDocument()
    })
  })
})
```

### Zustand Store Test
```tsx
import { renderHook, act } from '@testing-library/react'
import { useAuthStore } from './authStore'

describe('useAuthStore', () => {
  beforeEach(() => {
    useAuthStore.setState({ user: null, isAuthenticated: false })
  })

  it('sets user on login', () => {
    const { result } = renderHook(() => useAuthStore())
    act(() => result.current.login({ id: 1, name: 'Alice' }))
    expect(result.current.user).toEqual({ id: 1, name: 'Alice' })
    expect(result.current.isAuthenticated).toBe(true)
  })

  it('clears user on logout', () => {
    const { result } = renderHook(() => useAuthStore())
    act(() => result.current.login({ id: 1, name: 'Alice' }))
    act(() => result.current.logout())
    expect(result.current.user).toBeNull()
    expect(result.current.isAuthenticated).toBe(false)
  })
})
```

### Custom Hook Test
```tsx
import { renderHook, waitFor } from '@testing-library/react'
import { useDebounce } from './useDebounce'

describe('useDebounce', () => {
  it('returns debounced value after delay', async () => {
    const { result, rerender } = renderHook(
      ({ value }) => useDebounce(value, 500),
      { initialProps: { value: 'initial' } }
    )

    rerender({ value: 'updated' })
    expect(result.current).toBe('initial') // Not yet updated

    await waitFor(() => {
      expect(result.current).toBe('updated') // After debounce
    }, { timeout: 600 })
  })
})
```

---

## Test File Naming Convention

```
components/
├── ui/              # shadcn components — DO NOT test (tested upstream)
├── feature-name/
│   ├── FeatureName.tsx
│   ├── FeatureName.test.tsx     ← Component test
│   ├── useFeatureName.ts
│   └── useFeatureName.test.ts   ← Hook test
stores/
│   ├── featureStore.ts
│   └── featureStore.test.ts     ← Store test
```

## Rules
- DO NOT test shadcn/ui components — they are tested upstream.
- Use `screen.getByRole()` and `getByLabelText()` over `getByTestId()`.
- Use `userEvent` over `fireEvent` for realistic interactions.
- Mock API with MSW, never mock `fetch` directly.
- One test file per source file.
- Run ALL tests before reporting.
