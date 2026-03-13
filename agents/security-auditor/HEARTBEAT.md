# Heartbeat — Security Auditor

## OWASP Top 10 Checklist

### A01: Broken Access Control ⬜
- [ ] Auth required on every non-public endpoint
- [ ] Row-Level Security (RLS) on Supabase tables
- [ ] Users cannot access other users' data (IDOR check)
- [ ] Admin endpoints require admin role check
- [ ] CORS origin whitelist (not `*`)

### A02: Cryptographic Failures ⬜
- [ ] Passwords hashed (bcrypt/argon2, NEVER plain text)
- [ ] Secrets in env vars, not in code
- [ ] HTTPS enforced everywhere
- [ ] Sensitive data not logged

### A03: Injection ⬜
- [ ] All DB queries use parameterized queries (Drizzle ORM ✅)
- [ ] No `eval()`, `new Function()`, or `innerHTML` with user input
- [ ] No string concatenation in SQL/NoSQL queries
- [ ] HTML sanitization on all user-generated content display

### A04: Insecure Design ⬜
- [ ] Rate limiting on auth endpoints
- [ ] Account lockout after N failed attempts
- [ ] CSRF protection on state-changing requests
- [ ] Input length limits on all fields

### A05: Security Misconfiguration ⬜
- [ ] Security headers set (CSP, HSTS, X-Frame-Options, X-Content-Type)
- [ ] Default credentials removed
- [ ] Error messages don't leak stack traces
- [ ] Debug mode disabled in production

### A06: Vulnerable Components ⬜
- [ ] `npm audit` — 0 critical, 0 high
- [ ] No abandoned dependencies (last update > 2 years)
- [ ] `package-lock.json` committed (deterministic installs)

### A07: Authentication Failures ⬜
- [ ] Session management handled by Supabase Auth (not custom)
- [ ] Tokens expire and refresh properly
- [ ] Logout invalidates session

### A08: Data Integrity ⬜
- [ ] Zod validation on ALL API inputs
- [ ] File upload validation (type, size, content)
- [ ] No mass assignment (schema uses `.pick()` for allowed fields)

### A09: Logging & Monitoring ⬜
- [ ] Auth events logged (login, failed login, logout)
- [ ] API errors logged with request context (not user data)
- [ ] No sensitive data in logs

### A10: SSRF ⬜
- [ ] External URL fetches validate/whitelist destination
- [ ] No user-controlled redirect URLs without validation
