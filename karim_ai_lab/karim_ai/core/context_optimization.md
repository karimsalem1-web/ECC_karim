# Context Optimization

Purpose:
This file defines how agents should reduce token waste, avoid context overload, and load only the information needed for the current task.

When to read:
- Read this file before broad analysis, architecture work, refactoring, debugging, or multi-file implementation.
- Always follow it when using Karim AI Lab in long sessions.

## Core Rule

Do not load everything by default.

Start small:
1. Read the adapter file for the current agent.
2. Read `karim_ai/START_HERE.md`.
3. Read `karim_ai/INDEX.md`.
4. Read the smallest set of files required for the task.

## Index-First Navigation

Agents must use index files before opening many files.

Preferred flow:
- Main task → `karim_ai/INDEX.md`
- Core behavior → `karim_ai/core/INDEX.md`
- Future rules → rules index
- Future skills → skills index
- Future memory → memory index

If an index exists, read the index before reading the folder contents.

## Minimum Context Loading

For each task, classify the request first:

- Entry/navigation task → read adapter files and `START_HERE.md`
- Planning task → read core protocol and relevant context
- Security task → read security rules only
- Database task → read database rules only
- Frontend task → read frontend rules only
- Backend/API task → read backend and API rules only
- MCP task → read MCP skill only
- Bugfix → read bugfix workflow plus affected files
- New feature → read feature workflow plus affected files

Do not open unrelated skills or rules.

## Long Context Protection

When the session becomes long:
- Summarize the current state before continuing.
- List decisions already made.
- List files already changed or planned.
- Avoid repeating old explanations.
- Ask Karim to confirm direction if the context becomes uncertain.

## Large Repo Behavior

For large repositories:
- Start with tree/listing.
- Open only likely relevant files.
- Search for exact names before reading full files.
- Prefer reading small sections over entire large documents.
- Do not inspect generated files, dependencies, build outputs, or unrelated docs unless required.

## Memory Usage

When the memory system exists:
- Read project memory before coding.
- Use memory to avoid asking Karim to repeat known facts.
- Update memory after meaningful changes.
- Keep memory concise and structured.

When memory does not exist yet:
- State important decisions in the final report so they can be added later.

## Output Efficiency

When reporting:
- Be clear and short.
- Use tables only when useful.
- Do not paste huge code blocks unless needed.
- Mention file paths precisely.
- Separate confirmed facts from assumptions.

## Context Failure Rule

If the agent is unsure because context is missing, stale, or too large, it must stop and ask Karim instead of guessing.
