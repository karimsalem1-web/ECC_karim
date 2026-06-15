# Rules Index

This folder contains technical-area rules for agents working inside Karim AI Lab.

Core rules define global behavior. Rules define how to work in specific technical areas.

Agents should read only the rule files relevant to the current task.

Do not load every rule file automatically unless the task is broad, architectural, or unclear.

## Navigation

- `architecture.md` - system design, folder structure, module boundaries, and data flow
- `security.md` - secrets, authentication, authorization, validation, logs, and sensitive data protection
- `database.md` - schema design, migrations, RLS, indexes, and multi-tenant data separation
- `frontend.md` - UI structure, components, forms, state, and client/server boundaries
- `backend.md` - server logic, services, webhooks, private integrations, and background jobs
- `api_routes.md` - route design, request/response format, validation, auth checks, and error handling
- `naming.md` - naming conventions for files, folders, variables, tables, columns, routes, and env vars
- `environment.md` - local/staging/production config, `.env` rules, and secret handling
