# RAG Retrieval Patterns

Purpose:
Help agents design and use retrieval, RAG, and context-loading systems without loading too much context or missing important information.

Karim AI Lab uses index-first retrieval.
This skill must not encourage random "search first" behavior.

## Core Philosophy

Use this order:

```text
index first
-> rules/memory/skills first
-> known project files first
-> targeted retrieval only when context is missing
-> broaden search only when needed
```

Do not search the entire repo before using indexes.
Do not read all memory by default.
Do not read all skills by default.
Retrieve less first, then expand only if needed.

## When to Activate

Use this skill for:

- RAG system design
- vector search and semantic retrieval
- keyword and hybrid retrieval
- source-grounded answers
- context loading for AI workflows
- support agents and chatbots
- product docs search
- log and incident retrieval
- retrieval over product knowledge, help content, or project files
- context compression before LLM calls

## Required Read Order

Read:

1. `START_HERE.md`
2. `INDEX.md`
3. `rules/INDEX.md`
4. Relevant rule files
5. `memory/INDEX.md` when project memory is needed
6. Relevant `memory/by_area/` file
7. `skills/INDEX.md`
8. `skills/ai_llm/INDEX.md`
9. This file
10. Actual source files, docs, or data sources needed for the task

## Retrieval Flow

Use this flow:

```text
read entry/index files
-> identify task and needed context
-> read relevant rules
-> read relevant memory if project-specific context is needed
-> inspect known affected files
-> retrieve targeted extra context only if missing
-> decompose broad questions into focused queries
-> use hybrid keyword/vector retrieval when useful
-> filter/rerank for relevance
-> compress retrieved context before sending to LLM
-> validate output against source facts
-> update memory if stable retrieval behavior or project knowledge changed
```

## Retrieval Method Selection

Use the cheapest reliable method first:

```text
Exact path/file known -> open/read that file directly
Exact symbol/error/table/route known -> keyword search first
Conceptual or fuzzy query -> vector/semantic retrieval
Mixed exact + conceptual query -> hybrid retrieval
Customer support/product knowledge -> retrieve source chunks + dynamic data separately
AI chatbot answer -> retrieve facts, then validate final answer against retrieved source
Debugging current code -> current source code beats memory
Project history -> memory/done.md or relevant by_area file
Broad architecture question -> memory/summary.md + architecture memory + actual source tree/files
```

Use keyword search for:

- exact names
- routes
- tables
- env vars
- errors
- file paths
- function or class names
- webhook event names
- product IDs, order IDs, or SKUs

Use vector or semantic retrieval for:

- concepts
- product descriptions
- support knowledge
- fuzzy questions
- natural-language similarity
- related documentation
- user intent

Use hybrid retrieval when exact and semantic signals both matter.

Do not use vector retrieval when exact file/path lookup is better.

## Progressive Retrieval

Start narrow:

1. Read the relevant index.
2. Read the obvious rule, memory, skill, or source file.
3. Search exact terms if something is missing.
4. Decompose broad questions into focused queries.
5. Add semantic retrieval only when exact lookup is not enough.
6. Broaden only after identifying the missing context.

Avoid:

- loading every file in a folder
- reading all memory
- reading all skills
- searching the whole repo before checking indexes
- keeping low-relevance chunks because they might be useful
- letting repeated snippets crowd out stronger source context

## RAG System Design Notes

Chunks should be meaningful and not too large.

Good chunks usually preserve:

- one concept
- one procedure
- one API behavior
- one product rule
- one troubleshooting case
- one source section with enough surrounding context

Metadata matters.
Store useful metadata such as:

- source path or URL
- document ID
- section heading
- product area
- tenant/shop/account if safe and relevant
- created/updated timestamp
- version
- permissions or visibility
- embedding model/version when useful

Static knowledge and dynamic data should usually be separate.

- Static knowledge: docs, policies, feature explanations, stable help content.
- Dynamic live data: orders, customer status, billing state, inventory, credits, account permissions.

Dynamic data should usually not be embedded if it changes often.
Fetch live data from authoritative systems when exact current state matters.

Source URLs, paths, or IDs should be stored as metadata.
Retrieval should return enough source context to verify the answer.
Avoid duplicate chunks.
Log retrieval quality where useful.
Handle no-result and low-confidence states safely.
Use deterministic filters before semantic retrieval when possible.

## Source Grounding and Validation

Source-ground important claims.

Do not let RAG output invent product behavior.
Do not trust retrieved snippets if they conflict with current code or locked mission/rules.
Memory files can be stale; inspect source files when exact behavior matters.

If retrieval affects customer-visible answer, billing, credits, auth, orders, refunds, or production data, validate against authoritative sources.

Authoritative sources may include:

- current source code
- database schema or migrations
- API contracts
- backend services
- product configuration
- provider docs
- locked mission and rules
- approved project memory when exact source files are not available

## Context Compression

Compress retrieved context before sending to expensive models.

When compressing:

- keep source identifiers
- remove duplicates
- keep facts needed for correctness and safety
- preserve constraints and edge cases
- preserve dates, versions, paths, IDs, and ownership boundaries when they matter
- separate facts from interpretation

Do not compress away:

- restrictions
- prices
- ownership rules
- policy notes
- security requirements
- billing or credit behavior
- refund/order conditions
- edge cases that affect behavior

## Common Use Cases

Product RAG:

- Retrieve product docs and help content.
- Fetch live account/order/billing data separately.
- Validate customer-visible answers against source docs and live data.

Support agent:

- Retrieve relevant help articles, policies, and known troubleshooting notes.
- Check dynamic user/order/account state from backend systems.
- Return safe answers with escalation when confidence is low.

Docs chatbot:

- Retrieve source chunks with paths or URLs.
- Answer only from retrieved facts.
- Say when the answer is not found.

Logs and incidents:

- Use keyword search for exact errors, request IDs, paths, and timestamps.
- Use semantic grouping only after exact signals are collected.
- Avoid sending secrets or private customer data to providers.

AI workflow:

- Retrieve only task-relevant rules, memory, skill, and project files.
- Compress context before model calls.
- Validate output against current source facts.

## Memory Updates

Update memory when stable retrieval behavior or project knowledge changed.

Examples:

- a project uses a specific vector table or embedding model
- retrieval source priority becomes stable
- support answers must prefer one source over another
- a chunking or metadata convention becomes project standard
- a known stale source is retired or replaced

Do not update every memory file.
Use the relevant `memory/by_area/` file.

## Stop Conditions

Stop and ask Karim if:

- the right source of truth is unclear
- retrieved sources conflict
- memory conflicts with current code
- retrieval result affects billing, credits, orders, refunds, auth, tenant ownership, payments, production data, or customer-visible claims
- vector index may be stale
- source data contains sensitive information and provider usage is unclear
- the agent is tempted to broaden search too much instead of asking a focused question

## Output Expectations

When reporting retrieval or RAG work, state:

- what sources were read
- which retrieval method was used
- why that method was chosen
- what was excluded
- confidence level
- validation performed
- memory updates if stable retrieval behavior or project knowledge changed
