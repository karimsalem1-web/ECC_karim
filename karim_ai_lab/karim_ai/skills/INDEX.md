# Skills Index

Purpose:
Skills are optional execution playbooks that help agents perform specialized work well.

Skills do not replace rules.

Rules define mandatory boundaries.
Memory defines what is true about the current project.
Skills define practical methods, patterns, and checklists for doing a specific type of work.

Rules override skills when they conflict.
Memory is project truth; skills are execution guidance.

## Current Status

Asset 4 - Core Development Skills: complete.  
Asset 5 - Initial Specialized Skills: complete.

Active skill sections:

- `core_development/`
- `ai_llm/`
- `product_docs/`
- `mcp_tooling/`

## How Skills Should Be Used

Do not load all skills by default.

Use skills only when the task needs specialized execution guidance.

Recommended flow:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> relevant rule files
-> memory/INDEX.md if project memory is needed
-> relevant memory files
-> skills/INDEX.md if execution playbook is useful
-> relevant skill section index
-> relevant skill file
-> task files
```

## Difference Between Rules, Memory, and Skills

Rules answer:

What must the agent obey?

Memory answers:

What is true about this specific project?

Skills answer:

How should the agent do this kind of work well?

## Current Skill Sections

### `core_development/`

Use for normal software development work:

- backend services
- frontend UI
- API design
- database migrations
- security review
- testing and verification
- deployment and operations

Read:

```text
core_development/INDEX.md
```

Activate when normal development work benefits from a practical implementation, review, or verification playbook.

### `ai_llm/`

Use for AI and LLM work:

- deterministic versus LLM decisions
- model cost optimization
- RAG and retrieval patterns
- source-grounded AI answers
- prompt/model/provider behavior

Read:

```text
ai_llm/INDEX.md
```

Activate when the task clearly involves AI provider behavior, LLM calls, RAG, retrieval, model choice, or AI output quality.

### `product_docs/`

Use for product and user-facing documentation:

- docs pages
- help center articles
- setup guides
- onboarding guides
- FAQs
- tutorials
- product explanations
- SEO blog posts when relevant

Read:

```text
product_docs/INDEX.md
```

Activate when the task is to write or improve product documentation, help content, guides, or user-facing explanations.

### `mcp_tooling/`

Use for MCP tooling:

- MCP server design
- MCP tools/resources/prompts
- controlled agent access to external systems
- diagnostic tooling for Supabase, Shopify, Google Cloud, logs, files, or services

Read:

```text
mcp_tooling/INDEX.md
```

Activate when MCP clearly improves reliability, inspection, diagnosis, or controlled tool access.

## Future Skill Sections

Future skill sections are optional only.

Do not create these sections unless Karim approves:

- automation
- Shopify / e-commerce
- content / marketing
- data / analytics
- media / video

## Activation Rule

Only activate a skill when:

- the task clearly matches the skill
- the rule files alone are not enough
- the agent needs implementation patterns or checklists
- the work is important enough to benefit from a playbook

Do not activate skills for tiny edits unless they reduce risk.

## ECC Source Strategy

ECC contains useful skill material.

Karim AI Lab should use ECC as source material, but not copy it blindly.

When converting ECC skills:

- Keep useful patterns.
- Remove generic noise.
- Remove duplicate rule content.
- Rewrite in Karim AI Lab style.
- Keep files practical and task-focused.
- Prefer shorter, clearer playbooks over long encyclopedic notes.

## Skill File Rules

Each skill file should include:

- purpose
- when to activate
- required read order
- execution pattern
- practical checklist
- stop conditions
- output expectations

Skill files should not include:

- generic coding theory
- long unrelated examples
- duplicate rules already covered in `core/` or `rules/`
- project-specific facts that belong in `memory/`
- future ideas that are not actionable

## New Skill Section Rule

Do not create new skill sections unless Karim approves.

If a new section is needed, propose it first and explain:

- why existing sections are not enough
- what files it would include
- when agents should activate it
