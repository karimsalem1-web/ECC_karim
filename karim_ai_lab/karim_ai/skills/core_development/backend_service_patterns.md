# Backend Service Patterns

Purpose:
Help agents structure backend work into clear, testable layers without moving sensitive logic into the frontend or hiding business rules inside route handlers.

Backend skills do not override backend rules.
Before using this skill, read `rules/backend.md`.
For API routes, also read `rules/api_routes.md`.
For database work, also read `rules/database.md`.
For sensitive work, also read `rules/security.md`.

## When to Activate

Use this skill for:

- backend services
- server-side business logic
- repository, service, or provider patterns
- webhooks
- background jobs
- queues
- caching
- query optimization
- private integrations
- AI/provider calls
- credit, billing, or payment backend behavior

Do not activate this skill for tiny text edits or simple UI-only changes.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/backend.md`
5. `rules/api_routes.md` if routes or server actions are involved
6. `rules/database.md` if data is read or written
7. `rules/security.md` if auth, secrets, payments, credits, customer data, AI providers, webhooks, uploads, or tenant data are involved
8. `memory/INDEX.md` and relevant memory files if project behavior matters
9. Existing backend files related to the task

## Execution Pattern

Preferred backend flow:

```text
route/server action/webhook
-> validate request
-> authenticate/authorize
-> call service
-> service applies business logic
-> repository handles database access
-> provider client handles external APIs
-> return safe response
-> log important events
-> update memory if project behavior changed
```

Keep route handlers thin.

Route handlers should:

- parse the request
- validate request shape
- call auth helpers
- call one service function
- convert service results into safe responses

Service functions should:

- apply business rules
- coordinate repositories and providers
- handle idempotency and state transitions
- enforce credit/payment/business sequencing with backend authority
- return typed results or domain errors

Repositories should:

- contain database access
- select only needed columns
- avoid leaking storage-specific details into services
- handle query composition
- keep tenant filters and ownership checks explicit

Provider clients should:

- wrap external APIs
- keep secrets server-side
- normalize provider responses
- convert provider failures into safe internal errors
- avoid exposing raw provider errors to users

## Practical Checklist

Before implementation:

- Identify the entry point: route, server action, webhook, job, or internal service.
- Identify protected data, tenant context, ownership fields, and auth needs.
- Identify the service boundary and avoid mixing business logic into route files.
- Identify database tables, queries, and repository helpers.
- Identify external providers and required private env vars.
- Confirm approval gates for billing, credits, payments, destructive actions, or production data.

While implementing:

- Validate inputs before calling services.
- Authenticate and authorize before protected work.
- Keep service functions small and named by business action.
- Use repositories for data access instead of scattering queries.
- Use provider clients for external calls instead of inline fetch/client code everywhere.
- Use transactions or equivalent consistency controls for multi-step critical writes.
- Prevent duplicate processing for webhooks, paid actions, AI jobs, and background jobs.
- Cache only data that is safe to cache and has a clear invalidation path.
- Optimize queries by selecting needed columns, batching lookups, and avoiding N+1 patterns.
- Log important events with safe context, never secrets or raw private data.
- Return safe user-facing errors and detailed server-side logs.

For webhooks:

- Verify webhook authenticity when the provider supports it.
- Treat webhook payloads as untrusted input.
- Make processing idempotent.
- Store provider event IDs when duplicate delivery is possible.
- Acknowledge quickly and move long work to a job when needed.

For background jobs:

- Persist durable jobs when failure or retry matters.
- Track status for long-running work.
- Make jobs retry-safe.
- Avoid in-memory-only queues for production-critical work.

For AI/provider calls:

- Keep provider keys server-side.
- Validate user input and file metadata before paid calls.
- Track cost, credits, and ownership on the backend.
- Store only safe, needed provider output.
- Normalize model/provider errors before returning to frontend.

## Stop Conditions

Stop and ask Karim if:

- the business rule is unclear
- authorization ownership is unclear
- a backend route or service would affect billing, credits, payments, customers, orders, or production data
- webhook authenticity cannot be verified
- retry/idempotency behavior is unclear
- a provider call requires a new secret or paid service
- a database transaction or consistency model is needed but unclear
- the change would move sensitive logic into frontend code

## Output Expectations

When reporting backend work:

- Name the backend entry points changed.
- Name the service/repository/provider boundaries used.
- Mention auth, tenant, and sensitive-data implications.
- Mention database or provider dependencies.
- Mention verification performed.
- Mention memory updates if project behavior changed.
