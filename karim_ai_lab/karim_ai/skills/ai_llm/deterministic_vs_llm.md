# Deterministic vs LLM

Purpose:
Help agents decide when to use deterministic parsing/code and when to use an LLM for extraction, classification, summarization, intent detection, or reasoning.

Do not use LLM for everything.
Deterministic logic is preferred for stable formats.
LLM output must be validated before being trusted.

## When to Activate

Use this skill when:

- parsing structured text with repeating patterns
- extracting order, product, customer, email, webhook, log, or form data
- deciding between regex, schema parsing, rules, and LLM calls
- building hybrid extraction pipelines
- optimizing cost, latency, and reliability for text processing
- handling messy text, ambiguity, intent, classification, summarization, or reasoning

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. `rules/backend.md` when backend logic is involved
5. `rules/api_routes.md` when API or webhook input is involved
6. `rules/database.md` when extracted data is stored
7. `rules/security.md` when sensitive data is involved
8. `memory/INDEX.md`
9. `memory/by_area/ai.md` if project AI behavior exists
10. `memory/by_area/business_logic.md` when extraction affects business rules
11. Existing parser, API, webhook, data, and provider code

## Decision Framework

```text
Is the input format consistent and repeating?
-> Yes: start with deterministic parsing
   -> Deterministic logic handles most cases: no LLM needed
   -> Deterministic logic misses edge cases: add LLM for low-confidence cases only
-> No: consider LLM for messy text, ambiguity, intent, summarization, classification, or reasoning
```

Use deterministic logic for:

- known webhook payloads
- stable API responses
- CSV or JSON with a schema
- product SKUs, IDs, prices, quantities, currencies, and dates in stable formats
- logs with consistent prefixes or fields
- form fields and typed inputs
- repeated email templates with predictable markers

Use an LLM for:

- messy customer messages
- natural-language intent
- ambiguous support requests
- classification with fuzzy categories
- summarization
- reasoning over multiple text fragments
- extraction where stable patterns are not enough

## Architecture Pattern

```text
Source text or payload
-> deterministic parser
-> cleaner / normalizer
-> confidence scorer
-> high confidence: accept deterministic output
-> low confidence: call LLM validator/extractor
-> validate LLM output against schema/business rules
-> return safe structured result
-> log metrics
-> update memory if a stable project parsing/extraction rule is created
```

## Practical Checklist

Before choosing an LLM:

- Check whether the input has a stable schema or repeated format.
- Try deterministic parsing first when the format is stable.
- Define required output fields.
- Define validation rules for each output field.
- Define confidence scoring or failure detection.
- Decide what happens when extraction is low-confidence.
- Estimate expected volume and cost impact.

Deterministic parser guidance:

- Use schema parsing for JSON, API, and webhook payloads.
- Use regex or parser code for stable text patterns.
- Normalize whitespace, punctuation, casing, currency, and dates before matching when useful.
- Return structured data and confidence metadata.
- Avoid silently accepting partial extraction when required fields are missing.

Confidence scoring:

- Score missing required fields.
- Score invalid date, currency, quantity, email, phone, URL, or ID formats.
- Score unexpected category or status values.
- Score conflicting fields, such as total not matching line items.
- Flag low-confidence outputs for LLM review or human review.

LLM extraction:

- Send only the minimum text needed.
- Ask for structured output.
- Validate response against a schema.
- Reject or retry invalid output carefully.
- Do not trust model-provided prices, credits, tenant ownership, roles, or payment state.
- Keep provider keys server-side.

Hybrid extraction:

- Use deterministic parsing for the majority path.
- Use LLM only for low-confidence or ambiguous edge cases.
- Track how often LLM fallback is used.
- Review recurring fallback cases and convert stable patterns into deterministic rules.

Cost and latency:

- Paid or high-volume LLM use must consider cost.
- Avoid LLM calls in hot paths when deterministic logic is enough.
- Cache safe repeated results when appropriate.
- Batch only when it does not harm correctness, privacy, or user experience.

Examples:

- Order emails: parse order ID, total, currency, items, and customer email deterministically when templates are stable; use LLM only for messy forwarded customer messages.
- Product data: use schema validation for product feeds; use LLM only to classify messy descriptions or infer tags.
- Webhooks: validate provider payload schema and signature; do not use LLM to interpret trusted structured fields.
- Support messages: use LLM for intent and summary, then validate any extracted IDs, dates, or amounts deterministically.
- Logs: use deterministic parsing for known log formats; use LLM only for summarizing incidents after sensitive data is removed.

## Stop Conditions

Stop and ask Karim if:

- the extracted data affects payments, credits, orders, customers, tenant ownership, billing, or production data
- confidence thresholds are unclear
- validation rules are unclear
- LLM usage would be high-volume or paid and cost approval is unclear
- sensitive data may be sent to a provider
- fallback behavior is unclear
- model output would become customer-visible without review

## Output Expectations

When reporting work:

- State whether deterministic, LLM, or hybrid extraction was chosen.
- Explain why.
- Name validation and confidence checks.
- Mention cost and latency considerations.
- Mention sensitive-data handling.
- Mention memory updates if a stable project parsing or extraction rule was created.
