# Backend Rules

Purpose:
Guide agents when working with server-side logic, services, private integrations, webhooks, background jobs, queues, AI/provider calls, billing operations, and protected business logic.

When to read:
- Read this file before creating or editing backend logic.
- Always read before touching API handlers, server actions, Supabase Edge Functions, private integrations, webhooks, queues, background jobs, payment logic, credit logic, AI provider calls, database service-role access, or automation bridges.

## Backend Principles

1. Backend owns sensitive work.
   Auth checks, authorization, service-role database access, paid AI calls, webhook verification, payments, credit logic, customer data handling, private integrations, and secret-based operations belong on the backend.

2. Validate at the boundary.
   Validate request body, query params, route params, headers, webhook payloads, file metadata, uploaded content metadata, and external API responses before using them.

3. Keep handlers thin.
   Route handlers should validate, authorize, call services, and return responses. Business logic should live in services/modules when the project structure supports it.

4. Use safe errors.
   Log useful server-side details, but return safe messages to clients.

5. Avoid hidden side effects.
   Any backend function that sends messages, charges money, deducts credits, writes records, triggers jobs, calls paid AI models, or changes customer/order data must be explicit and reviewable.

6. Make repeated events safe.
   Webhooks, queues, retries, and background jobs should be idempotent when duplicates are possible.

7. Server code is not a shortcut around bad design.
   Do not use service-role access, admin credentials, or backend bypasses to avoid proper authorization and RLS design.

8. Backend changes can affect production users.
   Treat backend logic as high-risk when it touches payments, stores, customers, AI costs, automation, messages, or orders.

## Mandatory Read Order Before Backend Work

Before backend work, read:

1. `karim_ai/START_HERE.md`
2. `karim_ai/INDEX.md`
3. `karim_ai/core/executive_laws.md`
4. `karim_ai/core/approval_gates.md`
5. `karim_ai/core/working_protocol.md`
6. `karim_ai/rules/security.md`
7. `karim_ai/rules/environment.md`
8. `karim_ai/rules/database.md`
9. `karim_ai/rules/api_routes.md` when the backend work exposes or consumes routes
10. Project memory files if they exist
11. Existing backend files related to the same feature

Do not rewrite backend logic from memory only.

## Approval Gate

Ask for explicit approval before:

- Adding a new backend service with production impact
- Changing authentication logic
- Changing authorization logic
- Changing payment or billing behavior
- Changing credit deduction or refund behavior
- Changing webhook processing logic
- Changing customer/order sync behavior
- Changing AI provider/model for paid calls
- Moving backend logic to frontend
- Adding service-role database access
- Adding background jobs that write data
- Adding retry logic that may duplicate side effects
- Changing production env var requirements
- Deleting backend files or routes

Agents may propose backend changes, but must not apply high-risk changes until approved.

## Backend Responsibility Rules

Backend must handle:

- Authentication verification
- Authorization checks
- Tenant ownership checks
- Service-role database operations
- Webhook signature verification
- Payment provider logic
- Credit validation and deduction
- Paid AI/model calls
- Private API calls
- External integration secrets
- Background jobs
- Queue processing
- File upload signing
- Sensitive logging
- Rate limit handling
- Data sanitization before returning to frontend

Frontend must not handle these responsibilities directly.

## Service Structure

Prefer clear separation:

