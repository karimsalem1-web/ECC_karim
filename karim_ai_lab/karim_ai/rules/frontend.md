# Frontend Rules

Purpose:
Guide agents when working with frontend screens, UI components, forms, client-side state, API calls, loading states, user flows, dashboards, Shopify extensions, AI tool interfaces, and customer-facing pages.

When to read:
- Read this file before creating or editing frontend logic.
- Always read before touching pages, components, hooks, forms, dashboards, admin screens, customer portals, Shopify UI extensions, app proxy pages, AI result screens, upload flows, checkout-related UI, or frontend API clients.

## Frontend Principles

1. Frontend is a user experience layer, not a security layer.
   The frontend may guide users, but it must not protect secrets, enforce final authorization, verify payments, deduct credits, or perform sensitive business logic.

2. Keep sensitive logic server-side.
   Auth checks, ownership verification, service-role database access, paid AI calls, payment logic, credit deduction, private integrations, and secret-based operations belong on the backend.

3. Components should be small and focused.
   A component should have a clear purpose. Avoid giant components that mix UI, API calls, validation, business logic, and layout.

4. Separate rendering from logic when complexity grows.
   UI components should render. Hooks/services should handle state, API calls, and reusable logic.

5. Every user action needs clear feedback.
   Show loading, success, error, disabled, empty, and edge states where needed.

6. Never trust frontend state for protected decisions.
   Frontend state can be changed by users. Backend must verify all protected actions.

7. UI must respect the product flow.
   Do not add buttons, steps, pages, modals, or fields that change the business process without approval.

8. Frontend changes can break trust.
   Bad UI can cause wrong orders, wrong payments, duplicate actions, failed uploads, customer confusion, or poor AI results.

## Mandatory Read Order Before Frontend Work

Before frontend work, read:

1. `karim_ai/START_HERE.md`
2. `karim_ai/INDEX.md`
3. `karim_ai/core/executive_laws.md`
4. `karim_ai/core/approval_gates.md`
5. `karim_ai/core/working_protocol.md`
6. `karim_ai/rules/security.md`
7. `karim_ai/rules/environment.md`
8. `karim_ai/rules/api_routes.md` when API calls are involved
9. `karim_ai/rules/backend.md` when sensitive actions are involved
10. `karim_ai/rules/database.md` when data shape is involved
11. Project memory files if they exist
12. Existing frontend files related to the same feature

Do not redesign frontend flows from memory only.

## Approval Gate

Ask for explicit approval before:

- Adding a new user-facing page
- Changing a core user flow
- Changing checkout/payment/billing UI
- Changing credit/subscription display
- Changing order/customer/product screens
- Changing upload or AI generation flows
- Changing Shopify storefront/app extension UI
- Moving sensitive logic from backend to frontend
- Adding new external tracking scripts
- Adding new frontend dependencies
- Changing design system conventions
- Removing existing validation or error states
- Changing public marketing claims

Agents may propose frontend changes, but must not apply high-risk UI/product flow changes until approved.

## Frontend Responsibility Rules

Frontend may handle:

- Rendering screens
- Collecting user input
- Basic input validation for UX
- Calling approved APIs
- Showing loading states
- Showing safe error messages
- Displaying user-owned data returned by backend
- Managing local UI state
- Triggering uploads through signed URLs
- Navigating between steps
- Presenting AI results safely

Frontend must not handle:

- Private API keys
- Service-role database access
- Final authorization decisions
- Payment verification
- Credit deduction
- Webhook processing
- Direct n8n calls with secrets
- Paid AI provider calls with secret keys
- Cross-tenant filtering as the only protection
- Sensitive business rules that must be enforced server-side

## Component Structure Rules

Prefer clear separation:

```text
page/screen: route-level layout and composition
component: focused UI unit
form: input collection and validation UX
hook: reusable state or client-side logic
api client: typed calls to backend routes
types: shared frontend TypeScript types
utils: pure helpers and formatters
```

Bad:

One huge page file handles UI, validation, API calls, auth logic, payment decisions, AI prompt building, and error handling.

Good:

Page composes the screen.
Form collects input.
Hook manages client state.
API client calls backend.
Backend performs sensitive work.

Do not force heavy structure for tiny projects, but preserve separation as complexity grows.

## Data Flow Rules

Preferred frontend data flow:

```text
User action
-> Frontend validation for UX
-> Backend/API route/server action
-> Backend validation + authorization
-> Database/provider/AI/service
-> Safe response
-> Frontend updates UI
```

