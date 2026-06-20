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
- For future skills, read the skills index once it exists.
- For future workflows, read the workflows index once it exists.

## Token Optimization

Do not load all files by default.

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

If unclear, ask Karim before editing.

## Current Asset Status

Asset 1 - Cross-agent entry strategy: complete.  
Asset 2 - Core rules layer: complete.  
Asset 3 - Project memory system skeleton: complete.

Future assets will add skills, workflows, templates, and ECC extraction/cleanup.
