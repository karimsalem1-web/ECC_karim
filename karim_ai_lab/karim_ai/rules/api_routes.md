# API Routes Rules

Purpose:
Guide agents when designing, editing, reviewing, or documenting API routes, server actions, endpoint contracts, webhooks, callbacks, request validation, response shapes, authentication, authorization, rate limits, and safe errors.

When to read:
- Read this file before creating, editing, documenting, or reviewing API routes.
- Always read before touching server actions, route handlers, endpoints, webhooks, callbacks, Shopify app proxy routes, n8n callback routes, AI provider endpoints, payment routes, file upload routes, or protected API contracts.

## API Route Principles

1. One route should have one clear responsibility.
   Do not create routes that mix unrelated actions.

2. Validate every request.
   Validate method, headers, auth, params, query, body, content type, and file metadata when relevant.

3. Authenticate and authorize protected actions.
   Authentication proves who is calling. Authorization proves they are allowed to perform the action on that resource.

4. Use consistent response shapes.
   APIs should return predictable success and error responses.

5. Return safe errors.
   Do not expose stack traces, secrets, SQL details, provider internals, or sensitive business logic.

6. Keep route handlers thin.
   Routes should validate, authorize, call backend services, and return responses. Business logic should not become trapped inside route files when the project structure supports services/modules.

7. Avoid route sprawl.
   Do not create many unclear endpoints when a cleaner resource structure exists.

8. API changes are contracts.
   A route change may break frontend screens, mobile clients, Shopify extensions, n8n workflows, external services, or AI agents.

## Mandatory Read Order Before API Route Work

Before API route work, read:

1. `karim_ai/START_HERE.md`
2. `karim_ai/INDEX.md`
3. `karim_ai/core/executive_laws.md`
4. `karim_ai/core/approval_gates.md`
5. `karim_ai/core/working_protocol.md`
6. `karim_ai/rules/security.md`
7. `karim_ai/rules/environment.md`
8. `karim_ai/rules/backend.md`
9. `karim_ai/rules/database.md` when the route reads/writes data
10. Project memory files if they exist
11. Existing route files and clients that call them

Do not redesign routes from memory only.

## Approval Gate

Ask for explicit approval before:

- Creating a new public route
- Changing an existing request/response contract
- Deleting a route
- Renaming a route
- Changing auth requirements
- Changing authorization or tenant ownership checks
- Changing payment, billing, credit, customer, order, or webhook routes
- Adding a route that calls paid AI providers
- Adding a route that writes production data
- Adding a route that exposes files or signed URLs
- Adding a route consumed by external systems
- Moving sensitive route logic to frontend

Agents may propose API changes, but must not apply high-risk route changes until approved.

## Route Responsibility Rules

Each route should clearly answer:

- What is the route for?
- Who can call it?
- What method is allowed?
- What input is required?
- What output is returned?
- What data does it read?
- What data does it write?
- Does it call external services?
- Does it trigger paid actions?
- Does it trigger background jobs?
- Does it expose private data?

If the route cannot be explained simply, split it or move logic into services.

## Route Naming Rules

Use clear, predictable route names.

Good examples:

