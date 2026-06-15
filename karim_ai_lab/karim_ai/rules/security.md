# Security Rules

Purpose:
Guide agents when working with secrets, authentication, authorization, sensitive data, validation, logging, and external services.

When to read:
- Read this file before touching auth, users, customers, orders, payments, credits, webhooks, API keys, admin actions, RLS, uploads, or external integrations.
- Always read this file before moving logic between frontend and backend.

## Security Principles

1. Never expose secrets.
   API keys, tokens, service-role keys, webhook secrets, private URLs, credentials, and signing secrets must never appear in frontend code, logs, commits, screenshots, or documentation.

2. Sensitive logic stays server-side.
   Authentication, authorization, service-role database access, paid AI calls, webhook verification, payment logic, credit deduction, and private integrations must run on the backend.

3. Never trust frontend input.
   Validate and sanitize every input received from users, forms, webhooks, APIs, files, images, and LLM-generated outputs.

4. Verify identity and ownership.
   Do not assume that authenticated means authorized. Protected records must be filtered by the correct owner field such as user_id, merchant_id, brand_id, shop_id, or the project-approved equivalent.

5. Return safe errors.
   Do not expose stack traces, keys, SQL details, internal URLs, provider secrets, or sensitive business logic to the frontend.

6. Use least privilege.
   Use the lowest permission level needed. Service-role or admin credentials should only be used in trusted server-side contexts.

## Authentication and Authorization

Before accessing protected data, check:
- Is the user/session authenticated?
- Is the token valid?
- Does this user own or have permission for this record?
- Is the correct tenant/shop/brand/account filter applied?
- Does the action require admin permission?

Stop and ask Karim if the ownership model is unclear.

## Supabase and RLS

When Supabase is used:
- Assume RLS is enabled or should be enabled for protected tables.
- Do not bypass RLS from frontend code.
- Use service-role keys only in secure backend/server contexts.
- Filter protected data by the correct ownership field.
- Do not create or modify RLS policies without approval.
- Test RLS behavior before treating it as safe.

## Webhooks

For webhook work:
- Verify webhook signatures when the provider supports it.
- Do not trust webhook payloads without validation.
- Store webhook secrets only in secure environment variables.
- Make webhook processing idempotent when duplicate events are possible.
- Log event IDs and safe metadata, not secrets or full sensitive payloads.

## External APIs and AI Providers

For external API or AI calls:
- Keep API keys server-side.
- Validate payload size and content.
- Add cost awareness for paid calls.
- Avoid sending unnecessary customer/private data to AI providers.
- Do not trigger expensive batch jobs without approval.
- Handle provider errors safely.

## Uploads and Files

For uploads:
- Validate file type, size, and source.
- Do not trust file names.
- Avoid storing private files in public buckets unless intended.
- Use signed URLs when appropriate.
- Do not expose internal storage paths unnecessarily.

## Logging Rules

Allowed to log:
- Safe IDs
- Status codes
- Event names
- Timestamps
- Non-sensitive debug summaries

Do not log:
- API keys
- Tokens
- Passwords
- Full auth headers
- Payment details
- Private customer data
- Raw webhook secrets
- Service-role keys

## Security Review Checklist

Before reporting completion, check:
- No secrets exposed.
- No private logic moved to frontend.
- Inputs are validated.
- Protected data is owner-filtered.
- Errors are safe.
- External calls are server-side.
- Approval gates were respected.

## Stop Conditions

Stop and ask Karim before continuing if:
- Auth model is unclear.
- Tenant/ownership model is unclear.
- RLS changes are needed.
- Secrets must be created, moved, rotated, or deleted.
- The task may expose customer/order/payment/private data.
