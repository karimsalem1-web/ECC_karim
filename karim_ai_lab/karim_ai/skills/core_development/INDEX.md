# Core Development Skills Index

Purpose:
Core Development Skills help agents execute normal software development work with better structure, safer patterns, and clearer verification.

This section is not one skill.
It is a category containing seven planned skill files.

## Current Status

Core Development Skills are being built from ECC source material.

The planned skill files are:

- `backend_service_patterns.md`
- `frontend_ui_patterns.md`
- `api_design.md`
- `database_migrations.md`
- `security_review.md`
- `testing_verification.md`
- `deployment_ops.md`

Do not create or fill these files until Karim approves the next prompts.

## Planned Skills

### `backend_service_patterns.md`

Purpose:
Help agents structure backend code cleanly.

ECC source inspiration:

```text
skills/backend-patterns/SKILL.md
```

Use for:

- backend services
- server-side business logic
- repository/service/provider patterns
- background jobs
- webhooks
- caching
- query optimization

### `frontend_ui_patterns.md`

Purpose:
Help agents build frontend UI flows and components cleanly.

ECC source inspiration:

```text
skills/frontend-patterns/SKILL.md
```

Use for:

- React / Next.js components
- UI composition
- forms
- loading and error states
- responsive layouts
- customer-facing flows
- dashboard screens

### `api_design.md`

Purpose:
Help agents design clear and consistent API routes and contracts.

ECC source inspiration:

```text
skills/api-design/SKILL.md
```

Use for:

- new endpoints
- route contracts
- HTTP methods
- status codes
- request/response shapes
- pagination
- filtering
- webhooks and callbacks

### `database_migrations.md`

Purpose:
Help agents plan and execute database changes safely.

ECC source inspiration:

```text
skills/database-migrations/SKILL.md
```

Use for:

- creating tables
- altering schema
- adding indexes
- data backfills
- migration safety
- rollback planning
- production-safe database changes

### `security_review.md`

Purpose:
Help agents review sensitive work before completion.

ECC source inspiration:

```text
skills/security-review/SKILL.md
```

Use for:

- auth
- authorization
- secrets
- API routes
- file uploads
- customer data
- payments
- third-party integrations
- RLS and tenant isolation

### `testing_verification.md`

Purpose:
Help agents verify work before saying it is done.

ECC source inspiration:

```text
skills/verification-loop/SKILL.md
skills/tdd-workflow/SKILL.md
skills/e2e-testing/SKILL.md
skills/eval-harness/SKILL.md
```

Use for:

- build checks
- type checks
- lint checks
- tests
- security smoke checks
- diff review
- E2E when needed
- high-risk feature verification

### `deployment_ops.md`

Purpose:
Help agents prepare deployment and production operations safely.

ECC source inspiration:

```text
skills/deployment-patterns/SKILL.md
skills/docker-patterns/SKILL.md
```

Use for:

- deployment readiness
- CI/CD
- environment differences
- Docker when relevant
- health checks
- rollback planning
- production release review

## How to Choose a Skill

Backend task:

```text
skills/core_development/backend_service_patterns.md
```

Frontend task:

```text
skills/core_development/frontend_ui_patterns.md
```

API route task:

```text
skills/core_development/api_design.md
```

Database schema task:

```text
skills/core_development/database_migrations.md
```

Security-sensitive task:

```text
skills/core_development/security_review.md
```

Testing or final verification task:

```text
skills/core_development/testing_verification.md
```

Deployment or production readiness task:

```text
skills/core_development/deployment_ops.md
```

## Multi-Skill Tasks

Some tasks need more than one skill.

Examples:

New API with database change:

```text
api_design.md
backend_service_patterns.md
database_migrations.md
testing_verification.md
```

Frontend form connected to API:

```text
frontend_ui_patterns.md
api_design.md
security_review.md if sensitive data is involved
testing_verification.md
```

Billing or credit feature:

```text
backend_service_patterns.md
api_design.md
database_migrations.md
security_review.md
testing_verification.md
```

Deployment task:

```text
deployment_ops.md
testing_verification.md
security_review.md if production secrets or sensitive config are involved
```

## Token Optimization

Do not read every skill file.

Read only:

- this index
- the specific skill file needed
- any second skill file only if the task clearly crosses areas

## Stop Conditions

Stop and ask Karim if:

- a new skill file seems needed
- the correct skill is unclear
- the task belongs to a future skill section not yet created
- ECC content conflicts with Karim AI Lab rules
- the skill would duplicate existing rule files
- a skill is becoming too broad or too long
