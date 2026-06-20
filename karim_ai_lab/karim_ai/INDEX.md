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
Asset 5 - Initial Specialized Skills: complete.  
Asset 6 - Workflows: complete.

Future assets will add:

- templates
- ECC extraction and cleanup
- optional future specialized skills only when needed
- optional future workflows only when needed

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
Rules override skills when they conflict.
Memory is project truth; skills are execution guidance.

Active skill sections:

- `core_development/`
- `ai_llm/`
- `product_docs/`
- `mcp_tooling/`

Active specialized skill files:

AI / LLM:

- `ai_llm/deterministic_vs_llm.md`
- `ai_llm/llm_cost_optimization.md`
- `ai_llm/rag_retrieval_patterns.md`

Product Docs:

- `product_docs/user_docs_and_guides.md`

MCP Tooling:

- `mcp_tooling/mcp_server_patterns.md`

Skills answer:

How should the agent do this kind of work well?

## Workflows

Folder:

```text
workflows/
```

Read:

```text
workflows/INDEX.md
```

Use this layer only when the task needs a step-by-step process.

Active workflow files:

- `workflows/INDEX.md`
- `workflows/planning_workflow.md`
- `workflows/build_fix_workflow.md`

Workflows are not loaded for every task.
Tiny safe edits may not need a workflow.
Rules override workflows when they conflict.
Skills provide expertise; workflows provide process.
Memory stores project truth.

Workflows answer:

What process should the agent follow from start to finish for this task type?

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

    ai_llm/
      INDEX.md
      deterministic_vs_llm.md
      llm_cost_optimization.md
      rag_retrieval_patterns.md

    product_docs/
      INDEX.md
      user_docs_and_guides.md

    mcp_tooling/
      INDEX.md
      mcp_server_patterns.md

  workflows/
    INDEX.md
    planning_workflow.md
    build_fix_workflow.md
```

## Planned Future Tree

The tree may evolve slightly according to the needs of each project.

Planned future sections:

```text
templates/
```

Optional future specialized skill sections may be added only when needed.
Optional future workflows may be added only when needed.

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

Core development skill-supported task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> relevant rule files -> skills/INDEX.md -> skills/core_development/INDEX.md -> one relevant core development skill file
```

Planning workflow:

```text
START_HERE.md -> INDEX.md -> workflows/INDEX.md -> workflows/planning_workflow.md -> rules/INDEX.md -> relevant rule files -> memory/INDEX.md if project context is needed -> relevant memory files -> skills/INDEX.md if execution playbook is useful -> relevant skill files -> affected project files
```

Build fix workflow:

```text
START_HERE.md -> INDEX.md -> workflows/INDEX.md -> workflows/build_fix_workflow.md -> rules/INDEX.md -> relevant rule files -> skills/core_development/testing_verification.md -> affected project files
```

Tiny safe edit with no workflow:

```text
START_HERE.md -> INDEX.md -> relevant rule file if needed -> affected file
```

AI/LLM task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> relevant rule files -> memory/INDEX.md if needed -> skills/INDEX.md -> skills/ai_llm/INDEX.md -> relevant AI/LLM skill file
```

Product docs task:

```text
START_HERE.md -> INDEX.md -> memory/INDEX.md -> relevant memory files -> skills/INDEX.md -> skills/product_docs/INDEX.md -> skills/product_docs/user_docs_and_guides.md
```

MCP task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/security.md -> rules/environment.md -> skills/INDEX.md -> skills/mcp_tooling/INDEX.md -> skills/mcp_tooling/mcp_server_patterns.md
```

## Important Restrictions

Do not modify original ECC files outside `karim_ai_lab` unless Karim explicitly approves it.

Do not create unnecessary files.

Do not load unnecessary context.

Do not load all skills by default.

Do not assume missing schemas, routes, APIs, environment variables, business rules, project memory, or specialized skills.

If unclear, stop and ask Karim.
