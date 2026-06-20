# AI / LLM Skills Index

Purpose:
AI / LLM skills are optional execution playbooks for working with model behavior, cost, deterministic-vs-LLM decisions, RAG/retrieval, prompts, provider behavior, and AI workflow quality.

These skills do not replace core rules, technical rules, or project memory.

## Current Status

Current active files:

- `deterministic_vs_llm.md`
- `llm_cost_optimization.md`

Planned but not created yet:

- `rag_retrieval_patterns.md`

Do not create planned files unless Karim approves.

## How to Use These Skills

Do not use LLMs when deterministic logic is enough.

Use AI / LLM skills when the task involves:

- model or provider choice
- prompt design
- deterministic parsing versus LLM extraction
- cost and latency control
- retries and fallbacks
- output validation
- RAG/retrieval decisions
- AI workflow quality

## Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. Relevant rule files
5. `memory/INDEX.md` if project memory is needed
6. `skills/INDEX.md`
7. `skills/ai_llm/INDEX.md`
8. Relevant AI / LLM skill file

## Token Optimization

Do not read all AI skills by default.

Choose only the relevant file:

- Use `deterministic_vs_llm.md` for parsing, extraction, classification, and deterministic-vs-model decisions.
- Use `llm_cost_optimization.md` for model routing, token budgets, caching, retries, provider failures, and cost control.

## Stop Conditions

Stop and ask Karim if:

- model, provider, or pricing information may be outdated and cost matters
- paid provider calls are cost-impacting
- AI provider behavior is unclear
- prompt or model choice affects production cost
- prompt or model choice affects customer-visible output
- output validation requirements are unclear
- the task needs a planned AI skill file that does not exist yet
