# Project Memory Index

Purpose:
Help agents find and update project-specific memory without loading unnecessary history.

This memory system stores what is true about the project using Karim AI Lab.

It does not replace rules.

Rules live in:

```text
karim_ai/core/
karim_ai/rules/
```

Memory lives in:

```text
karim_ai/memory/
```

## Core Principle

Do not read all memory by default.

Read only the memory files relevant to the current task.

After finishing a task, update the relevant memory file for that area.

## Memory Files

### `summary.md`

Short stable project summary.

Use this file for:

- what the project is
- main product goal
- main architecture summary
- main business logic summary
- important stable context every agent should know

Hard rule:
`summary.md` must never exceed 250 lines.

If it grows beyond 250 lines, compress it before adding more.
Remove unnecessary detail.
Keep only stable, useful context.

Do not use `summary.md` as a long history log.

### `done.md`

Completed work log.

Use this file for:

- completed assets
- completed features
- completed fixes
- important implementation milestones

Each entry should be short.

Recommended entry format:

```text
YYYY-MM-DD - Area - What was completed - Files/paths affected
```

Do not use `done.md` as a todo list.

Do not add future tasks here.

### `by_area/architecture.md`

Project-specific architecture memory.

Use for:

- actual project structure
- important module boundaries
- major data flow notes
- app architecture decisions already implemented
- framework-specific structure

### `by_area/security.md`

Project-specific security memory.

Use for:

- auth model
- authorization model
- tenant isolation notes
- sensitive data handling
- security issues fixed
- production safety notes

### `by_area/database.md`

Project-specific database memory.

Use for:

- tables
- ownership fields
- RLS notes
- migrations already applied
- indexes
- vector storage details
- schema issues and fixes

### `by_area/backend.md`

Project-specific backend memory.

Use for:

- backend services
- server functions
- private integrations
- background jobs
- queues
- webhook processing
- credit/payment backend behavior

### `by_area/api_routes.md`

Project-specific API route memory.

Use for:

- route contracts
- request/response formats
- webhook endpoints
- callback endpoints
- server actions
- route issues and fixes

### `by_area/frontend.md`

Project-specific frontend memory.

Use for:

- UI structure
- page structure
- component patterns
- forms
- user flows
- loading/error states
- design notes
- customer-facing screens

### `by_area/environment.md`

Project-specific environment memory.

Use for:

- required env vars
- local/staging/production differences
- deployment configuration
- public/private env boundaries
- setup notes

Never store real secret values.

### `by_area/integrations.md`

Project-specific integration memory.

Use for:

- Shopify
- WooCommerce
- Stripe
- PayPal
- n8n
- SendPulse
- Meta
- Google Cloud
- Supabase
- external APIs
- webhook providers

### `by_area/ai.md`

Project-specific AI memory.

Use for:

- AI providers
- models
- embeddings
- prompts that became stable product logic
- vector/RAG behavior
- AI cost behavior
- AI output handling
- image/video/audio generation flows

### `by_area/business_logic.md`

Project-specific business logic memory.

Use for:

- product rules
- pricing rules
- credit rules
- billing rules
- refund/exchange logic
- order rules
- customer flow rules
- brand-specific behavior
- operational policies

## How to Read Memory

Use this pattern:

```text
START_HERE.md
-> INDEX.md
-> memory/INDEX.md
-> summary.md only if broad project context is needed
-> relevant by_area file
-> done.md only if completed history is needed
```

Examples:

Frontend task:

```text
memory/INDEX.md -> by_area/frontend.md
```

Backend task:

```text
memory/INDEX.md -> by_area/backend.md
```

Database task:

```text
memory/INDEX.md -> by_area/database.md
```

AI feature task:

```text
memory/INDEX.md
-> by_area/ai.md
-> by_area/backend.md if backend work is involved
-> by_area/database.md if AI outputs or vectors are stored
```

Shopify integration task:

```text
memory/INDEX.md
-> by_area/integrations.md
-> by_area/backend.md
-> by_area/api_routes.md if endpoints/webhooks are involved
```

Billing or credits task:

```text
memory/INDEX.md
-> by_area/business_logic.md
-> by_area/backend.md
-> by_area/database.md
```

## How to Update Memory After Work

After finishing a task:

- Update the most relevant `by_area/*.md` file.
- Add only useful project-specific memory.
- Add a short completed entry to `done.md`.
- Update `summary.md` only if stable project-level understanding changed.
- Keep `summary.md` under 250 lines.

Do not update every memory file.

Do not duplicate the same note across many files.

Do not add temporary thoughts unless they are useful later.

## What Not to Store

Do not store:

- general coding rules
- generic security advice
- generic frontend advice
- generic backend advice
- generic database advice
- temporary todo lists
- unclear future ideas
- outdated assumptions
- private secrets
- real API keys
- raw customer data
- long chat transcripts
- noisy implementation logs

## Stop Conditions

Stop and ask Karim if:

- the correct memory area is unclear
- the memory note may become misleading
- a summary update would exceed 250 lines
- the task belongs to multiple areas and needs a memory structure decision
- the agent is tempted to create a new memory file

## New Memory File Rule

Do not create new memory files unless Karim approves.

Use the existing area files first.

If a new area is truly needed, propose it and explain why.
