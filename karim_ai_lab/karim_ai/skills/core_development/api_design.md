# API Design

Purpose:
Help agents design clear API routes and contracts that are consistent, safe, and understandable for frontend, backend, webhook, and integration work.

API design skills do not override API route rules.
Before using this skill, read `rules/api_routes.md`.
For backend logic, also read `rules/backend.md`.
For database changes, also read `rules/database.md`.
For sensitive routes, also read `rules/security.md`.

## When to Activate

Use this skill for:

- new API routes
- route contract changes
- request and response shape design
- public/customer-facing APIs
- internal APIs
- webhooks and callbacks
- pagination, filtering, or sorting
- safe error handling
- idempotent paid or destructive actions

Public/customer-facing APIs need stable contracts.
Internal APIs can be simpler but still must be clear.
Webhook endpoints must verify authenticity where provider supports it.
Paid or destructive API actions require approval gate.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/api_routes.md`
5. `rules/backend.md`
6. `rules/database.md` if the route reads or writes data
7. `rules/security.md` if the route touches auth, tenant data, secrets, payments, credits, uploads, customer data, webhooks, or admin behavior
8. `memory/INDEX.md` and relevant memory files if route contracts or project behavior matter
9. Existing route files and API clients for local conventions

## Execution Pattern

Design the route before implementing it:

```text
identify caller
-> choose public/internal/webhook/callback boundary
-> choose route name and method
-> define request schema
-> define auth and authorization checks
-> define service/backend action
-> define response envelope
-> define status codes and safe errors
-> define idempotency, pagination, filtering, or sorting if needed
-> update memory if the API contract becomes stable project knowledge
```

## Practical Checklist

Route naming:

- Use resource-oriented names.
- Prefer nouns over action verbs.
- Keep route names consistent with the existing project.
- Use nested routes only when ownership or hierarchy is real.
- Avoid exposing internal provider names unless that is the product contract.

HTTP method semantics:

- Use `GET` for reads.
- Use `POST` for creates, submitted actions, webhooks, and non-idempotent operations.
- Use `PUT` for full replacement when the project already uses it.
- Use `PATCH` for partial updates.
- Use `DELETE` for deletion or removal when allowed by the product flow.

Request validation:

- Validate body, params, query, and headers.
- Reject unknown or unsafe payload shapes when appropriate.
- Validate IDs, ownership hints, pagination values, enum values, and file metadata.
- Never trust client-provided tenant, owner, price, credit, role, or payment state.

Response shape:

- Follow the existing project response format.
- If no format exists, use a consistent envelope with success, message, data, and error.
- Keep public/customer-facing responses stable.
- Return only fields the caller needs.
- Do not expose stack traces, SQL errors, provider raw errors, or secret-bearing data.

Status codes:

- `200` for successful reads or updates with a body.
- `201` for successful creation.
- `202` for accepted async work.
- `204` for successful deletion with no body.
- `400` or `422` for validation problems, following project convention.
- `401` for missing or invalid auth.
- `403` for authenticated but unauthorized.
- `404` for missing resources.
- `409` for duplicate, conflict, or invalid state transitions.
- `429` for rate limiting.
- `500` only for unexpected server failures.

Pagination, filtering, and sorting:

- Paginate list endpoints that can grow.
- Use cursor pagination for large or infinite lists.
- Use offset/page pagination for small admin screens when acceptable.
- Validate filter and sort fields against an allowlist.
- Keep search query behavior explicit.

Webhooks and callbacks:

- Verify signatures or authenticity when available.
- Treat payloads as untrusted input.
- Make processing idempotent.
- Store external event IDs when duplicate delivery is possible.
- Return quickly and defer long work to jobs.
- Never expose private webhook secrets in frontend code.

Internal vs public APIs:

- Public APIs need stable naming, response shape, and backwards compatibility.
- Internal APIs can be narrower and simpler.
- Internal APIs still need validation, auth, safe errors, and clear contracts.
- Do not version APIs until there is a real compatibility need.

Idempotency:

- Use idempotency keys or event IDs for paid, destructive, or retry-prone actions.
- Avoid duplicate credit deduction, payment capture, order changes, AI jobs, and webhook processing.
- Make retries safe where clients or providers may repeat calls.

## Stop Conditions

Stop and ask Karim if:

- the caller or user flow is unclear
- the route affects billing, payments, credits, customers, orders, production data, or tenant isolation
- the API should be public but stability requirements are unclear
- the response format conflicts with existing project conventions
- webhook verification cannot be confirmed
- idempotency requirements are unclear
- the route needs a new backend service, database shape, or env secret that was not approved

## Output Expectations

When reporting API work:

- Name routes created or changed.
- State public, internal, webhook, or callback boundary.
- Summarize request and response contract.
- Mention auth, authorization, idempotency, and safe-error behavior.
- Mention backend/database dependencies.
- Mention memory updates if a route contract became stable project knowledge.
