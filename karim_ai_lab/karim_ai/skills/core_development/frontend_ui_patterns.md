# Frontend UI Patterns

Purpose:
Help agents build frontend screens, components, forms, and user flows that are clear, responsive, accessible, and safely connected to backend APIs.

Frontend skills do not override frontend rules.
Before using this skill, read `rules/frontend.md`.
For API-connected UI, also read `rules/api_routes.md`.
For sensitive UI, also read `rules/security.md`.
For backend-owned business logic, also read `rules/backend.md`.

## When to Activate

Use this skill for:

- React or Next.js frontend work
- page and component structure
- UI composition
- forms and validation UX
- loading, error, empty, and success states
- responsive layouts
- dashboard screens
- customer-facing flows
- upload flows
- API integration from frontend
- accessibility and performance review

Do not activate this skill for tiny copy edits unless the change affects a real user flow.

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/frontend.md`
5. `rules/api_routes.md` if the UI calls APIs
6. `rules/security.md` if the UI touches auth, protected data, payments, credits, customer data, uploads, AI providers, admin actions, or tenant data
7. `rules/backend.md` if backend-owned business logic is involved
8. `memory/INDEX.md` and relevant memory files if project UI behavior matters
9. Existing pages, components, hooks, forms, API clients, and design-system files related to the task

## Execution Pattern

Use this frontend flow:

```text
understand user flow
-> inspect existing UI patterns
-> choose page/component boundary
-> define state and data needs
-> validate frontend/backend responsibility split
-> design loading/error/empty states
-> implement accessible responsive UI
-> connect to API safely
-> verify user flow
-> update memory if project UI behavior changed
```

## Practical Checklist

Before implementation:

- Identify the user goal and the exact flow being changed.
- Inspect existing UI components, spacing, typography, forms, and API clients.
- Decide whether the change belongs in a page, component, form, hook, or API client.
- Identify required data, local state, server state, and mutation behavior.
- Confirm backend/API support exists before wiring UI.
- Flag approval gates for checkout, billing, credits, customers, orders, uploads, AI generation, or public claims.

Component structure:

- Keep components focused on one purpose.
- Compose small UI pieces instead of building one large screen file.
- Move reusable state or API behavior into hooks only when reuse or complexity justifies it.
- Keep API clients typed or consistently shaped when the project supports it.
- Follow existing design-system conventions before creating new styles.

Forms:

- Label fields clearly.
- Mark required fields.
- Validate obvious mistakes before submit.
- Show field-level errors when useful.
- Preserve user input after failed submissions.
- Disable submit while submitting.
- Prevent duplicate submissions.
- Show a clear recovery path after errors.

States:

- Design initial loading state.
- Design submitting state.
- Design success state.
- Design empty state.
- Design error state.
- Design disabled state.
- Design retry state when useful.
- Handle slow network behavior for expensive or long-running actions.

Frontend security boundary:

- Frontend should not hold secrets.
- Frontend should not decide protected business logic alone.
- Frontend should not trust user-editable values for price, credits, tenant ownership, roles, or payment state.
- Sensitive actions must be confirmed and handled by backend/API rules.
- Hidden buttons are not authorization.
- Public env vars must be intentionally public.

API integration:

- Call approved backend routes, not private providers directly.
- Handle unexpected response shapes.
- Avoid leaking sensitive data in query strings, logs, or browser console.
- Refresh or update only the relevant data after mutations.
- Do not claim success before backend confirmation.

Upload flows:

- Explain accepted file types.
- Explain size limits.
- Validate file type and size before upload.
- Provide preview when useful.
- Show progress when possible.
- Show clear upload error and retry paths.
- Prevent duplicate uploads.
- Confirm backend validation exists.
- Avoid exposing private storage paths.

Dashboard flows:

- Prioritize clarity, filters, table/list states, and safe actions.
- Make current tenant, shop, brand, or account visible when relevant.
- Reset stale state when switching tenant or context.
- Confirm destructive actions and disable while processing.
- Keep dense interfaces readable and scannable.

Customer-facing flows:

- Keep flows simple and low-friction.
- Show prices, currency, product, order, and policy details clearly when relevant.
- Use trustworthy copy.
- Avoid unnecessary modals or steps.
- Keep mobile layout usable.

Accessibility and responsive basics:

- Use semantic HTML where practical.
- Use buttons for actions.
- Ensure inputs have labels.
- Provide meaningful alt text for informative images.
- Keep keyboard navigation usable.
- Make modals closable and understandable.
- Check important screens at mobile widths.
- Avoid fixed widths that break layout.

Performance basics:

- Avoid unnecessary client-side state.
- Avoid repeated API calls.
- Debounce search and expensive inputs.
- Paginate or virtualize long lists.
- Lazy-load heavy components when useful.
- Optimize images and avoid blocking storefront/customer flows.

## Stop Conditions

Stop and ask Karim if:

- the user flow is unclear
- the design direction is unclear
- backend/API support does not exist
- the UI affects checkout, billing, credits, customers, orders, production data, AI generation, uploads, or public claims
- sensitive logic appears to be required in frontend code
- tenant or ownership context is unclear
- upload requirements are unclear
- the change would add a new dependency or design-system convention
- the UI behavior should be recorded in memory but the correct area is unclear

## Output Expectations

When reporting frontend work:

- Name pages, components, forms, hooks, or API clients created or changed.
- Summarize the user flow affected.
- Mention loading, error, empty, disabled, and success states handled.
- Mention API/backend dependencies.
- Mention auth, tenant, upload, payment, credit, or customer-data implications when relevant.
- Mention responsive and accessibility checks performed.
- Mention memory updates if project UI behavior changed.
