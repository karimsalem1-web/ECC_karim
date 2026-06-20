# Deployment Operations

Purpose:
Help agents prepare deployment and production operations safely, with practical checks for build readiness, environment configuration, secrets, migrations, health checks, rollback, and post-deploy verification.

Deployment skills do not override deployment/environment/security/database rules.
Before using this skill, read `rules/environment.md`.
For production or sensitive deploys, also read `rules/security.md`.
For database deploys, also read `rules/database.md`.
For backend/API deploys, also read `rules/backend.md` and `rules/api_routes.md`.

## When to Activate

Use this skill for:

- deployment readiness review
- CI/CD setup or review
- staging or production release planning
- Vercel, Shopify, Supabase, n8n, Google Cloud, container, or generic web app deploys
- environment variable and secret boundary checks
- build artifact checks
- database migration deployment risk
- health checks and smoke tests
- rollback or forward-fix planning
- Docker review when the project actually uses Docker

Deployment skill does not mean deploy automatically.
Production-impacting deploys require Karim approval.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/environment.md`
5. `rules/security.md` for production, secrets, auth, payments, credits, customer data, webhooks, uploads, or sensitive config
6. `rules/database.md` for database migrations or production data impact
7. `rules/backend.md` and `rules/api_routes.md` for backend/API deploys
8. `memory/INDEX.md` and relevant memory files if deployment behavior matters
9. Existing CI/CD, hosting, build, Docker, env, and deployment configuration

## Execution Pattern

Use this deployment flow:

```text
identify deployment target
-> inspect build and runtime requirements
-> verify env variables and secrets boundaries
-> verify database/migration impact
-> run build and relevant checks
-> confirm health/check endpoints or smoke test path
-> plan rollback or forward-fix
-> deploy only with approval when production-impacting
-> verify after deployment
-> update memory if deployment behavior changed
```

## Practical Checklist

Deployment target:

- Identify target platform: local, preview, staging, production, Vercel, Shopify, Supabase, n8n, Google Cloud, Docker/container, or other host.
- Identify build command, runtime command, runtime version, package manager, and artifact output.
- Identify whether the deploy affects frontend, backend, API routes, database, jobs, webhooks, or integrations.

Environment and secrets:

- Secrets must stay in hosting/provider secret storage, not source code.
- Confirm public and private env vars are separated.
- Confirm required env vars exist for the target environment.
- Confirm production secrets are not copied into local files.
- Confirm config differences between local, staging, and production are intentional.

Database and migrations:

- Identify pending migrations.
- Confirm destructive migrations or production data changes have approval gate.
- Prefer additive migrations before destructive changes.
- Confirm deploy order when app code and schema changes depend on each other.
- Confirm rollback or forward-fix path if migration fails.

Build and checks:

- Run the project build when deploy artifacts are affected.
- Run relevant type, lint, and tests before deployment readiness.
- If using CI/CD, ensure build/type/lint/test steps are clear.
- Confirm lockfiles and package manager usage are consistent.
- Confirm generated artifacts are intentional.

Health checks and smoke tests:

- Health checks and smoke tests matter more than complex deployment theory.
- Identify the simplest endpoint or user flow that proves the deploy is alive.
- For backend/API deploys, check a health endpoint or safe read endpoint.
- For frontend deploys, check the critical page loads and key assets render.
- For Shopify/app integrations, check app context, install/shop context, webhook/app proxy paths when relevant.
- For n8n or automation deployments, check the trigger path and one safe dry-run if available.

Logging and monitoring:

- Confirm logs are available after deploy.
- Confirm errors are safe and do not leak secrets.
- Confirm basic metrics, alerts, or provider dashboards are known for production-impacting deploys.
- Confirm there is a way to detect elevated errors after release.

Rollback and forward-fix:

- Identify previous deploy or artifact.
- Confirm whether rollback is safe with the current database state.
- Prefer forward-fix if rollback would break schema compatibility or lose data.
- Keep feature flags or kill switches in mind for risky features when the project supports them.

Docker guidance:

- Docker should only be used when the project actually uses Docker.
- Pin base image versions instead of using `latest`.
- Use multi-stage builds for production images.
- Run containers as non-root when practical.
- Do not bake secrets into images.
- Use `.dockerignore` to keep images small and avoid leaking files.
- Add health checks for long-running services.

Production approval:

- Deploy only with approval when production-impacting.
- Get approval for destructive migrations, production data changes, billing changes, credit behavior, auth changes, tenant isolation changes, webhook behavior, or secret/config changes.

## Stop Conditions

Stop and ask Karim if:

- the deployment target is unclear
- production impact is unclear
- secrets or env vars are missing or ambiguous
- destructive migrations or production data changes are involved
- rollback or forward-fix plan is unclear
- build, type, lint, tests, health checks, or smoke tests fail
- deployment requires live production access not explicitly approved
- CI/CD changes could affect production release behavior
- Docker is being introduced without a real project need

## Output Expectations

When reporting deployment work:

- Name the deployment target and affected app areas.
- State build/check results.
- State env and secret boundary findings without exposing values.
- State migration or database impact.
- State health check or smoke test path.
- State rollback or forward-fix plan.
- State whether production approval is required or was obtained.
- If deployment cannot be verified, say so clearly.
- Mention memory updates if deployment behavior changed.
