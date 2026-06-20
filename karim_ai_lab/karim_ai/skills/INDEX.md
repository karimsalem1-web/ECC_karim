# Skills Index

Purpose:
Skills are optional execution playbooks that help agents perform specialized work well.

Skills do not replace rules.

Rules define mandatory boundaries.
Memory defines what is true about the current project.
Skills define practical methods, patterns, and checklists for doing a specific type of work.

## Current Status

Asset 4 - Skills System is in progress.

Current active skill section:

```text
core_development/
```

Future skill sections may be added later for:

- AI / LLM / RAG
- automation
- Shopify / e-commerce
- content / research
- marketing
- data / analytics

Do not create future skill sections unless Karim approves.

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
