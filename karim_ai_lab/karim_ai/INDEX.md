# Karim AI Lab Index

Purpose:
This is the main navigation file for Karim AI Lab.

Agents should read this after `START_HERE.md`.

Use this file to choose the correct guidance layer before working.

## Current Status

Asset 1 - Cross-agent entry strategy: complete.  
Asset 2 - Core rules layer: complete.  
Asset 3 - Project memory system skeleton: complete.  
Asset 4 - Core Development Skills: complete.

Future assets will add:

- specialized AI / RAG / automation / Shopify / content skills
- workflows
- templates
- ECC extraction and cleanup

## Main Files

### `LOCKED_MISSION.md`

The permanent mission and strategy of Karim AI Lab.

### `START_HERE.md`

The first file every agent should read before doing any task.

### `INDEX.md`

This file. Main navigation for the system.

## Core Operating Rules

Folder:

```text
core/
```

Read:

```text
core/INDEX.md
```

Use this layer for:

- executive laws
- approval gates
- working protocol
- context optimization
- quality standards

Core rules answer:

How should the agent behave?

## Technical Rules

Folder:

```text
rules/
```

Read:

```text
rules/INDEX.md
```

Use this layer for:

- architecture
- security
- database
- frontend
- backend
- API routes
- naming
- environment variables

Technical rules answer:

How should the agent code safely in this area?

## Project Memory

Folder:

```text
memory/
```

Read:

```text
memory/INDEX.md
```

Use this layer for:

- project-specific context
- stable summary
- completed work
- area-specific memory
- project-specific architecture notes
- project-specific database notes
- project-specific frontend/backend/API notes
- project-specific integrations, AI, and business logic notes

Memory answers:

What is true about this specific project?

Memory does not replace rules.

Rules tell the agent how to behave.
Memory tells the agent what happened or what is true in this project.

## Skills

Folder:

```text
skills/
```

Read:

```text
skills/INDEX.md
```

Use this layer for optional execution playbooks.

Skills are not mandatory rules.
Use skills only when the task benefits from specialized execution guidance.

Active skills section:

```text
skills/core_development/
```

The 7 active core development skills are:

- `backend_service_patterns.md`
- `frontend_ui_patterns.md`
- `api_design.md`
- `database_migrations.md`
- `security_review.md`
- `testing_verification.md`
- `deployment_ops.md`

Skills answer:

How should the agent do this kind of work well?

## Current Active Tree

```text
karim_ai/
  LOCKED_MISSION.md
  START_HERE.md
  INDEX.md

  core/
    INDEX.md
    executive_laws.md
    approval_gates.md
    working_protocol.md
    context_optimization.md
    quality_standards.md

  rules/
    INDEX.md
    architecture.md
    security.md
    database.md
    frontend.md
    backend.md
    api_routes.md
    naming.md
    environment.md

  memory/
    INDEX.md
    summary.md
    done.md

    by_area/
      architecture.md
      security.md
      database.md
      backend.md
      api_routes.md
      frontend.md
      environment.md
      integrations.md
      ai.md
      business_logic.md

  skills/
    INDEX.md

    core_development/
      INDEX.md
      backend_service_patterns.md
      frontend_ui_patterns.md
      api_design.md
      database_migrations.md
      security_review.md
      testing_verification.md
      deployment_ops.md
```

## Planned Future Tree

The tree may evolve slightly according to the needs of each project.

Planned future sections:

```text
workflows/
context/
templates/
```

Future specialized skill sections may include:

```text
AI / LLM / RAG
automation
Shopify / e-commerce
content / research
marketing
data / analytics
```

The tree is a working blueprint, not a prison.
Small structure changes are allowed when they improve clarity, reduce noise, or match the project situation.

## How Agents Should Choose Files

Do not load everything by default.

Use this pattern:

1. Read `START_HERE.md`.
2. Read this `INDEX.md`.
3. Decide whether the task needs core rules, technical rules, memory, skills, or workflows.
4. Read the relevant section index.
5. Read only the specific files needed for the task.
6. Inspect the actual project files.
7. Plan.
8. Check approval gates.
9. Implement only the allowed scope.
10. Update memory when relevant.

## Common Routing Examples

Frontend task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/frontend.md
```

Frontend task with project memory:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/frontend.md -> memory/INDEX.md -> memory/by_area/frontend.md
```

Backend task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/backend.md
```

Database task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/database.md
```

API route task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/api_routes.md
```

Security-sensitive task:

```text
START_HERE.md -> INDEX.md -> core/approval_gates.md -> rules/security.md -> relevant technical rule file
```

Architecture or refactor task:

```text
START_HERE.md -> INDEX.md -> core/approval_gates.md -> rules/architecture.md -> relevant technical rule file
```

Project memory task:

```text
START_HERE.md -> INDEX.md -> memory/INDEX.md -> relevant memory file
```

Skill-supported task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> relevant rule files -> skills/INDEX.md -> skills/core_development/INDEX.md -> one relevant skill file
```

Testing or verification task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> relevant rule files -> skills/INDEX.md -> skills/core_development/testing_verification.md
```

Deployment readiness task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/environment.md -> skills/INDEX.md -> skills/core_development/deployment_ops.md
```

## Important Restrictions

Do not modify original ECC files outside `karim_ai_lab` unless Karim explicitly approves it.

Do not create unnecessary files.

Do not load unnecessary context.

Do not load all skills by default.

Do not assume missing schemas, routes, APIs, environment variables, business rules, project memory, or specialized skills.

If unclear, stop and ask Karim.
