# Start Here

Read this file before any task.

Karim AI Lab is designed to help coding agents start from the same source of truth without loading unnecessary files.

## Required Start Flow

1. Read this file.
2. Read `INDEX.md`.
3. Identify the task type.
4. Load only the minimum relevant files.
5. Inspect existing project files before editing.
6. Plan before implementation.
7. Check approval gates.
8. Edit only the approved scope.
9. Update memory when relevant.
10. Summarize changes clearly.

## What to Read Next

After this file, read:

```text
INDEX.md
```

Then choose the correct layer:

- For operating behavior, read `core/INDEX.md`.
- For technical coding rules, read `rules/INDEX.md`.
- For project-specific memory, read `memory/INDEX.md`.
- For optional execution playbooks, read `skills/INDEX.md`.
- For workflows, read `workflows/INDEX.md`.
- For AI/LLM work, read `skills/ai_llm/INDEX.md`.
- For docs/help/guide work, read `skills/product_docs/INDEX.md`.
- For MCP work, read `skills/mcp_tooling/INDEX.md`.

Skills are optional execution playbooks, not mandatory rules.
Workflows are step-by-step task processes.
Use workflows only when the task needs a process.
Tiny safe edits may not need workflows.
Rules override skills when they conflict.
Rules override workflows when they conflict.
Skills provide expertise; workflows provide process.
Memory stores project truth.

## Token Optimization

Do not load all files by default.
Do not load all skills by default.

Use this pattern:

```text
START_HERE.md
-> INDEX.md
-> Relevant section index
-> Relevant specific file
-> Task files
```

Example for frontend work:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> rules/frontend.md
-> memory/INDEX.md if project memory is needed
-> memory/by_area/frontend.md if frontend memory is relevant
-> affected frontend files
```

Example for backend + database work:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> rules/backend.md
-> rules/database.md
-> memory/INDEX.md if project memory is needed
-> relevant memory/by_area files
-> affected backend/database files
```

Example for a core development skill:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> relevant rule files
-> memory/INDEX.md if project memory is needed
-> skills/INDEX.md
-> skills/core_development/INDEX.md
-> one relevant core development skill file
-> task files
```

AI/LLM task:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> relevant rule files
-> memory/INDEX.md if project context is needed
-> skills/INDEX.md
-> skills/ai_llm/INDEX.md
-> relevant AI/LLM skill file
```

Product docs task:

```text
START_HERE.md
-> INDEX.md
-> memory/INDEX.md
-> relevant memory files
-> skills/INDEX.md
-> skills/product_docs/INDEX.md
-> product_docs/user_docs_and_guides.md
```

MCP task:

```text
START_HERE.md
-> INDEX.md
-> rules/INDEX.md
-> rules/security.md
-> rules/environment.md
-> skills/INDEX.md
-> skills/mcp_tooling/INDEX.md
-> mcp_tooling/mcp_server_patterns.md
```

Planning / non-trivial implementation task:

```text
START_HERE.md
-> INDEX.md
-> workflows/INDEX.md
-> workflows/planning_workflow.md
-> rules/INDEX.md
-> relevant rule files
-> memory/INDEX.md if project context is needed
-> relevant memory files
-> skills/INDEX.md if execution playbook is useful
-> relevant skill files
-> affected project files
```

Build/debug task:

```text
START_HERE.md
-> INDEX.md
-> workflows/INDEX.md
-> workflows/build_fix_workflow.md
-> rules/INDEX.md
-> relevant rule files
-> skills/core_development/testing_verification.md
-> affected project files
```

Tiny safe edit:

```text
START_HERE.md
-> INDEX.md
-> relevant rule file if needed
-> affected file
```

## Do Not Assume

Do not assume missing:

- schemas
- routes
- APIs
- environment variables
- business rules
- project structure
- ownership models
- security policies
- customer flows
- billing or credit behavior
- project memory
- specialized skills

If unclear, ask Karim before editing.

## Current Asset Status

Asset 1 - Cross-agent entry strategy: complete.  
Asset 2 - Core rules layer: complete.  
Asset 3 - Project memory system skeleton: complete.  
Asset 4 - Core Development Skills: complete.  
Asset 5 - Initial Specialized Skills: complete.  
Asset 6 - Workflows: complete.

Future assets will add templates, ECC extraction/cleanup, final promotion/rename strategy, and optional future workflows only when needed.
