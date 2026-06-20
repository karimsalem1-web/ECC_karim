# Database Migrations

Purpose:
Help agents plan and execute database changes safely, especially for Supabase/Postgres projects with RLS, ownership fields, indexes, and production data.

Database migration skills do not override database rules.
Before using this skill, read `rules/database.md`.
For security-sensitive schema work, also read `rules/security.md`.
For backend/API impact, also read `rules/backend.md` and `rules/api_routes.md` when relevant.

## When to Activate

Use this skill for:

- creating tables
- altering schema
- adding or changing indexes
- writing migrations
- planning data backfills
- changing RLS or ownership fields
- adding vector or embedding storage
- changing database behavior used by backend or API routes
- preparing production-safe database changes

Every schema change must be treated as a migration.
Do not make manual production database edits.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/database.md`
5. `rules/security.md` if RLS, tenant isolation, auth, secrets, customer data, payments, credits, or private data are involved
6. `rules/backend.md` if backend services or queries are affected
7. `rules/api_routes.md` if route contracts are affected
8. `memory/INDEX.md`
9. `memory/by_area/database.md` if database memory exists
10. Existing migrations, schema files, database docs, and query code

## Execution Pattern

Use this flow:

```text
inspect existing schema
-> identify affected tables and ownership fields
-> identify backend/API/frontend impact
-> choose additive, destructive, or expand-contract strategy
-> separate schema migration from data migration
-> plan indexes and constraints
-> review RLS and tenant isolation impact
-> write reviewable migration
-> plan rollback or forward-fix path
-> verify against realistic data shape
-> update memory/by_area/database.md after completed database work
```

Never invent schema.
Inspect the existing schema first.

## Practical Checklist

Before writing a migration:

- Confirm the exact table, column, index, constraint, function, trigger, or policy affected.
- Confirm ownership fields and tenant boundaries.
- Confirm existing naming conventions.
- Confirm whether the change is additive, destructive, or behavioral.
- Confirm whether application code must deploy before or after the migration.
- Confirm whether the migration touches production data.
- Get explicit approval before destructive migrations.

Migration safety:

- Keep migrations reviewable.
- Separate schema migrations from data migrations.
- Prefer additive changes before destructive changes.
- Use expand-contract for production-safe renames, type changes, and removals.
- Avoid long locks on large tables.
- Add indexes intentionally and name them clearly.
- Avoid backfilling large data in one unbounded transaction.
- Never store real secrets in migrations.
- Never run destructive migrations without explicit approval.
- Document affected tables and ownership fields.

Supabase/Postgres orientation:

- Consider RLS policies with every tenant-owned table.
- Add ownership fields intentionally, such as user, shop, account, organization, or tenant IDs.
- Use `CREATE INDEX CONCURRENTLY` for large existing Postgres tables when the migration tool supports it.
- Avoid adding `NOT NULL` columns to populated tables without a safe default or backfill plan.
- Add foreign keys and uniqueness constraints only after checking existing data.
- Treat SQL functions, triggers, and policies as production behavior that needs review.

Expand-contract pattern:

```text
expand: add new nullable column/table/index/policy
deploy: update app to write old and new shapes when needed
migrate: backfill data safely
verify: compare old and new behavior
contract: remove old shape only after code no longer depends on it
```

Data migrations:

- Keep data migrations separate from schema changes.
- Batch large backfills.
- Make backfills restartable where possible.
- Log progress safely.
- Avoid raw customer data in logs.
- Prefer forward-fix migrations in production when rollback is risky.

Vector and embedding storage:

- Use vector columns only when the project actually needs similarity search.
- Document embedding dimensions and provider/model if it becomes stable project behavior.
- Index vector data intentionally for the expected query pattern.
- Avoid storing raw private prompts or customer data unless the product requires it and security rules allow it.

Rollback and forward-fix:

- Define how to recover if the migration fails.
- Use rollback in development when safe.
- Prefer forward-fix in production when rollback may lose data or break deployed code.
- Never edit a migration that already ran in a shared or production environment.

## Stop Conditions

Stop and ask Karim if:

- the existing schema cannot be confirmed
- ownership fields or tenant isolation are unclear
- the change is destructive
- production data may be affected
- RLS behavior is unclear
- a backfill may be large or slow
- the application deploy order is unclear
- rollback or forward-fix strategy is unclear
- a migration would store secrets, raw customer data, or unnecessary private data
- vector storage or embedding behavior is unclear

## Output Expectations

When reporting database work:

- Name affected tables, columns, indexes, constraints, policies, functions, or triggers.
- State whether the change is additive, destructive, or expand-contract.
- Mention ownership and RLS implications.
- Mention backend/API impact.
- Mention rollback or forward-fix notes.
- Mention verification performed.
- Update `memory/by_area/database.md` after completed database work.
