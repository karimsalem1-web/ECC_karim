# Karim AI Lab Index

Purpose:
This is the main navigation file for Karim AI Lab.

Agents should read this after `START_HERE.md`.

Use this file to choose the correct guidance layer before working.

## Current Status

Asset 1 - Cross-agent entry strategy: complete.  
Asset 2 - Core rules layer: complete.

Future assets will add:

- project memory system
- skills system
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
```

## Planned Future Tree

The tree may evolve slightly according to the needs of each project.

Planned future sections:

```text
memory/
skills/
workflows/
context/
templates/
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

## Common Routing Examples

Frontend task:

```text
START_HERE.md -> INDEX.md -> rules/INDEX.md -> rules/frontend.md
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

## Important Restrictions

Do not modify original ECC files outside `karim_ai_lab` unless Karim explicitly approves it.

Do not create unnecessary files.

Do not load unnecessary context.

Do not assume missing schemas, routes, APIs, environment variables, or business rules.

If unclear, stop and ask Karim.
