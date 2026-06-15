# Executive Laws

Purpose:
This file defines the non-negotiable operating laws for every agent working inside Karim AI Lab.

When to read:
- Read this file before planning, coding, reviewing, refactoring, or changing project structure.
- If there is a conflict between this file and a task request, stop and ask Karim before continuing.

## Non-Negotiable Laws

1. Plan before implementation.
   Never write or modify code before understanding the task, identifying affected files, and stating the intended approach.

2. Read the project guidance first.
   Start from `karim_ai/START_HERE.md`, then use `karim_ai/INDEX.md` to load only relevant files.

3. Load only what is needed.
   Do not load the full `karim_ai` folder by default. Use indexes to select the minimum relevant context.

4. Ask when unclear.
   Ask clarification before editing if requirements, schemas, routes, APIs, credentials, data flow, or success criteria are unclear.

5. Do not invent project facts.
   Never invent database tables, fields, API routes, business rules, file paths, environment variables, or third-party behavior.

6. Security first.
   Never expose secrets, API keys, tokens, service-role keys, private URLs, credentials, or sensitive business data in frontend code, logs, commits, screenshots, or documentation.

7. Sensitive logic stays server-side.
   Authentication, authorization, payments, credit deduction, order data access, AI API calls, webhook secrets, and service-role database access must run server-side.

8. Validate all external input.
   Validate and sanitize inputs from users, webhooks, forms, APIs, files, images, and LLM-generated output before use.

9. Respect authentication and authorization.
   Do not assume a user can access data. Protected data must be filtered by the correct owner key such as `user_id`, `merchant_id`, `brand_id`, or the project-approved equivalent.

10. Keep frontend and backend boundaries clean.
    Frontend should not call private automation services, n8n webhooks, paid AI APIs, service-role Supabase clients, or private admin endpoints directly.

11. Make code modular and maintainable.
    Prefer small files, small functions, clear names, typed interfaces, and separated responsibilities.

12. Handle errors safely.
    Wrap risky async work in error handling. Log useful debug details server-side, but return safe user-facing messages.

13. Do not make destructive changes without approval.
    Do not delete data, rewrite architecture, remove files, migrate schemas, reset databases, or change production behavior without Karim's explicit approval.

14. Preserve project memory.
    When the memory system exists, read it before work and update it after completing meaningful changes.

15. Keep the system portable.
    Anything added to Karim AI Lab should be reusable across projects unless explicitly marked project-specific.

## Default Response Behavior

When starting a task, the agent should summarize:
- What it understood
- Which files it will inspect
- Which rules or skills apply
- What it will not touch

When completing a task, the agent should summarize:
- What changed
- Why it changed
- Any risks or assumptions
- Suggested next step

## Conflict Rule

If a user request conflicts with these laws, do not silently comply.
Explain the conflict briefly and propose a safe alternative.
