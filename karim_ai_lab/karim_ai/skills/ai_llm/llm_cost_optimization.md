# LLM Cost Optimization

Purpose:
Help agents design LLM workflows that control cost, latency, retries, quality, and provider usage without weakening validation or production safety.

Pricing and model availability can change.
Do not hardcode current provider pricing as permanent truth.
Verify pricing and model availability when cost matters.

## When to Activate

Use this skill when:

- building workflows that call LLM providers
- choosing models or providers
- processing batches with varying complexity
- controlling cost, latency, retries, or rate limits
- deciding whether deterministic logic can replace an LLM call
- designing fallback or caching behavior
- logging model usage or estimating spend
- preparing production AI workflows

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/backend.md` when LLM calls are server-side
5. `rules/api_routes.md` when AI is exposed through API routes
6. `rules/security.md` when customer data or provider keys are involved
7. `rules/environment.md` when provider keys or env vars are involved
8. `memory/INDEX.md`
9. `memory/by_area/ai.md` if project AI behavior exists
10. `memory/by_area/integrations.md` if provider integration is involved
11. Existing provider, prompt, logging, and billing/credit code

## Execution Pattern

```text
identify task and quality bar
-> check deterministic alternative
-> choose cheapest safe model/provider path
-> set token and budget limits
-> build prompt with only needed context
-> validate output
-> cache safe repeated work
-> retry only transient failures
-> log usage and provider behavior
-> escalate to stronger model only when needed
-> update memory if project-level model/provider/cost behavior becomes stable
```

## Model Routing

Start with the cheapest safe model when quality requirements allow it.
Use expensive models only when needed.

Good routing signals:

- input length
- output complexity
- number of items
- ambiguity level
- customer-visible risk
- reasoning requirement
- prior confidence score
- failure of cheaper model validation

Avoid:

- using the strongest model for every request
- scattering model names across business logic
- hardcoding pricing assumptions
- changing model choice without considering quality, latency, and cost

Separate model choice from business logic where possible.

## Deterministic First

Avoid unnecessary LLM calls.

Use deterministic logic before LLM when:

- data has a stable schema
- rules are explicit
- validation can decide the answer
- formatting or transformation is mechanical
- provider cost or latency is not justified

Use LLM when ambiguity, natural language, classification, summarization, reasoning, or messy extraction requires it.

## Cost Controls

Use:

- token budgets
- max output limits
- prompt compression
- short context windows
- model routing
- batching when appropriate
- caching when safe
- usage logging
- budget guardrails
- per-user or per-tenant limits when product rules require them

Prompt compression:

- Send only relevant fields.
- Remove repeated boilerplate.
- Summarize long history before sending when safe.
- Use stable system instructions instead of repeating long task context.
- Do not remove context needed for safety or correctness.

Caching:

- Cache deterministic prompts and safe repeated outputs when allowed.
- Do not cache private or sensitive content unless the project explicitly supports safe storage.
- Include model, prompt version, input hash, and output schema version when needed.
- Invalidate cache when prompt, model, or rules change.

Batching:

- Batch when it reduces overhead and does not make failures harder to recover.
- Avoid batching unrelated tenants or sensitive customer data together.
- Avoid giant batches that are expensive to retry.

## Retries and Fallbacks

Do not retry blindly if the request is paid or side-effecting.

Retry only when:

- the error is transient
- the operation is safe to repeat
- budget allows it
- idempotency is clear

Do not retry:

- authentication failures
- invalid requests
- schema validation failures without changing the prompt/input
- paid side effects without idempotency
- provider refusal or safety failures without understanding why

Fallback strategy:

- Try cheaper model first when safe.
- Validate output.
- Escalate to stronger model only when validation fails or confidence is low.
- If provider is down, choose a safe fallback: queue, retry later, degraded mode, or user-facing safe error.

## Output Validation

Validate every important LLM output before using it.

Check:

- JSON/schema shape
- required fields
- enum values
- IDs, dates, currency, quantities, and totals
- tenant/ownership boundaries
- unsafe or unsupported claims
- customer-visible wording
- business rules

Never let an LLM be the final authority for payment state, credit deduction, tenant ownership, authorization, refund eligibility, or order truth.

## Usage Logging

Log useful cost and quality signals without exposing secrets or raw private data:

- provider
- model
- prompt or schema version
- input/output token counts when available
- latency
- retry count
- validation result
- fallback/escalation reason
- estimated cost when available

Do not log:

- provider keys
- raw customer secrets
- private files
- payment details
- unnecessary customer data

## Production Safety

Before production use:

- Confirm provider keys are server-side.
- Confirm budget guardrails exist.
- Confirm failure behavior is safe.
- Confirm output validation exists.
- Confirm cost-impacting calls are approved.
- Confirm customer-visible AI output has the right review or retry path.
- Confirm memory is updated if model/provider/cost behavior becomes stable.

## Stop Conditions

Stop and ask Karim if:

- pricing or model availability may affect the decision and has not been verified
- provider choice affects production cost or customer output
- budget limits are unclear
- retry behavior is unclear
- paid or side-effecting calls may repeat
- customer data may be sent to a provider without clear approval
- model output controls billing, credits, payments, authorization, tenant ownership, or order truth
- provider failure behavior is unclear

## Output Expectations

When reporting work:

- State deterministic alternatives considered.
- State model/provider routing approach.
- State budget and token controls.
- State retry and fallback behavior.
- State output validation.
- State usage logging.
- State what pricing/model facts need verification if cost matters.
- Mention memory updates if project-level AI provider or cost behavior became stable.
