# Database Rules

Purpose:
Guide agents when working with database schemas, migrations, RLS, indexes, relationships, ownership fields, vector storage, audit logs, and multi-tenant data separation.

When to read:
- Read this file before creating, changing, querying, migrating, or deleting database structures.
- Always read before touching Supabase tables, RLS policies, migrations, ownership fields, customer data, order data, AI memory, vector tables, billing records, credit logs, or multi-tenant records.

## Database Principles

1. Do not invent schema.
   Never invent tables, columns, relationships, policies, indexes, enums, or migration paths without checking existing project files or asking Karim.

2. Confirm the ownership model.
   Protected data must have a clear owner field such as `user_id`, `merchant_id`, `brand_id`, `shop_id`, `organization_id`, or a project-approved equivalent.

3. Assume multi-tenant risk.
   Any SaaS, Shopify, WooCommerce, CRM, AI customer service, automation, or dashboard system may contain data from multiple users, merchants, brands, stores, or clients.

4. RLS is not optional for protected data.
   When Supabase is used, assume protected tables require RLS unless the project explicitly says otherwise.

5. Migrations require approval.
   Creating, altering, dropping, or migrating tables is approval-gated.

6. Service-role access stays server-side.
   Never use service-role database access in frontend code.

7. Database changes affect more than the database.
   A schema change may break backend services, API routes, frontend screens, background jobs, n8n workflows, Shopify sync, AI memory, analytics, or reporting.

8. Simple schema beats clever schema.
   Prefer clear tables, clear fields, explicit relationships, and predictable access rules.

## Mandatory Read Order Before Database Work

Before database work, read:

1. `karim_ai/START_HERE.md`
2. `karim_ai/INDEX.md`
3. `karim_ai/core/executive_laws.md`
4. `karim_ai/core/approval_gates.md`
5. `karim_ai/core/working_protocol.md`
6. `karim_ai/rules/security.md`
7. `karim_ai/rules/environment.md`
8. `karim_ai/rules/database.md`
9. Any project memory files if they exist
10. Existing migrations, schema files, seed files, and database client code

Do not edit schema from memory only.

## Approval Gate

Ask for explicit approval before:

- Creating a new table
- Dropping a table
- Renaming a table
- Adding a required column
- Dropping a column
- Renaming a column
- Changing a column type
- Changing primary keys
- Changing foreign keys
- Changing indexes on production-critical tables
- Creating or changing RLS policies
- Disabling RLS
- Changing tenant isolation logic
- Changing migration history
- Running destructive SQL
- Changing billing, payment, credit, user, merchant, order, or customer tables
- Creating background jobs that write database rows

The agent may propose database changes, but must not apply them until approved.

## No Schema Invention Rule

Never assume a table or field exists.

Bad:

