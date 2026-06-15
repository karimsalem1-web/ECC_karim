# Naming Rules

Purpose:
Guide agents when naming files, folders, variables, functions, database tables, columns, routes, environment variables, workflows, and documentation.

When to read:
- Read this file before creating new files, folders, tables, columns, APIs, env vars, workflows, or major functions.
- Read it when cleaning messy names or standardizing a project.

## General Naming Principles

1. Names must explain purpose.
   A good name should reduce the need for comments.

2. Prefer consistency over personal style.
   Follow the naming style already used in the project unless it is unsafe or unclear.

3. Avoid vague names.
   Avoid names like `data`, `info`, `stuff`, `thing`, `temp`, `final`, `new`, `test2`, or `helper` when a clearer name exists.

4. Avoid misleading names.
   Do not name something `user` if it actually means merchant, customer, admin, session, or account.

5. Do not rename widely used items casually.
   Renaming tables, routes, folders, or shared functions may create hidden breakage.

## File and Folder Naming

Use the style already present in the project. If starting from scratch:

- Markdown files: snake_case.md or clear lowercase names.
- TypeScript/JavaScript files: follow project convention.
- React components: PascalCase when component-based.
- Hooks: start with `use`.
- API route folders: match framework convention.
- Config files: follow tool convention exactly.

Examples:
- Good: `order_service.ts`, `useCustomerOrders.ts`, `product_image_upload.md`
- Bad: `newFile.ts`, `logic2.ts`, `final_version.md`

## Function and Variable Naming

- Functions should start with a verb when they do something.
- Boolean values should read like true/false statements.
- IDs should specify what they belong to.

Examples:
- `fetchCustomerOrders`
- `validateWebhookSignature`
- `isAuthenticated`
- `hasActiveSubscription`
- `merchantId`
- `shopifyOrderId`

Avoid:
- `processData`
- `handleThing`
- `check`
- `id` when multiple IDs exist in scope

## Database Naming

Default database naming:
- Tables: snake_case plural or domain_consistent names.
- Columns: snake_case.
- Primary key: `id`.
- Timestamps: `created_at`, `updated_at`.
- Ownership fields should be explicit: `user_id`, `merchant_id`, `brand_id`, `shop_id`, or project-approved equivalent.

Do not invent table or column names without confirmation when schema is not defined.

## API Route Naming

Routes should describe resources and actions clearly.

Preferred:
- `/api/orders`
- `/api/products/sync`
- `/api/webhooks/shopify`
- `/api/customers/{customer_id}`

Avoid:
- `/api/doStuff`
- `/api/test`
- `/api/new`
- `/api/handle`

## Environment Variable Naming

Use uppercase snake case.

Examples:
- `SUPABASE_SERVICE_ROLE_KEY`
- `SHOPIFY_API_SECRET`
- `N8N_WEBHOOK_URL`
- `OPENAI_API_KEY`
- `CLAUDE_API_KEY`

Rules:
- Public frontend env vars must use the framework-required public prefix only when safe.
- Secret env vars must never use public prefixes.
- Do not invent env var names without checking existing project conventions.

## Documentation Naming

Documentation files should describe their role clearly.

Examples:
- `PROJECT_MEMORY.md`
- `CURRENT_TASK.md`
- `DECISIONS.md`
- `ISSUES_AND_FIXES.md`
- `API_CONTRACTS.md`

## Naming Review Checklist

Before finalizing names, check:
- Is the name clear?
- Is it consistent with the project?
- Does it avoid ambiguity?
- Does it reveal secrets or sensitive data?
- Could it conflict with existing files, routes, tables, or variables?
- Would another agent understand it later?