Rules:

- Do not call private providers directly from frontend.
- Do not send unnecessary private data.
- Do not expose raw provider responses unless safe and intended.
- Do not rely on frontend-only filtering for private data.
- Do not mutate protected business state without backend confirmation.

## API Client Rules

Frontend API calls must:

- Use approved backend routes.
- Handle loading state.
- Handle safe error state.
- Handle empty state.
- Handle unexpected response shape.
- Avoid duplicate submissions.
- Avoid leaking secrets in query strings.
- Avoid exposing sensitive payloads in logs.
- Use consistent response handling.

Preferred API response assumption:

```json
{
  "success": true,
  "message": "Human-readable summary",
  "data": {},
  "error": null
}
```

Error response:

```json
{
  "success": false,
  "message": "Safe human-readable error",
  "data": null,
  "error": "SAFE_ERROR_CODE"
}
```

If the project has an existing response format, follow it.

## Form Rules

Forms must be designed for real users.

Rules:

- Label inputs clearly.
- Mark required fields.
- Validate before submit for obvious mistakes.
- Show field-level errors when useful.
- Disable submit during submission.
- Prevent duplicate submissions.
- Preserve user input when errors happen.
- Use safe defaults.
- Do not collect unnecessary private data.
- Do not hide critical terms, prices, or actions.

For business-critical forms, confirm:

- What data is required?
- What happens after submit?
- Can the action be undone?
- Does it affect payments, credits, orders, customers, or messages?
- Does backend validation exist?

## Loading, Empty, Error, and Success States

Every interactive frontend feature should consider:

- Initial loading state
- Submitting state
- Success state
- Empty state
- Error state
- Retry state when useful
- Disabled state
- Partial data state
- Slow network state

Bad:

Button does nothing visually after click and user may click it multiple times.

Good:

Button shows loading, disables duplicate clicks, and displays success or safe error after response.

## State Management Rules

Keep state as simple as possible.

Use local state for:

- UI toggles
- form values
- modal open/close
- temporary selections
- simple loading/error state

Use shared/global state only when:

- multiple unrelated components need the same state
- state persists across routes
- auth/session data is involved
- app-wide notifications are needed

Avoid:

- duplicating server data in many places
- storing secrets in frontend state
- storing sensitive tokens in localStorage
- complex global state for small UI problems
- stale state after mutations

After a mutation, refresh or update only the relevant data.

## Security Rules

Frontend must never contain:

- API secrets
- service-role keys
- private database URLs
- provider secret keys
- webhook secrets
- payment secret keys
- private n8n keys
- admin credentials

Public env vars must be intentionally public.

Allowed public examples:

- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`

Forbidden public examples:

- `SUPABASE_SERVICE_ROLE_KEY`
- `STRIPE_SECRET_KEY`
- `SHOPIFY_API_SECRET`
- `CLAUDE_API_KEY`
- `OPENAI_API_KEY`
- `N8N_API_KEY`

Never log sensitive user data or secrets in the browser console.

## Authentication UI Rules

Frontend may show or hide UI based on session state, but backend must enforce real access.

Rules:

- Do not assume hidden buttons equal security.
- Do not show protected data before auth is confirmed.
- Handle expired sessions gracefully.
- Redirect or prompt login when needed.
- Do not store sensitive auth tokens insecurely.
- Do not trust client-only roles for protected actions.

Admin UI must be extra careful:

- Clearly separate admin routes from customer/user routes.
- Do not expose admin-only data to normal users.
- Confirm backend authorization exists for admin actions.

## Multi-Tenant UI Rules

For SaaS, Shopify, WooCommerce, CRM, or dashboard projects:

- Clearly show the current shop/brand/account when relevant.
- Do not mix data between tenants.
- Do not let frontend choose tenant without backend verification.
- Avoid showing stale data from a previously selected tenant.
- Refresh state when switching shop/brand/account.
- Make destructive actions tenant-specific and explicit.

Example risk:

User switches from Brand A to Brand B but cached orders from Brand A remain visible.

Prevent this with clear state reset and backend-filtered data.

## E-commerce UI Rules

For product, order, customer, checkout, refund, exchange, or billing UI:

- Show prices clearly.
- Show currency clearly.
- Show product/variant information accurately.
- Do not hide critical policies.
- Confirm destructive actions.
- Avoid duplicate order/customer actions.
- Validate phone/email/address fields.
- Keep customer-facing text clear and trustworthy.
- Do not claim an action succeeded before backend confirmation.
- Do not display unsupported return/refund promises.

For Shopify extensions/app proxy/storefront UI:

- Respect Shopify context.
- Keep storefront UI lightweight.
- Avoid blocking product pages.
- Avoid exposing private app secrets.
- Make installation/shop context explicit when needed.
- Confirm backend handles shop ownership and billing.

## AI Tool UI Rules

For AI generation, try-on, chatbot, content, image, video, or automation interfaces:

- Explain what the user should input.
- Show upload requirements.
- Validate file type and size.
- Show generation progress.
- Show safe failure messages.
- Avoid duplicate generation clicks.
- Show credit/cost impact before paid actions when applicable.
- Do not show raw model/provider errors.
- Do not send unnecessary private data to AI providers.
- Do not claim AI output is guaranteed accurate.
- Allow users to retry when safe.

For long-running AI tasks:

```text
start job
-> show progress/status
-> poll or subscribe to status
-> show result or safe failure
```

Do not block the UI without feedback.

## Upload UI Rules

Upload flows must:

- Explain accepted file types.
- Explain size limits.
- Validate before upload.
- Show progress when possible.
- Show upload success/failure.
- Prevent duplicate uploads.
- Use signed URLs when backend provides them.
- Avoid showing private storage paths.
- Handle slow networks.
- Handle mobile users.

Do not upload private files to public storage unless the product explicitly requires public access.

## Accessibility Rules

Frontend should be usable by real people.

Rules:

- Use semantic HTML when possible.
- Buttons must be buttons, not clickable divs unless necessary.
- Inputs need labels.
- Images need meaningful alt text when informative.
- Loading states should be understandable.
- Errors should be readable.
- Text contrast should be acceptable.
- Keyboard navigation should not be broken.
- Modals should be closable and understandable.

Do not sacrifice basic accessibility for visual style.

## Mobile and Responsive Rules

Assume users may be on mobile.

Rules:

- Check important screens on mobile sizes.
- Avoid fixed widths that break layout.
- Avoid tiny click targets.
- Keep forms readable.
- Keep modals usable.
- Avoid horizontal scrolling unless intentional.
- Test upload and camera flows on mobile when relevant.
- Keep storefront and customer-facing UI fast.

For Instagram/marketing/admin workflows, mobile usability may be critical.

## Performance Rules

Frontend should feel fast and avoid unnecessary work.

Rules:

- Avoid heavy client bundles when possible.
- Lazy-load heavy components when useful.
- Avoid repeated API calls.
- Debounce search inputs.
- Paginate long lists.
- Avoid rendering huge lists without virtualization/pagination.
- Optimize images.
- Avoid loading private dashboards before auth is known.
- Avoid expensive client-side processing when backend is better.

For e-commerce storefronts:

- Do not slow product pages unnecessarily.
- Avoid heavy scripts.
- Avoid blocking add-to-cart or checkout-related flows.

## Design Consistency Rules

Follow the project's existing design system.

Before changing design:

- Inspect existing components.
- Inspect spacing conventions.
- Inspect color usage.
- Inspect typography.
- Inspect button styles.
- Inspect form styles.
- Reuse existing UI components where practical.

Avoid:

- random colors
- inconsistent button styles
- one-off spacing
- unnecessary animations
- mixing multiple UI systems
- changing brand tone without approval

If the project has no design system, keep the UI simple, clean, and consistent.

## Copywriting Rules

Frontend text should be clear and user-friendly.

Rules:

- Use simple labels.
- Avoid technical errors for normal users.
- Explain next steps.
- Do not overpromise.
- Do not expose internal system terms unless user needs them.
- Keep CTA text action-focused.
- For customer-facing e-commerce flows, use trustworthy language.
- For admin dashboards, use precise language.

Bad:

Mutation failed due to provider exception.

Good:

We could not complete this action. Please try again.

## Internationalization and Language Rules

If the project supports multiple languages:

- Do not hardcode all copy inside components when a translation system exists.
- Keep language consistent on each page.
- Respect RTL/LTR requirements when Arabic is involved.
- Avoid mixing Arabic and English unless intentionally designed.
- Keep error messages translatable.
- Do not break layout with longer translated text.

For Karim's projects, Arabic/English support may matter. Ask before adding language-specific behavior.

## Data Display Rules

When displaying data:

- Format dates clearly.
- Format currency clearly.
- Format percentages clearly.
- Avoid showing raw IDs unless useful.
- Use human-readable statuses.
- Handle missing/null values.
- Show skeleton/empty states for missing data.
- Avoid exposing internal/private fields.

For money:

- Always show currency.
- Do not calculate final payment/credit truth only in frontend.
- Backend remains source of truth.

## Destructive Action UI Rules

Destructive actions include:

- delete
- archive
- cancel
- refund
- remove access
- disconnect integration
- reset data
- spend credits
- send message
- publish public content

Rules:

- Confirm destructive actions.
- Explain consequence.
- Use clear button labels.
- Avoid accidental clicks.
- Disable while processing.
- Show backend-confirmed result.
- Do not hide irreversible impact.

High-risk destructive actions require approval before implementation.

## Notifications and Toasts

Use notifications carefully.

Rules:

- Success messages should confirm what happened.
- Error messages should be safe and helpful.
- Do not show raw backend/provider errors.
- Avoid too many toasts.
- Persist important errors long enough to read.
- Use inline errors for forms when possible.

## Testing and Review Rules

Before considering frontend work complete, check:

- Main happy path
- Empty state
- Loading state
- Error state
- Validation behavior
- Mobile layout
- Auth/logout behavior if relevant
- Duplicate click protection
- Backend error handling
- Data refresh after mutation
- No secrets in browser code
- No sensitive console logs

For critical flows, test with realistic data.

## Stop Conditions

Stop and ask Karim if:

- The user flow is unclear.
- The design direction is unclear.
- A new page or major UI flow is needed.
- A frontend change affects payment, billing, credits, customers, orders, or production data.
- Sensitive logic appears to be required in frontend.
- API contract is unclear.
- Backend route does not exist.
- Upload limits or file rules are unclear.
- Shopify/storefront context is unclear.
- AI generation cost/credit behavior is unclear.
- A design system decision is needed.
- Text/copy changes affect public claims or policies.

## Frontend Work Checklist

Before editing:

- Read relevant Karim AI Lab files.
- Inspect existing frontend structure.
- Identify affected pages/components/hooks.
- Identify API routes used.
- Identify auth/tenant needs.
- Identify loading/error/empty states.
- Identify mobile impact.
- Identify approval-gated changes.

Before committing:

- No secrets in frontend.
- No service-role access in frontend.
- API calls use approved backend routes.
- Inputs have validation.
- Loading state exists.
- Error state exists.
- Empty state exists where needed.
- Duplicate actions are prevented.
- Mobile layout is considered.
- UI follows existing design conventions.
- Sensitive business logic remains backend-side.
- Documentation/memory updated if required.

Before production:

- Test with real user flow.
- Test on mobile size.
- Test failed API response.
- Test slow network/long job behavior when relevant.
- Check browser console for secret/data leaks.
- Confirm backend enforces protected actions.
- Confirm public text and claims are approved.
- Confirm no production-breaking UX changes.

## Forbidden Frontend Actions

Agents must not:

- Put secrets in frontend code.
- Put service-role keys in frontend.
- Call paid AI providers directly with secret keys.
- Call n8n directly when secrets or protected data are involved.
- Trust frontend owner IDs for protected actions.
- Use frontend filtering as the only tenant protection.
- Deduct credits in frontend.
- Verify payments only in frontend.
- Hide buttons as the only authorization method.
- Show raw stack traces/provider errors.
- Log private customer data unnecessarily.
- Add major UI flows without approval.
- Change checkout/payment/billing UI without approval.
- Remove loading/error states from important flows.
- Break mobile layout for customer-facing flows.

## Default Response Pattern for Frontend Requests

When Karim asks for frontend work, respond in this order:

1. Restate the frontend goal.
2. Identify existing files inspected.
3. Identify affected user flow.
4. Identify API/backend dependencies.
5. Identify auth/tenant implications.
6. List proposed frontend changes.
7. Flag approval-required items.
8. Apply only approved changes.
9. Summarize changed files.
10. Update project memory if it exists.

For small non-sensitive UI fixes, the agent may proceed if the requested change is clear and inside an already-approved task.

After updating frontend.md:

- Do not modify database.md.
- Do not modify backend.md.
- Do not modify api_routes.md.
- Do not modify rules/INDEX.md.
- Report only what changed and confirm that only frontend.md was edited.