```text
I will use `users.shop_id` because the app probably has shops.

Good:

I found no confirmed shop ownership field. I propose using `merchant_id` as the tenant boundary, but I need approval before writing the migration.

If schema is unclear, do one of the following:

Inspect the existing schema
Search existing migrations
Search existing backend queries
Ask Karim for confirmation
Propose a schema plan and wait for approval
Mark the field as undecided in the plan, not in production code
Schema Design Rules

When proposing schema, include:

Table purpose
Columns
Types
Primary key
Foreign keys
Ownership field
Timestamps
Required indexes
RLS impact
Migration plan
Rollback risk
Affected backend/API/frontend files

Default conventions:

Primary key: id
IDs: UUID unless project convention says otherwise
Timestamps: created_at, updated_at
Optional soft delete: deleted_at
Columns: snake_case
Tables: consistent project convention
Ownership fields: explicit and query-filtered

Most application tables should include:

id
created_at
updated_at

Protected multi-tenant tables should usually include one or more of:

user_id
merchant_id
brand_id
shop_id
organization_id
shop_domain

Do not add ownership fields blindly. Confirm the real tenant model first.

Table Naming Rules

Use clear snake_case table names.

Preferred patterns:

core_users
core_merchants
core_brands
shopify_products
shopify_orders
woo_products
ai_conversations
ai_messages
knowledge_base
policy_responses
automation_jobs
credit_ledger
billing_events
webhook_events

Rules:

Use core_ only for foundational identity/account tables.
Use feature prefixes when helpful: shopify_, ai_, billing_, automation_.
Avoid vague names like data, items, records, stuff, temp.
Avoid duplicate tables with almost the same meaning.
Do not rename existing tables unless there is a strong reason and approval.
Column Naming Rules

Use snake_case for every column.

Preferred examples:

id
user_id
merchant_id
brand_id
shop_domain
external_id
status
created_at
updated_at
deleted_at
metadata

Boolean columns should read clearly:

is_active
is_paid
has_attachment
can_retry

Avoid unclear abbreviations:

usr
mid
sts
cfg
tmp
Ownership and Tenant Separation

Every protected table must have a clear access boundary.

Before writing queries or policies, identify:

Who owns this row?
Who can read it?
Who can insert it?
Who can update it?
Who can delete it?
Can support/admin access it?
Can service-role jobs access it?
Is access based on user, merchant, brand, shop, or organization?

Never query private data without tenant filtering.

Bad:

select * from orders;

Good:

select * from orders where merchant_id = :merchant_id;

Frontend must never receive another tenant’s data and then filter it client-side.

Query Rules

For protected data:

Always filter by the correct owner key.
Avoid broad select * unless justified.
Select only the columns needed.
Limit results when lists can grow.
Order results intentionally.
Do not expose internal/admin-only columns to frontend.
Do not trust frontend-provided user_id, merchant_id, or shop_id without backend verification.
Do not perform broad update/delete operations without explicit safeguards.

For list queries, prefer:

Pagination
Limits
Filters
Stable ordering
Index-supported access patterns
Supabase RLS Rules

When Supabase is used, assume RLS must be enabled on all protected tables.

Protected tables include:

Users
Merchants
Brands
Shops
Orders
Customers
Conversations
Messages
Files
AI outputs
Knowledge base records
Credits
Billing events
Private settings
Integration metadata

When writing or reviewing RLS:

Define who can select, insert, update, and delete.
Match policies to the real ownership model.
Test policies before treating them as safe.
Do not weaken policies for convenience.
Do not use service-role as a workaround for bad policy design.
Do not disable RLS unless the table is explicitly public or server-only.

A table without RLS must be justified.

Migration Safety

Schema changes should be made through migrations whenever the project supports migrations.

Before migration:

Explain what will change.
Identify data loss risk.
Confirm environment: local, staging, or production.
Confirm approval.
Provide rollback or recovery notes when possible.
Identify affected code paths.

Every migration should:

Be small and focused.
Have a clear name.
Avoid unrelated changes.
Include required indexes and policies.
Avoid destructive operations unless approved.
Be tested locally or in staging when possible.

Do not edit old migrations unless:

The migration has not been applied anywhere; and
Karim confirms it is safe; or
The repo has a documented squash/reset workflow.

For existing projects, prefer creating a new migration.

Destructive Change Rules

Destructive changes require explicit approval and a rollback plan.

Destructive changes include:

DROP TABLE
DROP COLUMN
TRUNCATE
Mass DELETE
Mass UPDATE
Changing ID strategy
Changing ownership columns
Weakening RLS policies
Deleting migration history
Removing indexes from production-critical queries

Before destructive SQL, provide:

What will change
What data may be lost
Why it is necessary
Backup/export recommendation
Rollback plan
Exact SQL or migration diff
Relationship Rules

Use foreign keys when relationships are stable and important.

Examples:

orders.merchant_id -> merchants.id
messages.conversation_id -> conversations.id
products.brand_id -> brands.id
credit_ledger.user_id -> users.id

Rules:

Define ownership clearly.
Avoid orphan records unless intentionally allowed.
Be careful with cascade delete.
Prefer soft delete for important business records.
Document many-to-many join tables clearly.

Cascade delete is dangerous. Use it only when child records are meaningless without the parent.

Status and Enum Rules

Use consistent status values.

Preferred common statuses:

pending
processing
completed
failed
cancelled
active
inactive
draft
published
archived

Rules:

Do not create many similar statuses.
Avoid mixing past tense and present tense randomly.
Document status transitions.
Use database enums only when values are stable.
Use text with validation when the product is still changing fast.
JSON and Metadata Rules

metadata fields are allowed, but must not replace real schema.

Use JSON for:

Provider-specific payloads
Temporary external API metadata
Flexible logs
Low-query auxiliary information

Do not use JSON for:

Primary ownership
Status
Prices
Credits
Searchable product fields
Important relationships
Values used often in filters or joins

If a JSON field becomes business-critical, promote it to a real column.

Indexing Rules

Indexes must support real access patterns.

Common indexes:

user_id
merchant_id
brand_id
shop_domain
status
created_at
updated_at
conversation_id
order_id
product_id

Composite indexes may be needed for common filters:

(merchant_id, created_at)
(merchant_id, status)
(user_id, created_at)
(conversation_id, created_at)

Rules:

Do not over-index blindly.
Add indexes for frequent filters, joins, and ordering.
Add unique indexes for stable external IDs when needed.
Consider vector indexes for embedding search.
Verify query patterns before adding many indexes.
External ID Rules

When syncing with external platforms, store their IDs clearly.

Examples:

shopify_product_id
shopify_variant_id
shopify_order_id
woocommerce_product_id
stripe_customer_id
stripe_subscription_id
sendpulse_contact_id
meta_page_id

Rules:

Do not use external IDs as primary keys unless strongly justified.
Keep internal UUID primary keys.
Add unique constraints where needed.
Store platform/source if multiple providers can create similar records.
Vector Database Rules

For AI/RAG projects, vector storage must be treated as database architecture, not helper logic.

Before creating embeddings, confirm:

Embedding provider
Embedding model
Vector dimension
Source table
Chunking strategy
Metadata fields
Refresh/update strategy
Deletion strategy
Access policy

Required metadata for knowledge chunks should usually include:

id
source_type
source_id
title
content
embedding
metadata
created_at
updated_at

Rules:

Vector dimension must match the embedding model.
Do not mix embeddings from different models in one column unless designed for it.
Store enough metadata to trace the answer source.
Never expose private chunks across tenants.
Use tenant filters before vector similarity when required.
Log model/provider changes.
Audit and Ledger Rules

Use append-only logs for sensitive business events.

Append-only tables are recommended for:

Credit usage
Payments
Billing events
AI cost logs
Order sync events
Webhook events
Automation job history
Security-sensitive changes

Rules:

Never silently overwrite ledger history.
Use positive/negative amounts clearly.
Include reason/source.
Include actor or system identity.
Include timestamps.

Credit logic should follow this order:

Validate user/tenant.
Validate available credits.
Execute the paid action.
Deduct credits only after success, unless reservation logic exists.
Write ledger entry.
Return safe response.
Seed Data Rules

Seed data must never contain real secrets or private customer data.

Allowed seed data:

Fake users
Fake shops
Fake products
Fake orders
Public demo content
Generic policies

Forbidden seed data:

Real API keys
Real customer phone numbers
Real customer addresses
Real payment data
Real access tokens
Real private conversations
Environment and Secret Boundary

Database connection strings, service-role keys, and admin credentials must live only in secure server environments.

Never place these in:

Frontend code
Public env variables
Screenshots
Markdown examples with real values
Committed .env files
Logs

Use placeholder examples only:

SUPABASE_SERVICE_ROLE_KEY=your_service_role_key_here
DATABASE_URL=your_database_url_here
Documentation Requirements

When adding or changing schema, update the relevant documentation:

Project memory
Schema docs
API route docs
Backend docs
Affected feature docs
Migration notes

The update should explain:

What changed
Why it changed
Which files were affected
Whether data migration is needed
What remains to test
Stop Conditions

Stop and ask Karim if:

Table/field names are unclear.
Ownership model is unclear.
RLS policy is needed or changing.
Existing data may be affected.
A migration touches production.
The query could expose cross-tenant data.
A paid, credit, billing, or customer table is affected.
External sync behavior is unclear.
Vector dimension or embedding model is unknown.
Database Work Checklist

Before editing:

 Read project memory if it exists.
 Read existing schema/migrations.
 Identify tenant boundary.
 Identify affected frontend/backend/API files.
 Confirm whether approval is required.

Before committing:

 No secrets included.
 Naming follows snake_case.
 Required timestamps exist.
 Ownership columns exist where needed.
 RLS/policies are considered.
 Indexes support expected queries.
 Migration is focused.
 Destructive changes were approved.
 Documentation/memory updated if required.

Before production:

 Test migrations locally/staging.
 Test RLS with allowed user.
 Test RLS with blocked user.
 Test service-role workflow only server-side.
 Backup/export if destructive change exists.
 Confirm rollback plan.
Forbidden Database Actions

Agents must not:

Expose service-role keys.
Put database admin keys in frontend.
Disable RLS without approval.
Create schema from assumptions.
Run destructive SQL without approval.
Use real secrets in examples.
Store credentials in normal tables without an encryption strategy.
Create unfiltered cross-tenant queries.
Delete migration history casually.
Use JSON blobs to hide important schema decisions.
Change credit/payment/billing logic without approval.
Default Response Pattern for Database Requests

When Karim asks for database work, respond in this order:

Restate the database goal.
Identify existing schema/files inspected.
Identify tenant boundary.
List proposed schema/query/policy changes.
Flag approval-required items.
Wait for approval when needed.
Apply only approved changes.
Summarize changed files.
Update project memory if it exists.

For small non-destructive query fixes, the agent may proceed if the requested change is clear and inside an already-approved task.