```text
/api/orders/sync
/api/products/search
/api/ai/generate-description
/api/billing/create-checkout-session
/api/webhooks/shopify
/api/webhooks/stripe
/api/upload/signed-url
/api/jobs/:job_id/status

Avoid vague routes:

/api/do-action
/api/process
/api/data
/api/handler
/api/test-final
/api/new-api

Rules:

Use resource-based names when possible.
Use action names only when the action is truly not CRUD.
Keep webhook routes clearly grouped.
Keep internal/admin routes clearly separated.
Do not expose experimental/test routes in production unless protected.
Method Rules

Use HTTP methods intentionally:

GET     read data
POST    create, trigger, submit, process
PUT     replace
PATCH   partial update
DELETE  delete or archive

Rules:

Do not use GET for actions with side effects.
Do not use GET for paid AI calls, credit deduction, message sending, or order mutation.
Reject unsupported methods.
Make destructive operations explicit and protected.
Request Validation Rules

Validate:

HTTP method
Auth/session
Authorization
Route params
Query params
Request body
Headers
Content type
File metadata
Payload size
Required fields
Allowed enum/status values
External callback payloads

Reject invalid data before calling services or database logic.

Do not pass raw request data directly into:

Database queries
Provider API calls
AI prompts
n8n workflows
File paths
Webhook forwarding
Payment actions
Authentication and Authorization

For protected routes:

Verify identity.
Identify tenant boundary.
Verify ownership or permission.
Perform action.

Authentication alone is not enough.

Rules:

Derive user/merchant/shop identity from verified session where possible.
Do not trust frontend-provided owner IDs without verification.
Block cross-tenant access.
Admin/support routes must be explicit and protected.
Public routes must be intentionally public.

For Shopify routes, verify:

Shop domain
App installation/session
Signed requests or valid session
Shop ownership of requested resources

For webhooks, verify:

Provider signature when available
Event source
Event type
Replay/idempotency protection when possible
Tenant Boundary Rules

Protected API routes must know the tenant boundary.

Possible tenant boundaries:

user_id
merchant_id
brand_id
shop_id
shop_domain
organization_id

Before returning data, confirm:

Caller owns the resource.
Caller belongs to the tenant.
Route does not leak another tenant’s data.
Database queries are filtered correctly.
Service-role access is not bypassing ownership checks.

Never rely on frontend filtering for private data.

Suggested Response Shape

Use the project’s existing response format if one exists.

If starting from scratch, prefer:

{
  "success": true,
  "message": "Human-readable summary",
  "data": {},
  "error": null
}

Error response:

{
  "success": false,
  "message": "Safe human-readable error",
  "data": null,
  "error": "SAFE_ERROR_CODE"
}

Rules:

Keep response shape consistent.
Do not return raw provider errors.
Do not return stack traces.
Do not return secrets.
Return only required fields.
Use safe error codes when helpful.
Status Code Rules

Use status codes intentionally:

200 OK — successful read/update
201 Created — resource created
202 Accepted — job accepted / async processing started
204 No Content — successful action with no body
400 Bad Request — invalid input
401 Unauthorized — missing/invalid authentication
403 Forbidden — authenticated but not allowed
404 Not Found — resource missing or hidden
409 Conflict — duplicate or state conflict
422 Unprocessable Entity — valid shape but invalid business rule
429 Too Many Requests — rate limited
500 Internal Server Error — unexpected server failure

Do not return 200 for failed protected actions.

Safe Error Rules

Never expose:

Stack traces
SQL errors
Secrets
API keys
Provider raw failures
Internal file paths
Private customer data
Payment internals
RLS policy details that help attackers

Log useful server-side context, but return safe client-facing messages.

Route Handler Structure

Preferred route flow:

1. Confirm method
2. Parse request
3. Validate input
4. Authenticate
5. Authorize / confirm tenant ownership
6. Call service/module
7. Return safe response
8. Log safe result

For tiny projects, this can live in one file.
For growing projects, move business logic to services/modules.

Avoid route files with large mixed logic.

Webhook Route Rules

Webhook routes require extra care.

For webhooks:

Verify provider signature when supported.
Validate payload shape.
Confirm event type.
Use idempotency key when duplicates are possible.
Store event id when needed.
Return quickly if provider expects fast acknowledgement.
Move heavy work to background jobs when possible.
Do not trust webhook payloads blindly.
Do not expose webhook secrets.
Log safe provider/event metadata.

Common webhook examples:

Shopify app events
Stripe billing events
Meta/Instagram messages
SendPulse inbound messages
n8n callbacks
AI job completion callbacks

Duplicate webhook events must not:

deduct credits twice
create duplicate orders
send duplicate messages
trigger duplicate paid AI calls
overwrite newer data incorrectly
n8n Callback Route Rules

If a route receives data from n8n:

Validate source if possible.
Validate payload shape.
Validate job id or correlation id.
Confirm the related user/tenant/job exists.
Do not blindly write n8n output to database.
Map workflow failures to safe API/job statuses.
Avoid exposing n8n webhook URLs or API keys.

Preferred pattern:

frontend
↓
API route
↓
validation + authorization
↓
n8n workflow
↓
callback/status route
↓
database/job status update
↓
frontend status/result
AI Provider Route Rules

Routes that call AI providers must:

Keep provider keys server-side.
Validate user input.
Sanitize prompt inputs where needed.
Avoid sending unnecessary private data.
Check credits/subscription before paid calls.
Track cost when relevant.
Return safe outputs.
Handle provider failures.
Avoid repeated paid calls for the same request unless intended.

For image/video/audio/file workflows:

Validate file type.
Validate file size.
Use signed URLs when appropriate.
Return job status for long operations.
Avoid large uploads through slow route handlers.
Do not expose storage credentials.
File Upload Route Rules

Upload routes must:

Validate auth.
Validate file type.
Validate file size.
Validate purpose.
Generate signed URLs server-side when needed.
Use tenant-aware storage paths.
Avoid public access for private files.
Sanitize filenames.
Return only safe file references.
Define cleanup strategy for temporary files.

Preferred pattern:

client asks API for signed upload URL
↓
API validates user and file metadata
↓
API returns signed URL
↓
client uploads directly to storage
↓
backend/job processes stored file
Payment and Credit Route Rules

Payment, billing, and credit routes are approval-gated.

Rules:

Authenticate user/merchant.
Authorize resource.
Validate input.
Check subscription/payment/credits server-side.
Never trust frontend payment status.
Never expose payment secrets.
Deduct credits only after successful action unless reservation logic exists.
Write ledger/audit records.
Make retries idempotent.
Handle provider webhooks carefully.

Forbidden:

Deduct credits in frontend.
Trust client-side price/credit values.
Trigger paid providers without backend validation.
Return payment provider internals to frontend.
Rate Limit and Abuse Rules

Consider rate limits for routes that:

Call paid AI providers
Send messages
Upload files
Trigger jobs
Search databases
Use public forms
Receive unauthenticated requests
Process webhooks

Rules:

Add rate limits when abuse is likely.
Add payload size limits.
Avoid unlimited expensive operations.
Return 429 when rate limited.
Log safe abuse signals.
Do not reveal internal thresholds unnecessarily.
Async Job Route Rules

For long-running work, prefer job/status routes.

Example:

POST /api/ai/try-on/start
GET /api/jobs/:job_id/status

Rules:

Return 202 Accepted when work starts asynchronously.
Return job id/status.
Store job ownership.
Protect status route with ownership checks.
Make retries safe.
Avoid blocking route handlers for long AI/video/file jobs.
Versioning and Contract Changes

API routes are contracts.

Before changing a route contract:

Identify all callers.
Check frontend usage.
Check n8n workflows.
Check Shopify extensions/app proxy usage.
Check external provider callbacks.
Check tests/docs.
Prefer backward compatibility when possible.

Breaking changes require approval.

Stop Conditions

Stop and ask Karim if:

Route purpose is unclear.
Auth requirement is unclear.
Tenant boundary is unclear.
Request/response contract is unclear.
Route touches payment, billing, credits, customers, orders, or production data.
Route calls paid AI providers.
Route receives webhooks and signature verification is unknown.
Route exposes files or signed URLs.
Route may be called by external services.
Existing frontend/n8n/webhook callers may break.
API Route Work Checklist

Before editing:

 Read relevant Karim AI Lab files.
 Inspect existing route structure.
 Identify caller/client.
 Identify auth requirement.
 Identify tenant boundary.
 Identify request/response contract.
 Identify database tables affected.
 Identify external providers affected.
 Identify approval-gated changes.

Before committing:

 Only intended route files changed.
 Request validation exists.
 Auth check exists when needed.
 Authorization/ownership check exists when needed.
 Response shape is consistent.
 Errors are safe.
 No secrets exposed.
 No stack traces returned.
 Paid actions are protected.
 Webhooks are verified/idempotent when needed.
 Documentation/memory updated if required.

Before production:

 Env vars exist in production.
 External callbacks/webhooks tested.
 Route called successfully from real client/workflow.
 Failure responses tested.
 Logs checked for secret leaks.
 Rate limit/abuse risk reviewed.
 Rollback plan exists for breaking changes.
Forbidden API Route Actions

Agents must not:

Create vague routes.
Use GET for side effects.
Skip validation.
Skip authorization.
Trust frontend owner IDs.
Return private data across tenants.
Return stack traces.
Return raw provider errors.
Expose secrets or signed credentials.
Trigger paid actions without server-side checks.
Deduct credits without backend ledger logic.
Process webhooks without validation.
Create duplicate side effects on retries.
Change route contracts without checking callers.
Move protected route logic to frontend.
Default Response Pattern for API Route Requests

When Karim asks for API route work, respond in this order:

Restate the route goal.
Identify existing route files inspected.
Identify caller/client.
Identify auth and tenant boundary.
Define request/response contract.
List proposed route/service/database changes.
Flag approval-required items.
Apply only approved changes.
Summarize changed files.
Update project memory if it exists.

For small non-sensitive route fixes, the agent may proceed if the requested change is clear and inside an already-approved task.