```text
route/controller: request handling
validation: input checks
service: business logic
repository/data layer: database access
provider client: external API calls
logger: safe logs
types: shared input/output contracts

Do not force heavy architecture for tiny projects, but preserve separation when complexity grows.

Bad:

Route handler validates request, calls Shopify, deducts credits, writes database rows, sends WhatsApp messages, and formats UI response all in one long function.

Good:

Route handler validates and authorizes.
Service performs the business action.
Repository writes database rows.
Provider client calls external APIs.
Route returns a safe response.
Authentication and Authorization

Authentication answers:

Who is making the request?

Authorization answers:

Are they allowed to do this action on this resource?

Rules:

Authentication alone is not enough.
Always verify resource ownership.
Derive user/tenant identity from verified session when possible.
Do not trust frontend-provided user_id, merchant_id, shop_id, or brand_id without verification.
Backend must block cross-tenant access.
Admin/support access must be explicit and logged when sensitive.

For Shopify apps, confirm:

Shop domain
Installed shop/merchant mapping
Valid session or signed request
Ownership of product/order/customer data
Input Validation Rules

Validate:

HTTP method
Headers
Auth/session
Route params
Query params
Request body
Content type
File metadata
Payload size
Required fields
Data types
Allowed enum/status values
External provider responses

Reject invalid data before business logic runs.

Do not pass raw user input directly into:

database queries
provider API calls
AI prompts
shell commands
file paths
webhook forwarding
n8n payloads
Output Sanitization Rules

Before returning data to frontend:

Remove secrets.
Remove service/internal fields.
Remove provider raw payloads unless needed.
Remove stack traces.
Remove SQL details.
Remove private customer data not required by the UI.
Return only the data needed by the client.

Safe response examples:

{
  "success": true,
  "message": "Order synced successfully",
  "data": {
    "order_id": "example-id",
    "status": "synced"
  },
  "error": null
}
{
  "success": false,
  "message": "Order sync failed",
  "data": null,
  "error": "SYNC_FAILED"
}
Error Handling Rules

Backend code must:

Use try/catch around async operations.
Log server-side errors with enough context.
Return safe error messages to clients.
Avoid leaking stack traces.
Avoid leaking secrets or provider internals.
Use consistent error shapes when the project has a standard.
Map provider errors to safe application errors.

Do not swallow errors silently.

Bad:

catch error and return success false without logging anything.

Good:

log safe context server-side, return a safe user-facing error.
Logging Rules

Logs should help debugging without exposing private data.

Allowed:

request id
user id / merchant id when safe
route name
job id
provider name
status code
safe error code
execution timing
token/cost summary when relevant

Forbidden:

API keys
access tokens
refresh tokens
service-role keys
full customer addresses
full payment details
raw private conversations unless explicitly needed and protected
uploaded file contents
secrets from env vars
Environment Variable Rules

Backend may read private env vars.

Frontend must not read private env vars.

Private examples:

SUPABASE_SERVICE_ROLE_KEY
DATABASE_URL
CLAUDE_API_KEY
OPENAI_API_KEY
SHOPIFY_API_SECRET
STRIPE_SECRET_KEY
N8N_API_KEY
WEBHOOK_SECRET

Public examples must be intentionally public:

NEXT_PUBLIC_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY

Rules:

Never log env var values.
Never commit .env files.
Never create fake real-looking secrets.
Add new required env vars to the relevant env documentation if the project has it.
Stop if a needed secret is missing and cannot be safely inferred.
Database Access Rules

Backend database access must follow database.md.

Rules:

Use tenant-filtered queries.
Avoid broad selects.
Use service-role only server-side.
Do not bypass RLS unless the operation is explicitly trusted and authorized.
Validate ownership before writes.
Use transactions when multiple writes must succeed or fail together.
Use append-only ledgers for sensitive financial/credit events.

Service-role access requires extra caution:

explain why it is needed
keep it server-side
limit returned data
verify tenant ownership manually when bypassing RLS
External Integration Rules

For Shopify, WooCommerce, Supabase, n8n, AI APIs, payment providers, email, WhatsApp, Instagram, Meta, Google Cloud, or other services:

Keep credentials in secure env vars.
Validate outbound payloads.
Validate inbound responses.
Handle rate limits and failures.
Do not send unnecessary private data.
Add cost awareness for paid providers.
Use retries carefully.
Respect provider auth/signature requirements.
Store external IDs clearly.
Log safe provider status and error codes.

Do not call external services from frontend when the call requires secrets or private business logic.

Webhook Rules

For webhooks:

Verify signature when provider supports it.
Validate payload shape.
Validate source/provider.
Use idempotency keys when duplicate events are possible.
Store raw event only if safe and useful.
Do not trust webhook payloads blindly.
Process asynchronously when work is heavy.
Return quickly when provider expects fast acknowledgement.
Log event id, provider, type, and result.
Protect against replay attacks when possible.

Examples of webhook-sensitive systems:

Shopify app subscriptions
Shopify order/product events
Stripe billing events
Meta/Instagram messaging
SendPulse inbound events
n8n callback events
AI job completion callbacks
Background Jobs and Automation

For jobs:

Define trigger source.
Define input payload.
Define expected output.
Define retry behavior.
Define timeout expectations.
Define idempotency key if duplicates matter.
Define logging and failure handling.
Define whether the job is user-facing or internal.
Define whether the job costs money.
Define whether the job writes database rows.

Jobs should avoid hidden duplication.

If a job may be retried, make side effects safe.

Examples:

Do not send the same WhatsApp message twice.
Do not deduct credits twice.
Do not create duplicate orders.
Do not call paid AI multiple times for the same item unless intended.
n8n and Automation Bridge Rules

If n8n is used:

Frontend must not call n8n directly when secrets or protected data are involved.
Backend validates auth and data before calling n8n.
Backend sends clean JSON payloads.
n8n responses must be validated before use.
n8n errors must be mapped to safe backend errors.
Long-running n8n jobs should return a job id or status mechanism.
Do not hardcode n8n webhook URLs in frontend.
Do not expose n8n API keys to frontend.

Preferred flow:

Frontend
↓
Backend/API route/server action
↓
Validation + authorization
↓
n8n or external workflow
↓
Backend response/status update
↓
Frontend
AI Provider Rules

For AI calls:

Keep provider keys server-side.
Validate and sanitize user input before prompt construction.
Avoid sending unnecessary private data.
Include tenant/user context only when needed.
Track cost when relevant.
Use deterministic code instead of AI when the task does not need AI.
Cache or batch repeated requests when useful.
Store model/provider metadata for important outputs.
Handle provider failures safely.
Never expose raw provider errors to frontend.

For image, video, audio, or try-on AI workflows:

Validate file type and size.
Use signed upload URLs when needed.
Avoid routing large uploads through slow backend functions.
Store temporary files with cleanup strategy.
Do not expose private storage credentials.
Return job status or result URLs safely.
Credit, Billing, and Paid Action Rules

Paid or credit-based backend actions require strict order:

Authenticate user/merchant.
Authorize action.
Validate required input.
Check available credits/subscription/payment status.
Execute the action.
Deduct credits only after success unless reservation logic exists.
Write ledger/audit record.
Return safe response.

Never:

Deduct credits before validation.
Deduct credits twice for retry.
Hide paid provider failures.
Change billing logic without approval.
Expose payment secrets to frontend.
Trust frontend to decide whether a user paid.
File Upload and Storage Rules

Backend should handle secure upload preparation.

Rules:

Validate file type.
Validate file size.
Generate signed upload URLs server-side.
Store files under tenant-aware paths when needed.
Avoid public buckets for private files.
Do not expose storage service credentials.
Sanitize filenames.
Return only safe file references.
Add cleanup strategy for temporary files.

For AI processing, prefer:

client uploads directly to storage using signed URL
backend stores object reference
backend/job processes object
backend returns safe result reference
Performance Rules

Backend should avoid unnecessary slow work inside request/response paths.

Rules:

Move long operations to jobs when possible.
Set timeouts for external calls.
Avoid N+1 database queries.
Use pagination for growing lists.
Use indexes for common queries.
Avoid repeated paid AI calls.
Cache stable results when useful.
Return quickly for webhook acknowledgements.
Stop Conditions

Stop and ask Karim if:

Auth/ownership logic is unclear.
Tenant boundary is unclear.
A private integration key is needed.
A paid action may be triggered.
A webhook signature method is unknown.
A background job may duplicate side effects.
Backend logic is being moved to frontend.
The task may affect production data or customers.
Credit, billing, customer, order, or payment logic is affected.
Required env vars are missing or unclear.
External provider behavior is unknown.
Backend Work Checklist

Before editing:

 Read relevant Karim AI Lab files.
 Inspect existing backend structure.
 Identify auth method.
 Identify tenant boundary.
 Identify env vars required.
 Identify external integrations affected.
 Identify database tables affected.
 Identify approval-gated changes.

Before committing:

 No secrets in code.
 Input validation exists.
 Authorization exists.
 Errors are safe.
 Logs are safe.
 Service-role access is server-side only.
 External calls handle failure.
 Paid/credit actions are protected.
 Webhooks/jobs are idempotent when needed.
 Documentation/memory updated if required.

Before production:

 Env vars exist in production.
 Webhook secrets/signatures tested.
 Paid provider calls tested safely.
 Retry behavior checked.
 Logs checked for secret leaks.
 Production data impact reviewed.
 Rollback plan exists for risky changes.
Forbidden Backend Actions

Agents must not:

Move sensitive backend logic to frontend.
Expose service-role keys.
Log secrets.
Trust frontend ownership fields.
Skip authorization checks.
Use service-role access as a shortcut for bad RLS.
Trigger paid actions without validation.
Deduct credits without ledger records.
Process webhooks without validation.
Create duplicate side effects on retries.
Return stack traces or provider internals to frontend.
Hardcode private integration credentials.
Change billing/payment logic without approval.
Default Response Pattern for Backend Requests

When Karim asks for backend work, respond in this order:

Restate the backend goal.
Identify existing files inspected.
Identify auth and tenant boundary.
Identify affected database/API/frontend/integration parts.
List proposed backend changes.
Flag approval-required items.
Apply only approved changes.
Summarize changed files.
Update project memory if it exists.

For small non-sensitive backend fixes, the agent may proceed if the requested change is clear and inside an already-approved task.
