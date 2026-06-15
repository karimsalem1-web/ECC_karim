# Architecture Rules

Purpose:
Guide agents when designing or changing system architecture, folder structure, modules, boundaries, and data flow.

When to read:
- Read this file before creating a new feature, restructuring folders, changing data flow, adding services, or modifying major dependencies.
- Always read it before broad refactors or architecture-changing work.

## Core Architecture Principles

1. Plan architecture before implementation.
   Do not start coding until the proposed structure, affected files, data flow, and boundaries are clear.

2. Keep boundaries clean.
   Separate frontend, backend, database, automation, AI calls, and external integrations.

3. Keep sensitive logic server-side.
   Authentication checks, authorization, service-role database access, paid AI calls, webhooks, payment logic, credit logic, and private integrations must not run in frontend code.

4. Prefer modular structure.
   Each module should have a clear responsibility. Avoid mixing UI, data access, business logic, validation, and external API calls in one place.

5. Avoid premature complexity.
   Use the simplest architecture that solves the current problem safely and can grow later.

6. Avoid hidden coupling.
   Do not make one module depend on unrelated implementation details from another module.

7. Design for multi-project portability.
   Anything added to Karim AI Lab should be reusable unless clearly marked project-specific.

## Before Changing Architecture

The agent must identify:

- Current structure
- Proposed structure
- Files/folders affected
- Data flow before and after
- Risks
- Migration or compatibility impact
- Whether approval is required

If any of these are unclear, ask Karim before editing.

## Folder Structure Rules

- Use clear domain-based folders when the project grows.
- Keep shared utilities separate from feature-specific logic.
- Avoid dumping unrelated files into generic folders like `utils`, `helpers`, or `lib` without sub-organization.
- Do not move files only for style preferences unless the task is a refactor.
- Do not rename important folders without explaining downstream impact.

## Data Flow Rules

Preferred flow for protected operations:

Frontend
→ Backend/API/Server Function
→ Validation/Auth
→ Database or private service
→ Safe response to frontend

Avoid:

Frontend
→ Private webhook / paid API / service-role database / secret-bearing service

## Architecture Output Format

When proposing architecture, use this format:

- Goal
- Current structure
- Proposed structure
- Data flow
- Files to create/change
- Risks
- Approval gates
- Test/verification plan

## Stop Conditions

Stop and ask Karim before continuing if:
- The task changes the main technology stack.
- The task changes authentication or multi-tenant strategy.
- The task changes database ownership model.
- The task requires production migration.
- The task may break existing project behavior.
