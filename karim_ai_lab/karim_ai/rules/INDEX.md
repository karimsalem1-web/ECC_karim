# Rules Index

Purpose:
Help agents choose the correct technical rule files before working inside Karim AI Lab or any project using Karim AI Lab.

This folder contains task-specific technical rules.

Agents should not load every rule file automatically.
Read only the rule files relevant to the current task unless the task is broad, architectural, risky, or unclear.

## Rule Files

- `architecture.md`  
  System design, folder structure, module boundaries, feature boundaries, and data flow.

- `security.md`  
  Secrets, authentication, authorization, validation, logs, sensitive data protection, and safe coding.

- `database.md`  
  Schema design, migrations, RLS, indexes, ownership fields, vector storage, audit logs, and multi-tenant data separation.

- `frontend.md`  
  UI structure, components, forms, state, API calls, loading/error states, client/server boundaries, and user flows.

- `backend.md`  
  Server logic, services, webhooks, private integrations, background jobs, queues, AI/provider calls, billing, and protected operations.

- `api_routes.md`  
  Route design, request/response contracts, validation, auth checks, webhooks, callbacks, safe errors, and endpoint behavior.

- `naming.md`  
  Naming conventions for files, folders, variables, functions, tables, columns, routes, env vars, and agents.

- `environment.md`  
  Local/staging/production config, `.env` rules, public/private env boundaries, and secret handling.

## What to Read by Task Type

### New feature

Read:
- `architecture.md`
- relevant area file: `frontend.md`, `backend.md`, `database.md`, or `api_routes.md`
- `security.md` if protected data, auth, secrets, payments, credits, AI providers, or customer data are involved
- `environment.md` if new env vars are needed
- `naming.md` if new files, tables, routes, or modules are created

### Bug fix

Read:
- the rule file matching the broken area
- `security.md` if the bug touches auth, permissions, secrets, tenant data, payments, or customer data
- `database.md` if the bug touches schema, queries, migrations, RLS, or data ownership
- `api_routes.md` if the bug touches request/response behavior

### Refactor

Read:
- `architecture.md`
- the affected technical rule file
- `naming.md`
- `security.md` if auth, private data, or secrets may be affected

### Database change

Read:
- `database.md`
- `security.md`
- `backend.md` if backend queries/services are affected
- `api_routes.md` if API contracts are affected
- `environment.md` if database/env config changes

### Backend change

Read:
- `backend.md`
- `security.md`
- `database.md` if data is read/written
- `api_routes.md` if routes are exposed or changed
- `environment.md` if private env vars are used

### API route change

Read:
- `api_routes.md`
- `backend.md`
- `security.md`
- `database.md` if the route reads/writes data
- `environment.md` if secrets or env vars are used

### Frontend change

Read:
- `frontend.md`
- `api_routes.md` if APIs are called
- `security.md` if auth, protected data, payments, credits, AI providers, or customer data are involved
- `backend.md` if the frontend depends on protected backend behavior

### Security-sensitive work

Read:
- `security.md`
- `environment.md`
- plus the affected area file

Security-sensitive work includes:
- authentication
- authorization
- secrets
- env vars
- RLS
- payments
- credits
- customer data
- order data
- AI provider keys
- webhooks
- file uploads
- admin access
- tenant isolation

### AI/provider work

Read:
- `backend.md`
- `api_routes.md`
- `security.md`
- `environment.md`
- `database.md` if AI outputs, logs, memory, vectors, or costs are stored
- `frontend.md` if user-facing AI UI is involved

### Shopify / e-commerce work

Read:
- `backend.md`
- `api_routes.md`
- `security.md`
- `database.md`
- `frontend.md` if storefront, dashboard, extension, app proxy, product, customer, order, refund, exchange, or billing UI is affected

## Token Optimization Rule

Do not load all rule files by default.

Use this selection pattern:

1. Start with the task type.
2. Read the matching rule file.
3. Add `security.md` if protected or sensitive data is involved.
4. Add `database.md` if data structure, queries, RLS, ownership, or migrations are involved.
5. Add `environment.md` if env vars or secrets are involved.
6. Add `architecture.md` if the task changes structure or flow.
7. Add `naming.md` if new names are created.

## Stop and Escalate

If the task touches multiple areas and the correct rule set is unclear, stop and ask Karim before editing.

If the task may affect production data, customers, billing, credits, authentication, authorization, or tenant isolation, treat it as high-risk and follow approval gates.

After updating rules/INDEX.md:
- Do not modify any other rule file.
- Do not modify files outside karim_ai_lab.
- Report only what changed and confirm that only rules/INDEX.md was edited.
