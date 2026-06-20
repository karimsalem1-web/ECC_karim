# Security Review

Purpose:
Help agents perform a practical security review before completing sensitive work, with attention to secrets, auth, tenant isolation, APIs, database access, uploads, payments, credits, AI providers, webhooks, logs, and frontend exposure.

Security skills do not override security rules.
Before using this skill, read `rules/security.md`.
For API routes, also read `rules/api_routes.md`.
For backend work, also read `rules/backend.md`.
For database/RLS work, also read `rules/database.md`.
For frontend exposure, also read `rules/frontend.md`.

## When to Activate

Use this skill before completion of sensitive work involving:

- secrets or environment variables
- authentication
- authorization
- tenant isolation
- RLS
- API routes
- user input validation
- file uploads
- payments
- credits
- AI provider calls
- webhooks
- logging and errors
- frontend exposure
- dependency or config risk

Security review should be activated before completion of sensitive work, not only after bugs happen.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/security.md`
5. `rules/api_routes.md` if routes, webhooks, callbacks, or server actions are involved
6. `rules/backend.md` if backend services, provider calls, payments, credits, jobs, or private integrations are involved
7. `rules/database.md` if schema, RLS, queries, ownership fields, or tenant data are involved
8. `rules/frontend.md` if browser exposure, UI auth state, uploads, or customer-facing screens are involved
9. `memory/INDEX.md` and relevant memory files if project security behavior matters
10. The actual code and config touched by the task

## Execution Pattern

Use this security review flow:

```text
identify sensitive surface
-> identify attacker/user-controlled inputs
-> check auth and authorization
-> check tenant/ownership boundary
-> check secrets and env exposure
-> check database/RLS impact
-> check API/webhook/payment/credit/idempotency risk
-> check file upload or AI provider risk if involved
-> check logs and errors for leaks
-> verify fix or stop for approval
-> update memory if stable project security behavior changed
```

## Practical Checklist

Sensitive surface:

- Identify what the change can read, write, trigger, spend, publish, delete, or expose.
- Identify user roles, tenant boundaries, admin surfaces, and customer-facing surfaces.
- Identify whether production data, customer data, order data, billing, credits, payments, uploads, webhooks, or AI providers are involved.

User-controlled inputs:

- Validate body, params, query strings, headers, files, webhook payloads, and provider callbacks.
- Use allowlists for enums, sort fields, filters, file types, redirect targets, and callback actions.
- Never trust frontend-submitted owner IDs, tenant IDs, roles, prices, credit amounts, payment state, or admin flags.
- Avoid raw user input in SQL, shell commands, templates, logs, or provider prompts without the right validation and escaping.

Secrets and env:

- Do not store or expose secrets.
- Keep private keys, API keys, webhook secrets, service-role keys, payment secrets, and AI provider keys server-side.
- Do not put private secrets in public env vars.
- Do not log secrets or secret-bearing URLs.
- Validate required secrets at startup or at the integration boundary when appropriate.

Auth and authorization:

- Confirm authentication is required where needed.
- Confirm authorization checks happen before sensitive work.
- Confirm hidden UI is not the only protection.
- Confirm admin-only actions are enforced on the backend.
- Confirm ownership checks are server-side and cannot be bypassed by changing client state.

Tenant isolation and RLS:

- Confirm tenant/account/shop/user ownership fields are enforced.
- Confirm queries include the correct ownership boundary.
- Confirm RLS policies match the intended access model when Supabase/Postgres RLS is used.
- Confirm cached data cannot leak between tenants.
- Confirm tenant switching clears or refreshes stale frontend state when relevant.

API, webhook, payment, and credit risks:

- Verify API routes return safe errors and correct status codes.
- Verify paid actions, destructive actions, auth changes, RLS changes, and production-data changes have approval gate.
- Verify payment and credit truth is backend-owned.
- Verify idempotency for duplicate submits, payment retries, webhook retries, AI jobs, and destructive actions.
- Never trust webhook payloads without verification when provider supports it.
- Store provider event IDs when duplicate delivery is possible.

File uploads:

- Validate file type, extension, size, and count.
- Confirm backend validation exists even if frontend validates first.
- Avoid public storage for private files unless the product requires it.
- Avoid exposing private storage paths.
- Treat uploaded content as untrusted input.

AI provider calls:

- Keep provider keys server-side.
- Validate and minimize data sent to AI providers.
- Do not send unnecessary private data.
- Track cost, credits, and ownership on the backend.
- Avoid exposing raw model/provider errors.
- Review storage of prompts, outputs, embeddings, files, and logs for privacy risk.

Logging and errors:

- Never expose raw provider errors, stack traces, SQL errors, or secret-bearing data.
- Keep user-facing errors safe and actionable.
- Log enough server-side context to debug without leaking secrets, raw customer data, tokens, or payment details.
- Redact sensitive fields in structured logs.

Frontend exposure:

- Never trust frontend for protected state.
- Confirm frontend code does not contain private env vars or keys.
- Confirm frontend only displays data returned by authorized backend paths.
- Confirm protected UI actions are backed by server-side checks.

Dependency and config risk:

- Check for newly added dependencies and whether they are necessary.
- Check package lockfile changes when dependencies change.
- Check CORS, CSP, cookies, redirects, and public asset/storage config when affected.
- Check that local/staging/production differences are intentional.

## Stop Conditions

Stop and ask Karim if:

- a secret may have been exposed
- auth or authorization behavior is unclear
- tenant isolation or RLS behavior is unclear
- the change affects production data, customers, billing, payments, credits, admin access, or destructive actions
- webhook verification cannot be confirmed
- idempotency is unclear for paid, destructive, or retry-prone actions
- file upload limits or storage privacy are unclear
- AI provider data handling, cost, or credit behavior is unclear
- a security fix requires rotating secrets or changing production config
- the review finds a critical or high-risk issue outside the approved scope

## Output Expectations

When reporting security review work:

- Name the sensitive surfaces reviewed.
- Summarize auth, authorization, tenant, and RLS implications.
- Summarize secret/env exposure checks.
- Summarize API, webhook, payment, credit, upload, or AI risks reviewed when relevant.
- State whether any approval gate is required.
- State whether logs and errors are safe.
- Mention verification performed.
- Mention memory updates if stable project security behavior changed.
